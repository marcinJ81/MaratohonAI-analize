# Faza 1 — Big Picture: Weryfikacja startujących
Data: 2026-05-23

## Streszczenie poprzedniej fazy

Z Fazy 1 — Generacja grup startowych:
- Zawodnik ma przypisaną grupę startową i numer startowy (globalny)
- Lista startowa jest zamknięta i gotowa przed dniem zawodów
- Numer startowy globalny (nie per dystans)
- H2 otwarty: algorytm Fazy II generacji — do Process Level

## Pytania i odpowiedzi

Q1: Czy "kwalifikacja w biurze zawodów" i "weryfikacja przy starcie" to dwa osobne punkty obsługi w różnym czasie i miejscu?
A1: Tak — dwa odrębne momenty. Biuro zawodów to punkt w ciągu dnia, weryfikacja przy starcie to moment tuż przed startem grupy.

Q2: Czym jest "weryfikacja zawodnika" widoczna w ES jako osobny blok od biura zawodów?
A2: To weryfikacja na samej linii startu — zawodnicy się ustawiają i następuje ostatnia weryfikacja: czy wszyscy dotarli i są faktycznie tymi za których się podają.

Q3: Co się dzieje gdy zawodnik nie pojawi się na linii startu swojej grupy?
A3: Jest rejestrowany jako DNS (Did Not Start).

Q4: Kto rejestruje DNS i w jakiej granularności?
A4: Administrator rejestruje DNS per zawodnik. Administrator startuje grupy i zarządza tym procesem.

Q5: Czym jest "dyskietka z numerem startowym" z ES?
A5: Tag RFID — fizyczne urządzenie wydawane w pakiecie startowym. Zawodnik rejestruje nim odbicia na punktach kontrolnych trasy i na mecie. Zagubienie tagu uruchamia osobny scenariusz z kaucją.

Q6: Czym różni się DNS od dyskwalifikacji przy starcie?
A6: DNS = fizyczna nieobecność zawodnika. Dyskwalifikacja = złamanie regulaminu (np. cudzym numerem, niedozwolony sprzęt).

Q7: Czy "zamknięcie biura zawodów" i "zamknięcie zapisów na dystans" to to samo zdarzenie?
A7: Nie — dwa osobne zdarzenia:
- Zamknięcie biura zawodów (sobota 13:00) — logistyczne, przygotowanie do startu
- Zamknięcie zapisów na dystans — w momencie startu pierwszej grupy danego dystansu

Q8: Czy w niedzielę walk-in może rejestrować się dosłownie do momentu startu?
A8: Tak, bez bufora, ale wymaga zgody sędziego trasy.

Q9: Czy administrator i sędzia trasy to ta sama osoba?
A9: W dniu zawodów — tak, mogą być traktowani jako ta sama rola.

---

## Zdarzenia domenowe

### Biuro zawodów
- Zawodnik zgłosił się do biura zawodów
- Zweryfikowano tożsamość zawodnika
- Podpisano oświadczenie zawodnika
- Zawodnik odmówił podpisania oświadczenia → [brak startu]
- Opłacono uczestnictwo w biurze (dla uczestnika nieopłaconego lub walk-in)
- Zarejestrowano walk-in w biurze zawodów
- Wydano pakiet startowy (zawierający tag RFID)
- Zamknięto biuro zawodów *(sobota 13:00)*
- Zgubiono tag RFID → przejęto kaucję → odięto numer startowy → przypisano nowy numer → wydano nowy tag

### Start
- Zawodnicy ustawili się na linii startu grupy
- Zweryfikowano zawodnika przed startem (lista startowa + tożsamość)
- Zawodnik nie przeszedł weryfikacji → Zdyskwalifikowano zawodnika
- Zarejestrowano DNS zawodnika
- Wystartowano grupę
- Zarejestrowano czas startu grupy
- Wystartowano zawodnika indywidualnie (np. spóźnienie)
- Zamknięto zapisy na dystans *(w momencie startu pierwszej grupy dystansu)*

---

## Aktorzy

