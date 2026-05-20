# Faza 1 — Big Picture: Rejestracja
Data: 2026-05-20

## Streszczenie poprzedniej fazy (Faza 0 — Orientacja)

- Domena: system zarządzania maratonem kolarskim
- Cel analizy: odświeżenie wiedzy o procesie i uzupełnienie Event Stormingu
- Odbiorcy: techniczni i biznesowi
- Materiał wejściowy: istniejący Event Storming (~18 diagramów)
- Zakres tej fazy: obszar Rejestracji (od zapisu uczestnika do momentu gdy staje się zawodnikiem)
- Tryb pracy: Agentowy (Claude orkiestruje)
- Zakres Big Picture: bazowy (bez read modeli)

Przeniesione z ES przed analizą:
- Hotspot: niedopłata vs generowanie grup startowych — niedomknięty
- Konfiguracja maratonu — obszar poza zakresem tej fazy (decyzja użytkownika)

---

## Sub-konteksty i agenci

Obszar Rejestracji podzielono na 3 sub-konteksty:
- **R1** — Rejestracja podstawowa
- **R2** — Opłata za udział
- **R3** — Zmiana dystansu

---

## Pytania i odpowiedzi

### Agent R1 — Rejestracja podstawowa

**Q1:** Kto inicjuje rejestrację — uczestnik online, obsługa w biurze, czy oboje?

**A1:** Oba kanały są możliwe, ale działają na różnych etapach:
- **Kanał A — Online:** dostępny od otwarcia zapisów do dnia poprzedzającego zawody (zamyka się wraz z otwarciem biura zawodów). Uczestnik rejestruje się samodzielnie przez formularz.
- **Kanał B — Biuro zawodów:** dostępny od otwarcia biura do startu. Rejestracji dokonuje obsługa biura.

---

**Q2:** Co wyzwala zdarzenie "Zweryfikowano zawodnika" w kanale online?

**A2:** Weryfikacja email — system wysyła link lub kod weryfikacyjny na adres uczestnika. Po kliknięciu linku lub wpisaniu kodu zdarzenie "Zweryfikowano zawodnika" zostaje zarejestrowane. Następuje **przed** opłatą, nie po niej.

---

**Q3:** Czy istnieje limit miejsc per dystans, i co się dzieje gdy limit jest osiągnięty?

**A3:** Tak, limity są określone w regulaminie. Liczba wolnych miejsc jest widoczna na formularzu online i w biurze zawodów. Nieopłaceni uczestnicy **nie są wliczani** do limitu — miejsce zajmowane jest dopiero po zdarzeniu "Opłacono uczestnictwo". Brak mechanizmu tymczasowej rezerwacji miejsca dla niezapłaconych.

---

**Q4:** Czy uczestnik może anulować rejestrację?

**A4:** Tak:
- Przed opłatą: anulowanie bez konsekwencji finansowych.
- Po opłacie: "Anulowano uczestnictwo" → wyliczono kwotę zwrotu zgodnie z regulaminem (pełna / częściowa / brak — zależnie od momentu anulowania).

Brakujące zdarzenie w ES: **"Anulowano uczestnictwo"** jako osobna ścieżka z rozgałęzieniem na zwrot.

---

### Agent R2 — Opłata za udział

**Q5:** Czym różni się moment "Opłacono uczestnictwo" między kanałami?

**A5:** To ten sam pivotal event, ale o różnej synchroniczności:
- **Kanał A (online):** zdarzenie asynchroniczne — zwłoka wynika z oczekiwania na potwierdzenie przelewu bankowego lub z wahania uczestnika.
- **Kanał B (biuro):** zdarzenie synchroniczne — uczestnik płaci na miejscu i natychmiast staje się zawodnikiem. Brak zwłoki.

Granica między **uczestnikiem** a **zawodnikiem** w obu kanałach przebiega przez **"Opłacono uczestnictwo"**.

---

**Q6:** Co się dzieje gdy przelew bankowy zostaje odrzucony lub cofnięty?

**A6:** System wysyła powiadomienie do uczestnika. Dalsze rozwiązanie (kontakt z bankiem, ponowna wpłata) odbywa się **poza systemem**. Granica systemu: wysłanie notyfikacji o błędzie płatności.

---

**Q7:** Czy jest deadline na opłatę dla kanału online?

**A7:** Tak — zazwyczaj dzień poprzedzający zawody (zamknięcie rejestracji online). Po upłynięciu deadline'u:
- Uczestnik który się zarejestrował online ale nie opłacił może **przyjść do biura zawodów i opłacić na miejscu** — ścieżka hybrydowa (rejestracja online + płatność w biurze). To domyka etap rejestracji.
- Jeśli nie opłaci w żadnym kanale — rejestracja "wisi" w systemie bezterminowo. Brak automatycznego czyszczenia.

Nowe zdarzenie do dodania w ES: **"Zakończono rejestrację online"** (wyzwalane przez czas).

---

**Q8:** Kto i na jakiej podstawie przyznaje promocję (-100%)?

**A8:** Decyzja uznaniowa — przyznawana ręcznie przez uprawnioną osobę (sędzia trasy lub organizator zawodów). Brak automatycznej reguły. System rejestruje zdarzenie "zaaplikowano promocję", ale inicjatywa jest ludzka i odbywa się poza systemem.

