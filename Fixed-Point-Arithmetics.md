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
Wymagania dotyczące implementacji projektu Risc-V (ich nieprzestrzeganie skutkuje obniżeniem oceny):

- Obrazy powinny być wczytywane z dysku i zapisywane na dysk w formacie [BMP](https://en.wikipedia.org/wiki/BMP_file_format).
- operacje bitowe

- Obowiązuje zakaz używania typów zmiennopozycyjnych. W przypadku konieczności wykonywania obliczeń ułamkowych należy używać typów stałopozycyjnych (fixed-point).
- Użyty typ fixed-point powinien mieć tak dobraną ilośc bitów całkowitych i ułamkowych, aby maksymalizować dokładność obliczeń, jednocześnie nie dopuszczając do przepełnienia,

TBD

### Projekt Intel-x86
TBD
