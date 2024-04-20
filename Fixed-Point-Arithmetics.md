System pozycyjny
Na początek uogólnijmy działanie systemu pozycyjnego na przykładzie systemu dziesiętnego. Liczba skáda sie z cyfr. To jaką wartość każda z cyfr wnosi do liczby, wynika (a) z tego jaka to cyfra oraz (b) na jakiej pozycji w liczbie się znajduje. Wartość wnoszona przez cyfrę do sumarycznej wartości liczby jest iloczynem tej cyfry oraz wagi pozycji na której się znajduje. Waga każdej z pozycji jest z kolei wynikiem podniesienia do potęgi podstawy systemu (10 dla dziesiętnego) do potęgi odpowiadającej indeksowi pozycji. Indeksem pozycji jest odległość od najmłodszej cyfry całkowitej (dodatnia w lewo, ujemna w prawo). Poniższa tabela obrazuje to na przykładzie:

<table>
<tr><td>Podstawa systemu</td><td colspan=3><center>10</center></td></tr>
<tr><td>Liczba</td><td colspan=3><center><b>127<sub>10</sub></b></center></td></tr>
<tr><td>Cyfra</td><td><center>1</center></td><td><center>2</center></td><td><center>7</center></td></tr>
<tr><td>Indeks cyfry</td><td><center>2</center></td><td><center>1</center></td><td><center>0</center></td></tr>
<tr><td rowspan=2>Waga cyfry</td><td><center>10<sup>2</sup></center></td><td><center>10<sup>1</sup></center></td><td><center>10<sup>0</sup></center></td></tr>
<tr><td><center>100</center></td><td><center>10</center></td><td><center>1</center></td></tr>
<tr><td rowspan=2>Wartość cyfry</td><td><center>1 &middot; 100</center></td><td><center>2 &middot; 10</center></td><td><center>7 &middot; 1</center></td></tr>
<tr><td><center>100</center></td><td><center>20</center></td><td><center>7</center></td></tr>
<tr><td>Wartość liczby</td><td colspan=3><center>100 + 20 + 7 =<b> 127<sub>10</sub></b></center></td></tr>
</table>

W przypadku liczb ułamkowych pozycje cyfr liczone od najmłodszej cyfry całkowitej mogą być ujemne, co z kolei wpływa na wagi poszczególnych cyfr:

Podstawa systemu	10
Liczba 				10.7(10)
Cyfra 			1 		0 		7
Pozycja cyfry 	1 		0 		-1 (BOLD)
Waga cyfry		10^1 	10^0 	10^(-1)
				10		1		0.1
Wartość cyfry	10		0		0.7
Suma			10 +	0 + 	0.7 = 10.7

Zasada ta obowiązuje niezależnie od podstawy systemu. W związku z tym analogicznie dla systemu binarngo możemy zdefiniować metodę na określenie wartości liczb całkowitych:

Podstawa systemu	2
Liczba 				1101(2)
Cyfra 			1 		1 		0		1
Pozycja cyfry 	3		2 		1 		0
Waga cyfry		2^3 	2^2 	2^1		2^0
				8		4		2		1
Wartość cyfry	8		4		0		1
Suma			8 +		4 + 	0 + 	1 = 13(10)

oraz ułamkowych:

Podstawa systemu	2
Liczba 				10.11(2)
Cyfra 			1 		1 		0		1
Pozycja cyfry 	1		0 		-1 		-2 (BOLD)
Waga cyfry		2^1 	2^0 	2^(-1)	2^(-2)
				2		1		0.5		0.25
Wartość cyfry	2		1		0		0.25
Suma			2 +		1 + 	0 + 	0.25 = 3.25(10)

Dla ostatniego przykładu - wartości binarnej ułamkowej - przyjmijmy, że przecinka nie musimy zawsze umieszczać w zapisie liczby, a jedynie umawiamy się że on tam występuje. Dodatkowo w systemie komputerowym zawsze będize istnieć dokładna ilość bitów na jakich można zapisywać wartości liczbowe - odpowiadająca wielkością słowu procesora - zwykle 32, lub 64 bity, ale może zdażyć się też np. 8b. 

W takim kontekście liczbami o <B>formacie stałoprzecinkowym</B> będziemy nazywać liczby o konkretnej szerokości (np. 32b), dla których przyjmujemy występowanie przecinka w konkretnym miejscu (np. za 20 najmłodszymi bitami).