---

**Q9 (hotspot):** Co się dzieje z uczestnikiem który ma niedopłatę (zmienił dystans ale nie dopłacił) gdy startuje generowanie grup?

**A9 (domknięcie hotspotu):** Brak jednoznacznej odpowiedzi — każde podejście generuje problemy operacyjne:
- Przypisanie do nowej grupy przed opłatą → ryzyko konieczności usunięcia gdy nie zapłaci.
- Pozostawienie w starej grupie i czekanie → ryzyko opóźnienia lub zbyt późnej opłaty (przydział automatyczny bez wyboru grupy).
- Blokada generacji → ryzyko opóźnienia całego procesu.

**Decyzja: do ustalenia z biznesem. Udokumentować jako edge case w regulaminie.**

---

### Agent R3 — Zmiana dystansu

**Q10:** Czy zmiana na tańszy dystans (nadpłata) jest możliwa?

**A10:** Tak. Zwrot nadpłaty regulowany przez regulamin — zasady zależne od momentu zmiany (podobnie jak przy anulowaniu uczestnictwa).

---

**Q11:** Czy jest ograniczenie liczby zmian dystansu?

**A11:** Tak — regulamin dopuszcza **jedną zmianę dystansu** per zawodnik.

---

### Korekta aktora (synteza orkiestratora)

**Q12:** Kim jest aktor "obsługa punktu kontrolnego" pojawiający się w ES kontekstu Rejestracji?

**A12:** To personel punktów pomiaru czasu na trasie wyścigu — **inny aktor niż obsługa biura zawodów**. Zawodnik "odbija" dyskietkę (transponder) na fizycznym punkcie kontrolnym — tak rejestrowany jest czas przejazdu. W kontekście Rejestracji jest to aktor **spoza systemu** (zewnętrzny / read model consumer — np. czyta listę startową). Nie inicjuje zdarzeń w procesie rejestracji.

---

## Wnioski

1. **Dwa kanały rejestracji** działają sekwencyjnie w czasie, nie równolegle — online zamyka się gdy otwiera się biuro zawodów. Kluczowa różnica: synchroniczność płatności.

2. **Pivotal event "Opłacono uczestnictwo"** rozdziela pojęcia **uczestnik** (w procesie rejestracji) i **zawodnik** (pełnoprawny uczestnik zawodów z numerem startowym). Dotyczy obu kanałów.

3. **Ścieżka hybrydowa** (rejestracja online + płatność w biurze) istnieje i nie jest udokumentowana w ES. Wymaga dodania.

4. **Limity miejsc** liczone są od zawodników (po opłacie), nie od uczestników w trakcie rejestracji. Brak mechanizmu blokowania miejsca dla niezapłaconych.

5. **Anulowanie uczestnictwa** — brakująca ścieżka w ES. Dwa warianty: przed opłatą (brak konsekwencji) i po opłacie (zwrot wg regulaminu).

6. **Promocje (-100%)** — decyzja uznaniowa, inicjatywa ludzka poza systemem. System jedynie rejestruje zdarzenie.

7. **Jedna zmiana dystansu** per zawodnik — reguła regulaminowa.

8. **"Obsługa punktu kontrolnego"** błędnie pozycjonowana w ES jako aktor wewnętrzny Rejestracji. To zewnętrzny aktor pomiaru czasu.

9. **Brak automatycznego czyszczenia** nieopłaconych rejestracji online — wiszą bezterminowo.

---

## Otwarte pytania / niewiadome

- **H1 (hotspot):** Niedopłata vs generowanie grup startowych — do ustalenia z biznesem i udokumentowania w regulaminie.
- Kiedy dokładnie wysyłane jest powiadomienie o zbliżającym się deadline opłaty (jeśli w ogóle)?
- Czy istnieje mechanizm przypomnienia o nieopłaconej rejestracji (email reminder)?
- Co się dzieje z "wiszącymi" rejestracjami — czy administrator ma możliwość ich ręcznego usunięcia?
- Czy ścieżka hybrydowa (online + biuro) wymaga specjalnej obsługi przez personel biura?

---

## Zdarzenia do dodania / skorygowania w ES

| Zdarzenie | Akcja | Uwaga |
|---|---|---|
| Anulowano uczestnictwo | Dodać | Z rozgałęzieniem: przed/po opłacie |
| Zakończono rejestrację online | Dodać | Wyzwalane przez czas (deadline) |
| Ścieżka hybrydowa (online + biuro) | Dodać | Rejestracja online → płatność w biurze |
| Obsługa punktu kontrolnego w Rejestracji | Skorygować | Przenieść jako aktor zewnętrzny / read model consumer |

---

## Wejście do następnej fazy

- Pivotal event "Opłacono uczestnictwo" → wejście do obszaru Grup startowych
- Zawodnik po opłacie otrzymuje numer startowy → wejście do Generacji grup
- Hotspot H1 (niedopłata vs grupy) → otwarte pytanie dziedziczone do analizy Generacji grup
- Reguła: jedna zmiana dystansu per zawodnik → niezmiennik do uwzględnienia w Grupach startowych

---

## Korekty i uzupełnienia (retrospektywne)

*Sekcja pusta — wypełniana gdy późniejsze fazy wprowadzą zmiany do wniosków tej fazy.*
