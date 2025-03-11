# ARKO - Wprowadzenie

## Harmonogram ćwiczeń laboratoryjnych (kolejność chronologiczna)
1. Wprowadzenie do architektury Risc-V ([RiscvUppercase.asm](RiscvUppercase.asm), +rozdanie tematów projektów)
2. Sprawdzian z architektury Risc-V
3. Wprowadzenie do architektury Intel x86 (+konsultacje projektów Risc-V)
4. Oddanie projektów Risc-V
5. Sprawdzian z architektury Intel x86
6. Konsultacje projektów Intel x86
7. Oddanie projektów Intel x86

## Punktacja
Total | (/20pkt)
-|-
Sprawdzian Risc-V | 3pkt
Projekt Risc-V | 6pkt
Sprawdzian Intel-x86 | 3pkt
Projekt Intel-x86 | 6+2pkt

## Sprawdziany - Risc-V, Intel x86
Zadania na sprawdzianach polegają na implementacji programów przetwarzających łańcuchy znakowe wprowadzane przez użytkownika (podobnie jak przykłady przedstawione na zajęciach wprowadzających z obu architektur). Poziom skomplikowania może być większy - może być konieczna implementacja np.zagnieżdżonej pętli (pętla w pętli - jeden poziom zagnieżdżenia, nie więcej). Przykładowe zadanie do przećwiczenia w domu:
  - sortowanie znaków w łańcuchu znakowym (wg wartości kodów ASCII),
  - usuwanie cyfr z łańcucha znakowego (bez użycia bufora pomocniczego).

Dodatkowe informacje:
- oba sprawdziany są realizowane z użyciem tych samych środowisk co na zajęciach wprowadzających (RARS dla Risc-V, Makefile dla Intel x86),
- można korzystać z dokumentacji, notatek, kodu napisanego wcześniej oraz innych materiałów,
- w trakcie sprawdzianu obowiązuje zakaz współpracy z innymi osobami realizującymi przedmiot, 
- połowa punktów przydzielana jest za poprawne rozwiązanie problemu, a połowa za jakość tego rozwiązania, 
- niedziałające rozwiązanie nie jest oceniane, 
- zaliczenie sprawdzianu może wymagać obrony przedstawionego rozwiązania.

Risc-V:
  - znak nowej linii ('\n') należy traktować jako prawidłową część łańcucha znakowego.

## Projekt Risc-V
Wymagania dotyczące implementacji projektu Risc-V (ich nieprzestrzeganie skutkuje obniżeniem oceny):

