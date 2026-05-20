# Faza 2 — Process Level: System zarządzania maratonem rowerowym

Data: 2026-05-19

## Streszczenie Fazy 1

Kluczowe wnioski wchodzące do tej fazy:
- Maraton: Sob 15:00 → Ndz 15:00 (okno 24h); różne dystanse = różne godziny startu
- Dystans jest podstawowym wymiarem organizacyjnym — determinuje grupę, godzinę startu, okno czasowe i tabelę wyników
- **Łańcuch niezmiennik:** opłata → numer startowy → pozycja w grupie (kolejność opłat)
- 4 pivotal events: Opłacono uczestnictwo / Wygenerowano grupy / Wystartowano zawodników / Zakończono przejazdy
- Zmiana dystansu to najkompleksiejszy cross-cutting process w systemie
- Hotspot nierozstrzygnięty: niedopłata w momencie generowania grup

---

## Pytania i odpowiedzi

**Q1: Jak przebiega główny przepływ — od zdarzenia inicjującego do skutku końcowego?**

A1: Przepływ sekwencyjny z 9 fazami operacyjnymi:

```
[Konfiguracja maratonu]
        ↓
[Rejestracja] — uczestnik wybiera dystans
        ↓
[Opłata za udział] — opłata → numer startowy
        ↓
[Generacja grup startowych] — okno migracji → zamknięcie list
        ↓
[Przygotowanie zawodnika do startu] — weryfikacja tożsamości, pakiet startowy
        ↓
[Start zawodników] — sędzia trasy startuje grupy co 15 min
        ↓
[Pomiar czasu] — punkty kontrolne + rejestracja końca
        ↓  (równolegle)
[Nakładanie kar] — sędzia trasy, pominięte punkty kontrolne
        ↓
[Zakończenie zawodów] — wyniki, nagrody, zwrot pakietów
```

**Q2: Gdzie są punkty decyzyjne (rozgałęzienia przepływu)?**

A2:

| Punkt decyzyjny | Rozgałęzienie |
|---|---|
| Czy uczestnik opłacił? | tak → numer startowy + grupa; nie → czeka |
| Kiedy następuje zmiana dystansu? | przed opłatą / po opłacie / po grupach / w trakcie wyścigu — 4 różne ścieżki |
| Czy zmiana dystansu to upgrade czy downgrade? | upgrade → dopłata; downgrade → bez zwrotu (po grupach) |
| Czy zawodnik ma niedopłatę przy generowaniu grup? | ⚠️ NIEROZSTRZYGNIĘTE — patrz sekcja hotspotów |
| Czy zawodnik złożył wniosek o zmianę grupy? | sprawdź: termin + wolne miejsca + brak wcześniejszej zmiany + ten sam dystans |
| Czy zawodnik rezygnuje w trakcie? | rezygnacja → klasyfikowany ostatni (nie dyskwalifikowany!) |
| Czy zawodnik chce wydłużyć dystans w trakcie? | sprawdź: nowy dystans jeszcze nie skończony + zdąży w oknie 24h |
| Czy zawodnik zwrócił pakiet w terminie? | tak → kaucja zwrócona; nie → kaucja zatrzymana |

**Q3: Które zdarzenia są pivotal events (zmieniają stan domeny nieodwracalnie)?**

A3: (potwierdzone z Fazy 1 + uzupełnione)

1. **Opłacono uczestnictwo** → uczestnik → zawodnik; dostaje numer startowy; w trakcie opłacania nie można zmienić dystansu
2. **Wygenerowano grupy startowe** → zmiana grupy wchodzi w tryb okna migracji; zmiana dystansu możliwa tylko na krótszy; kolejność opłat determinuje pozycję
3. **Zamknięto listy startowe** → grupy są ostateczne; nowi zawodnicy trafiają na koniec ostatniej grupy lub do nowej
4. **Wystartowano zawodników** → zawody w toku; zapisy zamknięte; możliwe tylko: rezygnacja lub wydłużenie dystansu
5. **Zakończono przejazdy wszystkich dystansów** → koniec rywalizacji; wyniki niezmienne

