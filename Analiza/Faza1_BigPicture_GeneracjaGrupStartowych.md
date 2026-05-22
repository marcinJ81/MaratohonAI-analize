# Faza 1 — Big Picture: Generacja grup startowych
Data: 2026-05-22

## Streszczenie poprzedniej fazy (Faza 1 — Rejestracja)

- Pivotal event "Opłacono uczestnictwo" rozdziela uczestnika od zawodnika
- Zawodnik po opłacie otrzymuje globalny numer startowy (nie per dystans)
- Hotspot H1 (niedopłata vs generacja grup) — domknięty w tej fazie
- Ścieżka hybrydowa (rejestracja online + płatność w biurze) — istnieje, nieudokumentowana w ES
- Jedna zmiana dystansu per zawodnik

Otwarte pytania przeniesione z Rejestracji:
- H1 (niedopłata) → domknięty: system blokuje zmianę dystansu do opłaty, po opłacie → ostatnia grupa nowego dystansu

---

## Sub-konteksty i agenci

Obszar podzielono na 3 sub-konteksty:
- **G1** — Generacja grup (tworzenie grup z puli zawodników, okno zmian, finalna regeneracja)
- **G2** — Zmiana grupy przez zawodnika (wniosek w oknie czasowym)
- **G3** — Dołączenie po terminie (nowe opłaty po pierwszej generacji)

---

## Pytania i odpowiedzi

### Agent G1 — Generacja grup

**Q1:** Co definiuje grupę startową?

**A1:** Grupa startowa to zbiór zawodników startujących razem w tym samym oknie 5-minutowym.
- Maksymalnie **15 osób** per grupa — wymóg bezpieczeństwa (nakaz policji, trasa otwarta dla ruchu drogowego)
- Grupy generowane po **13 osób** — celowo zostawione 2 wolne miejsca na wnioski o zmianę grupy
- Grupy tworzone per dystans; dystanse o małej liczbie zawodników mogą być **łączone** (np. 650km + 700km) jeśli łącznie ≤ 15 osób
- Zawody 2-dniowe: długie dystanse (700–300km) startują **w sobotę** od ~15:00, krótkie **w niedzielę**
- Zapisy na dystanse sobotnię zamykają się o **13:00 w sobotę** (biuro zawodów)
- Każdy dystans ma limit czasowy (np. 700km ≈ 24h od startu); dystanse sobotnię wyznaczają godzinę zakończenia maratonu

---

**Q2:** Jak przypisywani są zawodnicy do konkretnej grupy?

**A2:** Kryterium = kolejność wpłat w ramach dystansu. Numer startowy jest **globalny** (nie per dystans) — przyznawany po opłacie niezależnie od dystansu. Brak kont uczestników — jednorazowy zapis per edycja zawodów. Brak kategorii wiekowej, brak kryterium z poprzednich lat.

---

**Q3:** Kiedy generowane są grupy?

**A3:** Grupy generowane **jednorazowo, zbiorczo** ~1,5 miesiąca przed startem (np. maj dla zawodów w czerwcu). Data ustalana w regulaminie — płynna, nie stały dzień. Generacja obejmuje tylko zawodników którzy opłacili do tego momentu.

Cykl edycji: ~1 rok. Przez większość roku trwa rejestracja + opłata → zawodnik dostaje numer startowy ale **nie jest przypisany do grupy** aż do momentu generacji.

---

**Q4:** Kto inicjuje generację i co następuje po niej?

**A4:** Administrator inicjuje ręcznie. Po Fazie I:
- Email do wszystkich zawodników z informacją o przydziale do grupy
- Otwiera się **okno zmian** (~tydzień, może być skrócone, raczej nie wydłużone — regulamin)
- Każdy zawodnik może złożyć wniosek o zmianę grupy (tylko raz, tylko w ramach swojego dystansu)

---

**Q5:** Co się dzieje gdy zawodnik zmienia dystans po wygenerowaniu grup?

**A5:** To **osobna operacja** niż zmiana grupy:
- Miejsce w starej grupie: zastąpione (jeśli grupy jeszcze otwarte) lub puste (jeśli grupy zamknięte)
- Zawodnik trafia jako ostatni na nowym dystansie
- Zmiana dystansu blokowana do momentu opłaty różnicy (H1 domknięty)

---

**Q6 (hotspot H1):** Co z zawodnikiem z niedopłatą przy generacji?

**A6:** System blokuje zmianę dystansu do czasu opłaty. Sprawy indywidualne rozwiązywane poza systemem (bezpośredni kontakt z administratorem). Po opłacie → ostatnia grupa nowego dystansu. **H1 domknięty.**

---

**Q7:** Co robi Faza II generacji (finalna)?

**A7:** Pełne odtworzenie grup od zera z nowymi kryteriami (uwzględniającymi złożone wnioski o zmianę). Nie patch — nowa generacja. Po Fazie II:
- Grupy **zamknięte** — zawodnicy nie mogą już zmieniać grup
- Nowi zawodnicy (po Fazie II) trafiają jako ostatni na swoim dystansie
- Brak wolnej grupy → zakładana nowa → może przesunąć harmonogram startu kolejnych dystansów

Szczegółowy algorytm Fazy II ("nowe kryteria") → otwarte pytanie do Process Level.

---

### Agent G2 — Zmiana grupy przez zawodnika