| Aktor | Rola |
|---|---|
| Zawodnik | Opłacony uczestnik — przychodzi do biura po pakiet |
| Uczestnik | Zarejestrowany, nieopłacony — płaci w biurze |
| Walk-in | Niezarejestrowany — rejestruje się i płaci w biurze |
| Obsługa punktu kontrolnego | Weryfikuje tożsamość w biurze, obsługuje pakiety |
| Administrator = Sędzia trasy | W dniu zawodów: startuje grupy, rejestruje czasy, zaznacza DNS, decyduje o spóźnionych walk-inach |

---

## Ścieżki wejścia do biura zawodów

```
Zawodnik (opłacony)
  → weryfikacja tożsamości → podpisanie oświadczenia → wydanie pakietu startowego

Uczestnik (zarejestrowany, nieopłacony)
  → płatność → [staje się zawodnikiem] → oświadczenie → pakiet startowy

Walk-in (niezarejestrowany)
  → rejestracja + płatność → [staje się zawodnikiem] → oświadczenie → pakiet startowy
```

---

## Asymetria sobota / niedziela

| | Sobota | Niedziela |
|---|---|---|
| Biuro zawodów | Zamknięte 13:00 | Otwarte cały czas |
| Walk-in do kiedy | Do 13:00 | Do momentu startu (za zgodą sędziego) |
| Zamknięcie zapisów | 15:00 (start pierwszej grupy) | Per dystans, przy starcie |
| Dystanse | 700–300 km | 200–50 km |

---

## Hotspoty

- **H1:** Asymetria sobota/niedziela — biuro zamknięte sobotę 13:00, otwarte cały czas w niedzielę. Różne reguły dostępu dla tych samych typów uczestników.
- **H2:** Walk-in w niedzielę do ostatniej chwili — wymaga zgody sędziego trasy. Decyzja ludzka poza systemem. Ryzyko: czy jest czas na weryfikację i wydanie tagu przed startem grupy?
- **H3:** Zagubienie tagu RFID — procedura kaucji + wymiana numeru startowego. Niejasne: czy zawodnik zmienia numer czy zachowuje stary z nowym tagiem?
- **H4:** Zawodnik odmawia podpisania oświadczenia — co dalej? Czy może wrócić? Czy slot w grupie zostaje zablokowany?
- **H5:** Dyskwalifikacja na starcie — co dzieje się z miejscem w grupie po dyskwalifikacji?

---

## Granice kontekstu

| Kierunek | Kontekst | Co przepływa |
|---|---|---|
| ← Wchodzi | Rejestracja | "Opłacono uczestnictwo" → zawodnik ma prawo do pakietu |
| ← Wchodzi | Generacja grup startowych | Lista startowa, przypisanie do grupy, numer startowy |
| → Wyprowadza | Pomiar czasu | Tag RFID aktywny, przypisany do zawodnika |
| → Wyprowadza | Start zawodników | Wystartowano grupę, zarejestrowano czas startu |

---

## Otwarte pytania / niewiadome

- Czy zawodnik który odmówił oświadczenia może wrócić do biura i podpisać je później?
- Czy przy zgubieniu tagu zawodnik zachowuje oryginalny numer startowy czy dostaje nowy?
- Czy decyzja sędziego trasy o przyjęciu walk-ina w ostatniej chwili jest rejestrowana w systemie?
- Co się dzieje z miejscem w grupie po dyskwalifikacji przy starcie?

## Wejście do następnej fazy

- Kluczowy pivotal event: **"Wydano pakiet startowy"** → zawodnik jest gotowy do startu
- Kluczowy pivotal event: **"Zamknięto zapisy na dystans"** → granica nieodwracalna, per dystans
- Tag RFID jako łącznik między weryfikacją a pomiarem czasu
- Administrator = sędzia trasy: jeden aktor, dwie nazwy w ES — ujednolicić w Process Level
- Hotspot H2 (walk-in niedziela) i H4 (brak oświadczenia) do dalszej eksploracji w Process Level

## Korekty i uzupełnienia (retrospektywne)

[2026-05-23]: Uzupełnienie ES — walk-in i uczestnik nieopłacony nie byli widoczni w oryginalnych diagramach. Oba scenariusze kończą się tym samym pivotal eventem "Opłacono uczestnictwo" co online.
[2026-05-23]: "Dyskietka" z ES to tag RFID — nie nośnik danych lecz urządzenie do pomiaru czasu.
[2026-05-23]: Administrator i sędzia trasy to ta sama rola w dniu zawodów — ES używa dwóch nazw dla jednego aktora.