- Dla tematów związanych z przetwarzneim obrazów, zapisanych w pliku [BMP](https://en.wikipedia.org/wiki/BMP_file_format) - plik z obrazem istnieje na dysku, program ma go otworzyć i przeprowadzić odpowiednie operacje. Plik zawiera dane wejściowe i/lub jest buforem na wynik (chyba, że w poleceniu napisano inaczej),
- Szerokość obrazu jest dowolna (w szczególności niepodzielna przez 4, obsługa "padding`u") - ustalana przez prowadzącego w momencie oddawania projektu. Można przyjąć sensowną wartość maksymalną,
- Pamięć na bitmapę należy zaalokować dynamicznie - w następujących krokach:
  - do statycznie zdefiniowanego obszaru pamięci wczytać nagłówek pliku BMP,
	- odczytać wysokość i szerokość bitmapy,
	- dynamicznie zaalokować (wywołanie systemowe sbrk) pamięć na tablicę pikseli,
	- wczytać tablicę pikseli do dynamicznie zaalokowanego obszaru pamięci,
- W zależności od wybranego tematu, należy wczytać parametry od użytkownika (np. z konsoli),
- Obowiązuje zakaz używania typów i operacji zmiennoprzecinkowych (float/double/..),
- Do obliczeń na ułamkach należy używać operacji stałoprzecinkowych (fixed-point). Format należy dobrać w sposób optymalny, adekwatnie do rozwiazywanego problemu. Obowiązuje zakaz wykonywania obliczeń ułamkowych w potędze liczby 10 (np. w dziedzinie 1/10000),
- Działania operujące na wartościach związanych z potegami liczby 2 należy implementować z użyciem operacji bitowych:
  - Mnożenie: M * (2^N) => M << N,
  - Dzielenie: M / (2^N) => M >> N,
  - Modulo: M % (2^N) => M & (2^N - 1) - lub maska bitowa z N-jedynkami, e.g. 7 % 4 = 7 % 2^2 = 7 & (2^2 - 1) = 7 & 0x03 = 0b0111 & 0b0011 = 0b0011
- Należy zaplanować i zademonstrować poprawność działania zaimplementowanego algorytmu. Włącznie z przypadkami brzegowymi. Założenia projektowe mogą ograniczać stopień skomplikowania projektu, ale muszą być przyjęte świadomie (ze zrozumieniem konsekwencji pozytywnych/negatywnych),
- Należy podjąć rozsądne decyzje projektowe dotyczące szczegółów nie wynikających wprost z polecenia,
- Kod powiniem być sformatowany wg nastpującej reguły: etykiety - bez wcięcia, kod - jedno wcięcie,
- Należy minimalizować ilość wywołań systemowych - wczytywać/zapisywać cały wiersz pikseli z/do pliku, zamiast każdego piksela osobno, lub cały obraz, zamiast każdego wiersza osobno, etc,
- Należy optymalizować czas wykonania poprzez realizację obliczeń w pamięci (np. przetwarzanie całego obrazu zawartego w pamięci, a nie wczytywanie go linia po linii z dysku),
- Należy minimalizować ilość dostępów do pamięci poprzez sięganie po wiele bajtów jednocześnie - używać instrukcji lw/sw, zamiast lb/sb. Należy pamiętać o wyrównaniu dostępów do pamięci,

- Projekt podlega obronie w momencie oddania. 

## Projekt Intel-x86
Wymagania dotyczące implementacji projektu Intel-x86 (ich nieprzestrzeganie skutkuje obniżeniem oceny):
- Projekt powinien być programem hybrydowym - część napisana w języku C/C++, a część w asemblerze,
- Kod C/C++ jest niezbędny do uzyskania pozytywnej oceny i musi działać, ale jego jakość nie jest oceniana,

Część C/C++:
- Program powinien być interaktywny - w trakcie działania programu użytkownik powinien mieć możliwość zmiany parametrów działania algorytmu (bez wyłączania programu, np. użycie myszki - do przybliżenia/oddalania - lub klawiszy na klawiaturze) i obserwacji wyniku działania,
- Program powinien korzystać z dowolnej biblioteki graficznej (Allegro, OpenGL etc.) do obsługi interakcji z użytkownikiem (klawiatura/mysz), wczytywania (jeżeli konieczne) i wyświetlania grafiki, zapisywanie wyniku w pliku nie jest konieczne,
- Projekt NIE musi operować na plikach BMP, rekomendowane jest użycie typów do obsługi grafiki zapewnianych przez wybraną bibliotekę graficzną,
- Alokacja zasobów powinna odbyć się na poziomie C/C++ - do funkcji asemblerowej powinny być przekazywane dane wejściowe oraz już zaalokowane bufory na dane wyjściowe,
- Przykładowy prototyp funkcji asemblerowej:

void f(int width, int height, char *pInputImg, char *pOutputImg, int algoSpecificParam0, float algoSpecificParam1, ...);

- Przykładowa implementacja części wysokopoziomowej w pseudokodzie:
 
````{verbatim}
allocateBuffers();
readDataFromFiles(); // if required
setParamsToDefaultValues();
while(true) {
	f(...); // call assembly function
	displayResult();
	readUserInput(); // this function shall block and wait for user interaction
	modifyParams(); // according to user input
}
````

Część Assembly:
- Część asemblerowa powinna być jedną funkcją,
- Pojedyncze wywołanie funkcji asemblerowej powinno realizować kompletny wynik działania algorytmu (np. renderować cały obraz, a nie pojedyncze piksele/wiersze), 
- Kod asemblerowy powinien używać liczb zmiennoprzecinkowych (float lub double),
- Można (nie trzeba) korzystać z jednostek wektorowych,
- Implementacja wersji 64b jest oceniana w skali do 8p (implementacja wersji 32b nie jest konieczna),