**Q8:** Jak zawodnik składa wniosek o zmianę grupy?

**A8:** Email lub wiadomość pozostawiająca ślad (do administratora). Zawodnik musi podać:
- Grupę aktualną
- Grupę docelową

Brak podpowiedzi systemu — świadoma decyzja zawodnika. Admin zbiera wnioski i przetwarza ręcznie.

Warunki zmiany (z ES):
- Nie przekroczono terminu zmiany grup
- Grupa docelowa ma wolne miejsca
- Zawodnik nie zmieniał wcześniej grupy
- Grupa docelowa jest z tego samego dystansu
- Grupy startowe zostały wygenerowane (warunek wstępny)

---

### Agent G3 — Dołączenie po terminie

**Q9:** Czy jest twarda granica po której nikt nie może dołączyć do grupy?

**A9:** Tak:
- Rejestracja online zamyka się **kilka dni przed startem**
- Biuro zawodów otwarte w dniu zawodów do **13:00** (2h przed sobotnim startem o 15:00)
- Po 13:00 brak możliwości dołączenia do sobotnich dystansów
- Biuro otwierane ponownie po sobotnim starcie — dla dystansów niedzielnych

---

### Wyjątek — przypisanie specjalnych zawodników

**Q10:** Automatyczne przypisanie bez wniosku?

**A10:** Istnieje możliwość ręcznego przypisania przez admina zawodników "specjalnie traktowanych" (na życzenie organizatora). Mała liczba przypadków, wyjątek od normalnego przepływu.

---

### Weryfikacja spójności

**Q11:** Co jest wyjściem z tego obszaru?

**A11:** Po Fazie II:
1. Grupy zamknięte → **opublikowana lista startowa** (stałe, publiczne)
2. W dniu zawodów: **weryfikacja startujących** (osobny etap)
3. Wejście do kontekstu Start

---

## Wnioski

1. **Dwufazowa generacja grup** — Faza I (po 13 osób, otwiera okno zmian) + Faza II (pełna regeneracja, zamknięcie). To architektonicznie ważne: nie jest to jeden akt lecz dwustopniowy protokół.

2. **Limit 15 osób** to wymóg zewnętrzny (policja/prawo drogowe), nie decyzja biznesowa. Generacja po 13 to świadomy bufor.

3. **Łączenie dystansów** w grupach startowych — małoliczne dystanse (np. 700km + 650km) mogą startować razem. To implikuje że "grupa" nie jest jednorodna per dystans.

4. **Zmiana grupy ≠ zmiana dystansu** — dwie osobne operacje z różnymi skutkami dla struktury grup.

5. **Brak kont uczestników** — jednorazowy zapis per edycja. Brak historii jako kryterium przypisania.

6. **Hotspot H1 domknięty:** niedopłata blokuje zmianę dystansu systemowo; rozwiązania indywidualne poza systemem.

7. **Harmonogram startu jest płynny** do niemal ostatniej chwili — nowe grupy tworzone dla spóźnionych zawodników mogą przesuwać godziny startu kolejnych dystansów.

8. **Wyjątek organizatorski** — admin może przypisać specjalnych zawodników poza normalnym przepływem.

---

## Otwarte pytania / niewiadome

- **H2:** Algorytm Fazy II — "nowe kryteria" generacji finalnej. Co dokładnie przebudowuje, jak honor-uje wnioski o zmianę grupy? → Do Process Level.
- Co się dzieje gdy zawodnik złożył wniosek o zmianę grupy, ale w międzyczasie zmienił też dystans? Konflikt wniosków.
- Jak wygląda "publikacja listy startowej" — system generuje PDF, stronę WWW, coś innego?
- Czy weryfikacja startujących w dniu zawodów jest osobnym kontekstem czy częścią tego obszaru?
- Definicja "małolicznego dystansu" uzasadniająca łączenie grup — kiedy 650+700 mogą startować razem? Regulamin?

---

## Zdarzenia do dodania / skorygowania w ES

| Zdarzenie | Akcja | Uwaga |
|---|---|---|
| Zamknięto okno zmian | Dodać | Brakuje jako explicit event — wyzwala Fazę II |
| Wygenerowano ostateczne grupy startowe | Dodać / Wyraźnie rozróżnić | Faza II ≠ Faza I — nowa generacja, nie modyfikacja |
| Opublikowano listę startową | Dodać | Pośredni etap między Fazą II a weryfikacją/startem |
| Przypisano specjalnego zawodnika do grupy | Dodać | Osobny wyjątkowy przepływ inicjowany przez admina |
| Zmiana dystansu po generacji → puste miejsce | Skorygować | ES nie modeluje pustego miejsca gdy grupy zamknięte |

---

## Wejście do następnej fazy

- Pivotal event "Wygenerowano ostateczne grupy startowe" → wejście do obszaru Weryfikacja/Start
- "Opublikowano listę startową" → widoczne dla zawodników i obsługi
- Harmonogram startu (kolejność grup, godziny) → dane operacyjne dla biura zawodów
- Otwarty H2 (algorytm Fazy II) → do Process Level
- Zawody 2-dniowe: sobota (700–300km, start ~15:00) + niedziela (krótkie dystanse)

---

## Korekty i uzupełnienia (retrospektywne)

*Sekcja pusta — wypełniana gdy późniejsze fazy wprowadzą zmiany do wniosków tej fazy.*