**Q4: Jakie są subdomeny — gdzie granice są naturalne?**

A4: Zidentyfikowane 9 bounded contexts:

| Bounded Context | Opis |
|---|---|
| **Konfiguracja maratonu** | Zarządzanie regulaminem, dystansami, godzinami startów, limitami, cenami |
| **Rejestracja** | Tworzenie konta uczestnika, wybór dystansu, zmiana dystansu przed opłatą |
| **Opłata za udział** | Płatności, kalkulacja opłat, dopłaty, zwroty, obsługa niedopłat; integracja z bankiem |
| **Generacja grup startowych** | Tworzenie grup, okno migracji, zamykanie list, obsługa zawodników po terminie; numer startowy jako warunek wejścia |
| **Przygotowanie zawodnika do startu** | Weryfikacja tożsamości, podpisanie oświadczenia, wydanie pakietu startowego, kaucja, obsługa zgubionej dyskietki |
| **Start zawodników** | Zamknięcie zapisów per dystans, startowanie grup co 15 min, start indywidualny |
| **Pomiar czasu** | Rejestracja startów i finiszów, obsługa wydłużenia dystansu w trakcie, podliczenie czasów per dystans |
| **Nakładanie kar** | Kary za złamanie regulaminu, kary za pominięte punkty kontrolne |
| **Zakończenie zawodów** | Wyniki końcowe z uwzględnieniem kar, nagrody, dyplomy, zwrot pakietów, kaucje |

**Q5: Co jest domeną główną (core), co wspierającą (supporting), co generyczną?**

A5:

| Klasyfikacja | Bounded Contexts | Uzasadnienie |
|---|---|---|
| **Core** | Generacja grup startowych | Złożony algorytm (kolejność opłat, max 15/grupę, starty co 15 min, okno migracji) — tu jest unikalna logika biznesowa |
| **Core** | Pomiar czasu | Staggered starts per dystans, wydłużenie dystansu w trakcie, integracja z systemem pomiaru — nietrywialny |
| **Core** | Nakładanie kar | Reguły specyficzne dla maratonu, wpływ na wyniki końcowe |
| **Supporting** | Rejestracja | Konieczna, ale nie wyróżniająca |
| **Supporting** | Opłata za udział | Specyficzna kalkulacja (dopłaty, zwroty, reguły per stan) — nie jest generic ze względu na złożoność reguł |
| **Supporting** | Przygotowanie do startu | Operacyjna, z unikalnym elementem (dyskietka/kaucja) |
| **Supporting** | Start zawodników | Operacyjna |
| **Supporting** | Zakończenie zawodów | Operacyjna |
| **Generic** | Konfiguracja maratonu | Zarządzanie konfiguracją — mogłoby być wyciągnięte do gotowego rozwiązania |

**Q6: Jakie są zależności między subdomenami?**

A6:

```
Konfiguracja maratonu ──────────────────────→ WSZYSTKIE
  (regulamin, dystanse, godziny startów,
   limity, ceny, terminy)

Rejestracja ──────────────────────────────→ Opłata za udział
  (uczestnik gotowy do opłacenia)

Opłata za udział ─────────────────────────→ Generacja grup startowych
  (numer startowy = warunek wejścia do grupy;
   kolejność opłat = kolejność w grupie)

Opłata za udział ←───────────────────────── Rejestracja
  (zmiana dystansu → przelicz opłatę)

Generacja grup startowych ────────────────→ Przygotowanie do startu
  (lista startowa = podstawa weryfikacji)

Przygotowanie do startu ──────────────────→ Start zawodników
  (zweryfikowani zawodnicy z pakietami)

Start zawodników ─────────────────────────→ Pomiar czasu
  (czas startu grupy = punkt odniesienia dla pomiarów)

Nakładanie kar ───────────────────────────→ Zakończenie zawodów
  (kary do uwzględnienia w wynikach)

Pomiar czasu ─────────────────────────────→ Zakończenie zawodów
  (czasy przejazdu per zawodnik per dystans)
```

**Q7: Gdzie mogą być konflikty własności danych lub procesów?**

A7:

