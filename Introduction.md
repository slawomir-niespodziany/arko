# ARKO - Wprowadzenie

## Harmonogram ćwiczeń laboratoryjnych

Brak wyszczególnionej godziny oznacza (pełny) dwugodzinny termin. Terminy kolokwiów są podane w następnej tabeli:

Ćwiczenie | Gr. 102 | Gr. 103 | Gr. 107 | Gr. 108
-|-|-|-|-
Risc-V - Wprowadzenie, rozdanie projektów | 19.03 | 19.03 | 12.03 | 12.03
Risc-V - Sprawdzian | 26.03 | 26.03 | 16.04 | 16.04
Intel x86 - Wprowadzenie **&** Risc-V - Konsultacje | 23.04 | 23.04 | 7.05 | 7.05
Intel x86 - Sprawdzian	| 14.05 | 14.05 | | 
Risc-V - Termin oddania projektów, Konsultacje	| 21.05 @ **17<sup>15</sup>** | 21.05 @ **19<sup>15</sup>** | 21.05 @ **16<sup>15</sup>** | 21.05 @ **18<sup>15</sup>**
Intel x86 - Sprawdzian, Konsultacje	| | | 28.05 | 28.05
Intel x86 - Termin oddania projektów | 4.06 @ **17<sup>15</sup>** | 4.06 @ **19<sup>15</sup>** | 4.06 @ **16<sup>15</sup>** | 4.06 @ **18<sup>15</sup>**

Kolokwia odbywają sie zgodnie z harmonogramem przedstawionym na wykładzie:

Kolokwium | 102 | 103 | 107 | 108
-|-|-|-|-
Kolokwium 1 | 9.04 @ **17<sup>15</sup>** | 9.04 @ **19<sup>15</sup>** | 9.04 @ **16<sup>15</sup>** | 9.04 @ **18<sup>15</sup>**
Kolokwium 2 | 11.06 @ **17<sup>15</sup>** | 11.06 @ **19<sup>15</sup>** | 11.06 @ **16<sup>15</sup>** | 11.06 @ **18<sup>15</sup>**

## Punktacja
Total | (/20pkt)
-|-
Sprawdzian Risc-V | 3pkt
Projekt Risc-V | 6pkt
Sprawdzian Intel-x86 | 3pkt
Projekt Intel-x86 | 6+2pkt


## Sprawdziany - Risc-V, Intel x86
Zadania na sprawdzianach będą polegać na implementacji programów przetwarzających łańcuchy znakowe wprowadzane przez użytkownika (podobnie jak przykłady przedstawione na zajęciach wprowadzających). Poziom skomplikowania może być większy - może być konieczna implementacja zagnieżdżonej pętli (pętla w pętli - nie więcej). Przykładowe zadanie do przećwiczenia w domu:
  - sortowanie znaków w łańcuchu znakowym (wg wartości kodów ASCII),
  - usuwanie cyfr z łańcucha znakowego.

Dodatkowe informacje:
- oba sprawdziany są realizowane z użyciem tych samych środowisk co na zajęciach wprowadzających (symulator RARS dla Risc-V oraz makefile dla Intel x86),
- można korzystać z dokumentacji, notatek, kodu napisanego wcześniej oraz innych materiałów,
- w trakcie sprawdzianu obowiązuje zakaz współpracy z innymi osobami realizującymi przedmiot, 
- połowa punktów przydzielana jest za poprawne rozwiązanie problemu, a połowa za jakość tego rozwiązania, 
- niedziałające rozwiązanie nie jest oceniane,
- zaliczenie sprawdzianu może wymagać obrony przedstawionego rozwiązania.

Risc-V:
  - znak nowej linii ('\n') należy traktować jako prawidłową część łańcucha znakowego.

## Projekt Risc-V
Wymagania dotyczące implementacji projektu Risc-V (ich nieprzestrzeganie będzie skutkować obniżeniem oceny):

