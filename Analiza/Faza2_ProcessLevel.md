# Faza 2 — Process Level: System zarządzania maratonem kolarskim
Data: 2026-05-23

## Streszczenie Fazy 1 — Big Picture

### Kluczowe wnioski

| Obszar | Pivotal events | Kluczowe ustalenia |
|---|---|---|
| Rejestracja | Opłacono uczestnictwo | Dwa kanały (online + biuro), ścieżka hybrydowa, limity od zawodników (nie uczestników), jedna zmiana dystansu |
| Generacja grup startowych | Wygenerowano ostateczne grupy startowe | Dwufazowa: I (po 13 osób) + II (pełna regeneracja, zamknięcie); limit 15 — wymóg policji; zmiana grupy ≠ zmiana dystansu |
| Weryfikacja startujących | Wydano pakiet startowy, Zamknięto zapisy na dystans | Trzy ścieżki wejścia (zawodnik / uczestnik / walk-in); oświadczenie obowiązkowe; tag RFID; asymetria sobota/niedziela |

### Obszary niezbadane w Fazie 1 (Big Picture)

Start zawodników, Pomiar czasu, Zakończenie zawodów / klasyfikacja — odkryte jako subdomeny w tej fazie.

### Otwarte hotspoty dziedziczone z Fazy 1

- H2: algorytm Fazy II generacji ("nowe kryteria")
- H-W2: walk-in niedziela do ostatniej chwili — decyzja sędziego poza systemem
- H-W4: odmowa podpisania oświadczenia — co z miejscem w grupie?
- Administrator = sędzia trasy — dwie nazwy, jedna rola

---

## Subdomeny i typologia

| # | Subdomena | Typ | Uzasadnienie |
|---|---|---|---|
| 1 | Rejestracja | **Supporting** | Wspiera core; zapis uczestnika i opłata to CRUD, nie przewaga konkurencyjna |
| 2 | Generacja grup startowych | **Core** | Najtrudniejsza, najbardziej czasochłonna, unikalna logika — tu jest przewaga |
| 3 | Biuro zawodów / Weryfikacja | **Supporting** | Operacyjne wsparcie dnia zawodów — ważne, pochodne od Rejestracji i Generacji |
| 4 | Start zawodników | **Supporting** | Inicjacja startu, rejestracja DNS — ważne operacyjnie, algorytmicznie proste |
| 5 | Pomiar czasu | **Generic** | Istnieją gotowe systemy RFID (ChampionChip, MyLaps i inne) — kupujemy lub integrujemy |
| 6 | Zakończenie / Wyniki | **Supporting** | Klasyfikacja, dyplomy — logika domenowa, ale nie unikalna |
| 7 | Konfiguracja / Regulamin | **Generic** | Parametry edycji, immutable w trakcie zawodów — typowa konfiguracja systemowa |

---

## Core Domain

**Generacja grup startowych** — potwierdzona przez użytkownika jako najtrudniejsza i najbardziej czasochłonna część systemu. To jest to czego nie można kupić gotowego i co odróżnia system od formularza + arkusza kalkulacyjnego.

---

## Pivotal Events

| # | Pivotal Event | Nieodwracalny | Zmiana odpowiedzialności | Widoczny zewnętrznie |
|---|---|---|---|---|
| P1 | **Opłacono uczestnictwo** | Tak — uczestnik staje się zawodnikiem | Rejestracja → Generacja | Tak — potwierdzenie dla zawodnika |
| P2 | **Wygenerowano ostateczne grupy startowe** | Tak — grupy zamknięte | Generacja → Biuro / Start | Tak — publikacja listy startowej |
| P3 | **Wydano pakiet startowy** | Tak — tag RFID aktywny i przypisany | Biuro → Start | Tak — zawodnik ma numer i tag |
| P4 | **Zamknięto zapisy na dystans** | Tak — per dystans, przy starcie pierwszej grupy | Rejestracja definitywnie zamknięta | Tak |
| P5 | **Wystartowano grupę** | Tak — zawodnik jedzie albo jest DNS/DNF | Start → Pomiar czasu | Tak |
| P6 | **Przekroczono metę / zakończono udział** | Tak — zawodnik staje się wpisem w wynikach | Pomiar czasu → Wyniki | Tak |

---

## Przepływ procesu

```
[Konfiguracja / Regulamin] ──────────────────────────────────────────────────────┐
  Ustalana przed edycją, immutable w trakcie zawodów.                            ↓ parametry dla wszystkich
  Zmiana → ponowne podpisanie regulaminu przez uczestników.

[Rejestracja]
  Kanał online (do dnia przed zawodami) + biuro zawodów + ścieżka hybrydowa
  ↓ P1: Opłacono uczestnictwo

[Generacja grup startowych]
  Faza I (po 13 osób, otwiera okno zmian) → okno zmian (~tydzień) → Faza II (pełna regeneracja, zamknięcie)
  ↓ P2: Wygenerowano ostateczne grupy startowe

[Biuro zawodów / Weryfikacja]
  Trzy ścieżki: zawodnik / uczestnik nieopłacony / walk-in
  Oświadczenie obowiązkowe. Asymetria sobota (do 13:00) / niedziela (do startu).
  ↓ P3: Wydano pakiet startowy
  ↓ P4: Zamknięto zapisy na dystans

[Start zawodników]
  Weryfikacja na linii startu. Rejestracja DNS. Inicjacja startu grupy.
  ↓ P5: Wystartowano grupę

[Pomiar czasu]  ← external system (RFID, readonly tags)
  Punkty kontrolne na trasie. Rejestracja odznaczeń tagiem. Rejestracja DNF (ręczna, sędzia trasy).
  ↓ P6: Przekroczono metę / zakończono udział

[Zakończenie / Wyniki]
  Klasyfikacja końcowa. Generacja certyfikatów / dyplomów.
  Odwołania → poza systemem (organizator + sędzia trasy).
```