1. **Opłata za udział ↔ Generacja grup** — właściciel "aktualnego dystansu zawodnika" przy niedopłacie
   - *To jest nierozstrzygnięty hotspot — patrz poniżej*

2. **Rejestracja ↔ Opłata za udział** — zmiana dystansu przed opłatą jest prosta (Rejestracja ją zarządza), ale kto jest "prawdziwym" właścicielem dystansu uczestnika?
   - *Propozycja: dystans staje się "potwierdzony" dopiero po opłacie — przed opłatą to intencja, po opłacie — fakt*

3. **Pomiar czasu ↔ Nakładanie kar** — wynik końcowy wymaga danych z obu kontekstów, ale kto go "oblicza"?
   - *Propozycja: Zakończenie zawodów jest agregatorem — zbiera dane z Pomiaru czasu i Kar*

---

## Wnioski

1. **Łańcuch opłata → numer → grupa** jest centralnym niezmiennikiem całego systemu — brak opłaty blokuje cały downstream
2. **Dystans** jest shared concept między prawie wszystkimi kontekstami — jego zmiana to cross-cutting process z 4 stanami i różnymi regułami w każdym kontekście
3. **Konfiguracja maratonu** jest dostawcą reguł dla wszystkich — powinna być stabilna zanim ruszą inne procesy
4. **Generacja grup** to kluczowy punkt synchronizacji — po nim wiele procesów zmienia charakter (inne reguły zmiany dystansu, inne reguły zmiany grupy)
5. **Nakładanie kar** jest słabo rozbudowane w ES — tylko 2 zdarzenia widoczne; to może być niedomodelowany obszar
6. **Rezygnacja w trakcie ≠ dyskwalifikacja** — zawodnik klasyfikowany jako ostatni; to musi być jawna reguła w Zakończeniu zawodów
7. **9 dystansów × grupy co 15 min** = potencjalnie duża liczba grup i złożony harmonogram startów — skala operacyjna jest nietrywialnym wymaganiem

---

## Otwarte pytania / niewiadome

- ⚠️ **[HOTSPOT — NIEROZSTRZYGNIĘTY]** Zawodnik z niedopłatą w momencie generowania grup: powinien trafić do nowego dystansu z ryzykiem braku opłaty, czy zostać w starym z oknem na spłatę? — **decyzja biznesowa do wpisania w regulamin**
- Jak dokładnie obliczana jest godzina startu każdej grupy per dystans — konfigurowana przez administratora czy wyliczana z regulaminowych limitów czasowych?
- Czy "nakładanie kar za pominięte punkty kontrolne" jest automatyczne (system wykrywa brak przejścia) czy manualne (sędzia zgłasza)?
- Co dzieje się z numerem startowym przy zmianie dystansu po wygenerowaniu grup — zawodnik dostaje nowy numer czy zachowuje stary?
- Ile wynosi kaucja za dyskietkę i czy jest stała czy konfigurowana?

---

## Korekty i uzupełnienia (retrospektywne)

*[Puste — brak retrospektywnych korekt z Fazy 1 na tym etapie]*

---

## Wejście do Fazy 3 (Design Level)

Konteksty bez Design Level (priorytet do zamodelowania):
1. **Opłata za udział** — złożone reguły kalkulacji, 4 warianty zmiany dystansu, zwroty, niedopłaty
2. **Rejestracja** — prosta, ale dystans jako "intencja vs fakt" wymaga precyzji
3. **Pomiar czasu** — staggered starts, wydłużenie dystansu w trakcie
4. **Nakładanie kar** — niedomodelowane w ES
5. **Zakończenie zawodów** — agregacja wyników z wielu kontekstów

Konteksty z częściowym Design Level (do uzupełnienia):
- **Generacja grup startowych** — `GeneracjaGrupStartowych` i `GrupyStartowe` agregaty są; brakuje modelu okna migracji i obsługi po terminie
- **Zmiana grupy startowej** — `ZmianyGrup` agragat istnieje

Nierozstrzygnięty hotspot (niedopłata przy generowaniu) musi być rozstrzygnięty **przed** modelowaniem Design Level dla Opłaty za udział i Generacji grup.
