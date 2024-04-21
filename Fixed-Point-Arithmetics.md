# Arytmetyka stałoprzecinkowa (Work in progress)

## Problem
Jakiego typu zmiennych należy użyć, aby rozwiązać następujący problem:
````{verbatim}
3.0 + 1.5 = ?
````

Pierwszym wyborem będzie oczywiście typ <i>zmiennoprzecinkowy</i> (float/double), który natywnie (w sposób naturalny dla procesora) zapewnia obsługę liczb ułamkowych. Niestety, nie zawsze można go użyć. Wiele (w szczególności mniejszych) architektur może nie posiadać jednostki zmiennopozycyjnej (jest to dodatkowy krzem, który kosztuje) i arytmetyka zmiennoprzecinkowa może być tam (co najwyżej) emulowana software\`owo (a to trwa). 
Alternatywnym rozwiązaniem jest użycie typu <i>całkowitoliczbowego</i> (integer). Mniej oczywistym, gdyż przenosi część odpowiedzialności na programistę. Procesor nie posiada kompletu informacji na temat rozwiązywanego problemu - w szczególności - nie wie o istnieniu kropki dziesiętnej:
````{verbatim}
30 + 15 = ? 
````

Rozwiązanie problemu składa się z trzech etapów:
1. Przed wprowadzeniem danych do algorytmu programista/użytkownik musi usunąć kropkę i wyrazić wszystkie wartości w postaci liczb całkowitych,
2. Algorytm, operujący wyłącznie na liczbach całkowitych, wykonuje obliczenia,
3. Wynik, otrzymany również w postaci wartości całkowitej, musi zostać zinterpretowany w odpowiedni sposób - z uwzględnieniem kropki w odpowiednim miejscu,
 
````{verbatim}
// Etap 1 (wprowadzanie danych):
uint8_t a = 30, b = 15; // 3.0 -> 30, 4.5 -> 45

// Etap 2 (obliczenia - tylko wartości całkowite):
uint8_t c = a + b; // c == 45

// Etap 3 (interpretacja wyniku):
// 45 -> 4.5
````

W powyższym przykładzie przyjęlismy niejawnie, że wartości na których operujemy, składają się z dwóch cyfr dziesiętnych. Jednej cyfry całkowitej i jednej ułamkowej. Taki przykładowy format nazywamy <b>stałoprzecinkowym</b>, gdyż przecinek  zawsze wystepuje w tym samym miejscu. 
Niewątpliwą zaletą takiego rozwiązania jest możliwość implementacji obliczeń przy użyciu wyłącznie liczb całkowitych. Dla procesora je wykonującego jest istotna tylko sumaryczna ilość użytych znaków, a podział na całkowite vs ułamkowe nie ma znaczenia.  Oczywiście, procesor nie operuje na znakach dziesiętnych, a na bitach. 

W dalszej części omówimy jak tego typu obliczenia zaimplementować w systemie <b>stałoprzecinkowym dwójkowym</b> - naturalnym dla procesora. 

> [!WARNING]
> ### Dlaczego użycie systemu o podstawie 10 będzie błędem?
> W powyższym przykładzie używamy wartości z zakresu [0.0, 9.9] - lub [0.0, 10.0). Są one zmapowane na wartości całkowite [0, 99] - lub [0, 100). Jednocześnie zakres użytego typu całkowitego jest znacznie szerszy - [0, 255] - lub [0, 256). Widzimy więc, że ponad połowa (~61%) dostępnego zakresu oferowanego przez typ całkowity pozostanie niewykorzystana - nieco ponad 1 bit nie zostanie w ogóle użyty. 
> 
> Tę niewykorzystaną część zakresu można użyć, aby poszerzyć zakres wartości możliwych do wyrażenia (ponad dwukrotnie = o ponad 1 bit), lub zwiększyć precyzję obliczeń (ponad dwukrotnie = o ponad 1 bit). Z drugiej strony, gdybyśmy uznali, że ani szerszy zakres, ani większa precyzja obliczeń nie są w danym przypadku potrzebne, to należy zapytać dlaczego nie użyliśmy procesora o krótszym słowie (np. procesor 16-bitowy, zamiast 32-bitowego), co w efekcie obniżyłoby całościowy koszt rozwiązania. 
> 
> Mając do dyspozycji określoną liczbę bitów (zasobów) należy maksymalizować ich wykorzysyanie. W tym kontekscie, użycie systemu o podstawie 10, może być bardzo trudne do obronienia. W dalszej części materiału zostanie pokazane jak zaprojektować stałoprzecinkowy system binarny, który optymalnie wykorzysta dostępne zasoby.

## System pozycyjny
Na początek uogólnijmy działanie systemu pozycyjnego na przykładzie systemu dziesiętnego. 

Liczba składa sie z cyfr. To jaką wartość każda z cyfr wnosi do liczby, wynika z tego jaka to cyfra <b>oraz</b> na jakiej pozycji w liczbie się znajduje - dokładniej - z iloczynu cyfry oraz wagi pozycji. Waga każdej pozycji w liczbie jest wynikiem podniesienia podstawy systemu (10 dla dziesiętnego) do potęgi odpowiadającej indeksowi pozycji. Indeksem pozycji jest odległość od najmłodszej cyfry całkowitej (dodatnia w lewo, ujemna w prawo). Poniższa tabela obrazuje to na przykładzie:
<table>
<tr><td>Podstawa systemu</td><td colspan=3>10</td></tr>
<tr><td>Liczba</td><td colspan=3>127<sub>10</sub></td></tr>
<tr><td>Cyfra</td><td>1</td><td>2</td><td>7</td></tr>
<tr><td>Indeks pozycji</td><td><b>2</b></td><td><b>1</b></td><td><b>0</b></td></tr>
<tr><td>Waga pozycji</td><td>10<sup><b>2</b></sup> = 100</td><td>10<sup><b>1</b></sup> = 10</td><td>10<sup><b>0</b></sup> = 1</td></tr>
<tr><td>Wartość wnoszona</td><td>1 &middot; 100 = 100</td><td>2 &middot; 10 = 20</td><td>7 &middot; 1 = 7</td></tr>
<tr><td>Wartość liczby</td><td colspan=3>100 + 20 + 7 = 127<sub>10</sub></td></tr>
</table>

W przypadku liczb niecałkowitych indeksy pozycji mogą być ujemne, co z kolei wpływa na wagi poszczególnych pozycji:
<table>
<tr><td>Podstawa systemu</td><td colspan=3>10</td></tr>
<tr><td>Liczba</td><td colspan=3>12.7<sub>10</sub></td></tr>
<tr><td>Cyfra</td><td>1</td><td>2</td><td>7</td></tr>
<tr><td>Indeks pozycji</td><td><b>1</b></td><td><b>0</b></td><td><b>-1</b></td></tr>
<tr><td>Waga pozycji</td><td>10<sup><b>1</b></sup> = 10</td><td>10<sup><b>0</b></sup> = 1</td><td>10<sup><b>-1</b></sup> = 0.1</td></tr>
<tr><td>Wartość wnoszona</td><td>1 &middot; 10 = 10</td><td>2 &middot; 1 = 2</td><td>7 &middot; 0.1 = 0.7</td></tr>
<tr><td>Wartość liczby</td><td colspan=3>10 + 2 + 0.7 = 12.7<sub>10</sub></td></tr>
</table>

Zasada ta obowiązuje niezależnie od podstawy systemu. W związku z tym analogicznie dla systemu dwójkowego możemy zdefiniować metodę na określenie wartości liczb całkowitych:
<table>
<tr><td>Podstawa systemu</td><td colspan=4>2</td></tr>
<tr><td>Liczba</td><td colspan=4>1101<sub>2</sub></td></tr>
<tr><td>Cyfra</td><td>1</td><td>1</td><td>0</td><td>1</td></tr>
<tr><td>Indeks pozycji</td><td><b>3</b></td><td><b>2</b></td><td><b>1</b></td><td><b>0</b></td></tr>
<tr><td>Waga pozycji</td><td>2<sup><b>3</b></sup> = 8</td><td>2<sup><b>2</b></sup> = 4</td><td>2<sup><b>1</b></sup> = 2</td><td>2<sup><b>0</b></sup> = 1</td></tr>
<tr><td>Wartość wnoszona</td><td>1 &middot; 8 = 8</td><td>1 &middot; 4 = 4</td><td>0 &middot; 2 = 0</td><td>1 &middot; 1 = 1</td></tr>
<tr><td>Wartość liczby</td><td colspan=4>8 + 4 + 0 + 1 = 13<sub>10</sub></td></tr>
</table>

oraz ułamkowych:
<table>
<tr><td>Podstawa systemu</td><td colspan=4>2</td></tr>
<tr><td>Liczba</td><td colspan=4>11.01<sub>2</sub></td></tr>
<tr><td>Cyfra</td><td>1</td><td>1</td><td>0</td><td>1</td></tr>
<tr><td>Indeks pozycji</td><td><b>1</b></td><td><b>0</b></td><td><b>-1</b></td><td><b>-2</b></td></tr>
<tr><td>Waga pozycji</td><td>2<sup><b>1</b></sup> = 2</td><td>2<sup><b>0</b></sup> = 1</td><td>2<sup><b>-1</b></sup> = 0.5</td><td>2<sup><b>-2</b></sup> = 0.25</td></tr>
<tr><td>Wartość wnoszona</td><td>1 &middot; 2 = 2</td><td>1 &middot; 1 = 1</td><td>0 &middot; 0.5 = 0</td><td>1 &middot; 0.25 = 0.25</td></tr>
<tr><td>Wartość liczby</td><td colspan=4>2 + 1 + 0 + 0.25 = 3.25<sub>10</sub></td></tr>
</table>

Analogicznie jak dla wartości całkowitych, jeżeli zachodzi potrzeba posługiwania się liczbami ujemnymi, to możemy zastosować kodowanie U2, gdzie waga najstarszego bitu będzie ujemna. Pozwoli to na użycie natywnie wspieranych operacji na liczbach całkowitych ze znakiem, nawet dla wartości z "wirtualnym" przecinkiem - ułamkowych.

## Wybór prawidłowego formatu
System komputerowy, w którym będziemy implementowali arytmetykę stałoprzecinkową zwykle będzie charakteryzował się konkretną wielkością słowa procesora. Często będzie to 32b, lub 64b, ale mogą się zdarzyć także inne długości np. 8b, 16b, 128b - mniejsze mikrokontrolery, procesory sygnałowe, lub jednostki wektorowe. W specyficznych przypadkach autor rozwiązania może wręcz mieć kontrolę nad tym jak długie będzie używane słowo binarne - logika programowalna (FPGA), lub dedykowany hardware (ASIC). Zawsze przed wyborem konkretnego formatu należy ustalić szerokość słowa na którym będą prowadzone operacje. Warto zwrócić uwagę na to, że gdy używamy systemu dziesiętnego na kartce papieru, ilość znaków przed i po przecinku jest nieograniczona - w systemie komputerowym już tak nie jest. 

Na potrzeby dalszych rozważań przyjmijmy słowo procesora o długości N=32b. W kolejnym kroku należy okreslić ile z tych bitów przeznaczymy na część całkowitą, a ile na ułamkową. Konsekwencją tego wyboru będą dwa parametry - <b>zakres</b> wartości możliwy do wyrażenia przy pomocy wynikowego formatu (czyli maksimum dla liczb bez znaku, lub minimum oraz maksimum dla liczb ze znakiem) <b>oraz</b> jego <b>rozdzielczość</b> (najmniejsza wartość o jaką moga się różnić dwie liczby). Przy ustalaniu tego podziału należy kierować się dwiema zasadami w nastepującej kolejności:
1. Przeznaczyć na część całkowitą tyle bitów (ale nie więcej), aby zapewnić możliwość wyrażenia wszystkich wartości z zakresu wymaganego przez dziedzinę problemu - wartości wejściowych, wyniku/wyników oraz wszystkich wyników pośrednich. 
2. Zmaksymalizować ilość bitów ułamkowych, aby zwiększyć precyzję obliczeń.


<table>
<tr><td>Podstawa systemu</td><td colspan=4>2</td></tr>
<tr><td>Liczba</td><td colspan=4>11.01<sub>2</sub></td></tr>
<tr><td>Cyfra</td><td>1</td><td>1</td><td>0</td><td>1</td></tr>
<tr><td>Indeks pozycji</td><td><b>1</b></td><td><b>0</b></td><td><b>-1</b></td><td><b>-2</b></td></tr>
<tr><td>Waga pozycji</td><td>2<sup><b>1</b></sup> = 2</td><td>2<sup><b>0</b></sup> = 1</td><td>2<sup><b>-1</b></sup> = 0.5</td><td>2<sup><b>-2</b></sup> = 0.25</td></tr>
<tr><td>Wartość wnoszona</td><td>1 &middot; 2 = 2</td><td>1 &middot; 1 = 1</td><td>0 &middot; 0.5 = 0</td><td>1 &middot; 0.25 = 0.25</td></tr>
<tr><td>Wartość liczby</td><td colspan=4>2 + 1 + 0 + 0.25 = 3.25<sub>10</sub></td></tr>
</table>

przykłady

Dla ostatniego przykładu - wartości binarnej ułamkowej - przyjmijmy, że przecinka nie musimy zawsze umieszczać w zapisie liczby, a jedynie umawiamy się że on tam występuje. Dodatkowo w systemie komputerowym zawsze będize istnieć dokładna ilość bitów na jakich można zapisywać wartości liczbowe - odpowiadająca wielkością słowu procesora - zwykle 32, lub 64 bity, ale może zdażyć się też np. 8b. 

W takim kontekście liczbami o <B>formacie stałoprzecinkowym</B> będziemy nazywać liczby o konkretnej szerokości (np. 32b), dla których przyjmujemy występowanie przecinka w konkretnym miejscu (np. za 20 najmłodszymi bitami).


Dobór odpowiedniej ilości bitów
zakres i precyzja
całkowitych vs ułamkowych

Zachowanie formatu w trakcie obliczeń
- dodawanie
- mnozenie
 - utrata precyzji

Format zmiennoprzecinkowy - zmiana precyzji w zależności od wartości, 
rozny wynik w zaleznosci od kolejności wykonywania obliczen, sortowanie


