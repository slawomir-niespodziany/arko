# ARKO - Wprowadzenie

## Plan zajęć
- Lab 1 - Wprowadzenie do architektury Risc-V, Rozdanie tematów projektów Risc-V,
- Lab 2 - Kolokwium Risc-V,
- Lab 3 - Wprowadzenie do architektury Intel-x86, Konsultacje projektów Risc-V
- Lab 4 - Termin oddania projektów Risc-V, Rozdanie tematów projektów Intel-x86,
- Lab 5 - Kolokwium Intel x86, Konsultacje Intel-x86,
- Lab 6 - Termin oddania projektów Intel-x86.


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
- można korzystać z dokumentacji, notatek, kodu napisanego wcześniej przez siebie oraz innych materiałów,
- obowiązuje zakaz współpracy z innymi osobami realizującymi przedmiot, 
- połowa punktów przydzielana jest za poprawne rozwiązanie problemu, a połowa za jakość tego rozwiązania, 
- niedziałające rozwiązanie nie jest oceniane,
- zaliczenie sprawdzianu może wymagać obrony przedstawionego rozwiązania.


## Projekt Risc-V
Wymagania dotyczące implementacji projektu Risc-V (ich nieprzestrzeganie będzie skutkować obniżeniem oceny):

- Dla projektów operujacych na obrazach:
  - obrazy powinny być wczytywane z dysku i zapisywane na dysk w formacie [BMP](https://en.wikipedia.org/wiki/BMP_file_format),
  - dla wczytywanych bitmap należy obsłużyć padding (szerokość może byc niepodzielna przez 4),
  - dla projektów generujących grafikę - program powinien być w stanie wygenerować bitmapy o dowolnej szerokości (również niepodzielnej przez 4),
  
- Obowiązuje zakaz używania typów zmiennopozycyjnych. W przypadku konieczności wykonywania obliczeń ułamkowych należy używać typów stałoprzecinkowych ([Fixed-Point](Fixed-Point-Arithmetics.md)),
- Użyty typ fixed-point powinien mieć tak dobraną ilośc bitów całkowitych i ułamkowych, aby maksymalizować dokładność obliczeń, jednocześnie nie dopuszczając do przepełnienia,

- Działania operujące na wartościach związanych z potegami liczby 2 należy implementować z użyciem operacji bitowych:
  - Mnożenie: M * (2^N) => M << N,
  - Dzielenie: M / (2^N) => M >> N,
  - Modulo: M % (2^N) => M & (2^N - 1) - lub maska bitowa z N-jedynkami, e.g. 7 % 4 = 7 % 2^2 = 7 & (2^2 - 1) = 7 & 0x03 = 0b0111 & 0b0011 = 0b0011

- Nie należy uzywać wielokrotnych wcięć przy formatoawniu (etykiety - bez wcięcia, kod - jedno wcięcie),

- Należy minimalizować ilość wywołań systemowych - wczytywać/zapisywać cały wiersz pikseli z/do pliku, zamiast każdego piksela osobno, lub cały obraz, zamiast każdego wiersza osobno, etc.
- Należy minimalizować ilość dostępów do pamięci poprzez sięganie po wiele bajtów jednocześnie - używać instrukcji lw/sw, zamiast lb/sb. Należy pamiętać o wyrównaniu dostępów do pamięci.

### Projekt Intel-x86
Wymagania dotyczące implementacji projektu Intel-x86 (ich nieprzestrzeganie będzie skutkować obniżeniem oceny):
TBD