### Rozgałęzienia

- **DNS** (Did Not Start): rejestrowane przy P5, zawodnik nie wchodzi do Pomiaru czasu
- **DNF** (Did Not Finish): ręczna rejestracja przez sędziego trasy w trakcie trasy
- **Walk-in**: wejście do Biura bez wcześniejszej rejestracji — ścieżka skrócona (rejestracja + opłata w jednym miejscu)
- **Tag RFID zgubiony / uszkodzony**: zawodnik jedzie bez oficjalnego pomiaru czasu (tagi readonly, nieodtwarzalne), brak wyniku w klasyfikacji
- **Zmiana Konfiguracji w trakcie edycji**: rzadki scenariusz — wymaga ponownego podpisania regulaminu

---

## Zależności między subdomenami

| Para | Kierunek zależności | Co przepływa |
|---|---|---|
| Rejestracja → Generacja | Sekwencyjna | Lista zawodników z numerami startowymi i dystansami |
| Generacja → Biuro / Start | Sekwencyjna | Lista startowa z przypisaniem do grup |
| Biuro → Start | Sekwencyjna | Tag RFID aktywny i przypisany do zawodnika |
| Start → Pomiar czasu | Sekwencyjna | Zawodnik z tagiem na trasie, czas startu grupy |
| Pomiar czasu → Wyniki | Sekwencyjna | Czasy odznaczeń punktów kontrolnych i mety |
| Konfiguracja → wszystkie | Przekrojowa (read-only) | Parametry edycji: limity, terminy, reguły |

**Niezależne od siebie:** Rejestracja nie zależy od Pomiaru czasu ani Wyników. Wyniki nie zależą od Rejestracji.

---

## Kluczowe niezmienniki (odkryte na tym poziomie)

- **Numer startowy jest immutable identyfikatorem zawodnika.** Tworzony w Rejestracji przy P1, używany read-only przez wszystkie kolejne subdomeny. Nie może ulec zmianie.
- **Tag RFID jest readonly i nieodtwarzalny.** Zgubienie / uszkodzenie = brak oficjalnego wyniku. Nie można przeprogramować nowego tagu z tym samym numerem.
- **Konfiguracja jest immutable w trakcie edycji.** Zmiana w trakcie → ponowne podpisanie regulaminu przez uczestników.
- **Limit 15 osób per grupa** — wymóg zewnętrzny (policja / prawo drogowe), nie decyzja biznesowa.

---

## Otwarte pytania / niewiadome → do Design Level

- **H2:** Algorytm Fazy II generacji grup — "nowe kryteria": co dokładnie przebudowuje, jak honoruje wnioski o zmianę grupy?
- **H-W2:** Walk-in niedziela last-minute — jak system wspiera decyzję sędziego trasy? Czy w ogóle?
- **H-W4:** Odmowa podpisania oświadczenia — czy zawodnik może wrócić? Co z jego miejscem w grupie?
- Konflikt wniosków: zawodnik złożył wniosek o zmianę grupy i zmianę dystansu jednocześnie — co wygrywa?
- Definicja "małolicznego dystansu" uzasadniająca łączenie grup w jednej grupie startowej — regulamin?
- Zmiana Konfiguracji w trakcie edycji: jak technicznie wygląda ponowne podpisanie regulaminu?
- Co się dzieje z zawodnikiem który dotarł na metę bez tagu (zgubiony) — czy pojawia się w wynikach jako DNF czy jako osobna kategoria?

---

## Wejście do Fazy 3 — Design Level

Bounded contexts do analizy (w kolejności priorytetowej):
1. **Generacja grup startowych** — core domain, najwyższy priorytet
2. **Rejestracja** — supporting, wejście do systemu, wiele wariantów ścieżek
3. **Biuro zawodów / Weryfikacja** — supporting, złożona logika dnia zawodów
4. **Start zawodników** — supporting
5. **Zakończenie / Wyniki** — supporting
6. **Konfiguracja / Regulamin** — generic, ale ważna zależność przekrojowa

Pomiar czasu (generic) — integracja z zewnętrznym systemem RFID, nie projektujemy sami.

---

## Korekty i uzupełnienia (retrospektywne)

[2026-05-23]: Faza 1 — Weryfikacja startujących zawierała błędny opis scenariusza zgubienia tagu RFID ("kaucja → nowy numer startowy"). Korekta: tagi są readonly i nieodtwarzalne. Zgubienie tagu = brak oficjalnego wyniku. Numer startowy jest immutable — nie może ulec zmianie pod żadnym scenariuszem.