- Dla projektów operujacych na obrazach:
  - obrazy powinny być wczytywane z dysku i zapisywane na dysk w formacie [BMP](https://en.wikipedia.org/wiki/BMP_file_format),
  - dla wczytywanych bitmap należy obsługiwać padding (obrazy o szerokości niepodzielnej przez 4 powinny być obsługowane),
  - dla projektów generujących grafikę - program powinien otwierać istniejący plik BMP (o dowolnej szerokości) i na nim rysować wynik działania,
  - dla żadnego projektu nie należy tworzyć nowego pliku BMP na poziomie assemblera,
  - operacje na bitmapie należy wykonywać w pamięci,
  - operację wczytywania obrazu należy wykonać w następujących krokach:
    - do statycznie zdefiniowanego obszaru pamięci wczytać nagłówek pliku BMP,
	- odczytać wysokość i szerokość bitmapy,
	- dynamicznie zaalokować (wywołanie systemowe sbrk) pamięć na tablicę pikseli,
	- wczytać tablicę pikseli do dynamicznie zaalokowanej przestrzeni,
  
- Obowiązuje zakaz używania typów zmiennoprzecinkowych. W przypadku konieczności wykonywania obliczeń ułamkowych należy używać typów stałoprzecinkowych ([Fixed-Point](Fixed-Point-Arithmetics.md)),
- Użyty typ fixed-point powinien mieć tak dobraną ilośc bitów całkowitych i ułamkowych, aby maksymalizować dokładność obliczeń, jednocześnie nie dopuszczając do przepełnienia,
- Obowiązuje zakaz wykonywania obliczeń ułamkowych z użyciem podstawy dziesiętnej (np. w dziedzinie 1/10000),

- Działania operujące na wartościach związanych z potegami liczby 2 należy implementować z użyciem operacji bitowych:
  - Mnożenie: M * (2^N) => M << N,
  - Dzielenie: M / (2^N) => M >> N,
  - Modulo: M % (2^N) => M & (2^N - 1) - lub maska bitowa z N-jedynkami, e.g. 7 % 4 = 7 % 2^2 = 7 & (2^2 - 1) = 7 & 0x03 = 0b0111 & 0b0011 = 0b0011

- Kod powiniem być sformatowany wg następujących reguł: etykiety - bez wcięcia, kod - jedno wcięcie,

- Należy minimalizować ilość wywołań systemowych - wczytywać/zapisywać cały wiersz pikseli z/do pliku, zamiast każdego piksela osobno, lub cały obraz, zamiast każdego wiersza osobno, etc.
- Należy minimalizować ilość dostępów do pamięci poprzez sięganie po wiele bajtów jednocześnie - używać instrukcji lw/sw, zamiast lb/sb. Należy pamiętać o wyrównaniu dostępów do pamięci.

- Projekt podlega obronie w momencie oddania. 

## Projekt Intel-x86
Wymagania dotyczące implementacji projektu Intel-x86 (ich nieprzestrzeganie będzie skutkować obniżeniem oceny):
- Projekt powinien być programem hybrydowym - część napisana w języku C/C++, a część w asemblerze,
- Kod C/C++ jest niezbędny do uzyskania pozytywnej oceny i musi działać, ale jego jakość nie jest oceniana,
- Projekt NIE musi operować na plikach BMP,

Część C/C++:
- Program powinien być interaktywny - w trakcie działania programu użytkownik powinien mieć możliwość zmiany parametrów algorytmu (np. użycie myszki i do przybliżenia/oddalania, lub klawiszy na klawiaturze do zmiany parametrów) i obserwacji wyniku działania,
- Program powinien korzystać z dowolnej biblioteki graficznej (Allegro, OpenGL etc.) do obsługi interakcji z użytkownikiem (klawiatura/mysz), wczytywania (jeżeli konieczne) i wyświetlania grafiki, zapisywanie wyniku w pliku nie jest konieczne,
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
- Pojedyncze wywołanie funkcji asemblerowej powinno realizować kompletny wynik działania algorytmu (np. renderować cały obraz, a nie pojedyncze piksele), 
- Kod asemblerowy powinien używać liczb zmiennoprzecinkowych (float lub double),
- Można (nie trzeba) korzystać z jednostek wektorowych,
