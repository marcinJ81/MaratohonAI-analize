# Faza 1 — Big Picture: System zarządzania maratonem rowerowym

Data: 2026-05-19

## Streszczenie poprzedniej fazy
Punkt startu: istniejący Event Storming (18 zrzutów ekranu, głównie Process Level + częściowy Design Level dla kontekstu Grupy Startowe). Materiał ES stanowi dane wejściowe — faza 1 buduje na nim, nie powtarza.

---

## Pytania i odpowiedzi

**Q1: Co dzieje się w tej domenie na najwyższym poziomie?**
A1: System zarządza pełnym cyklem życia maratonu rowerowego. Maraton ma stałą strukturę czasową: zawsze odbywa się z soboty na niedzielę, start 15:00 w sobotę, koniec 15:00 w niedzielę (okno 24h). Obejmuje: rejestrację uczestników i wybór dystansu, organizację startu według dystansów i grup, pomiar czasu przejazdu z punktami kontrolnymi, nakładanie kar regulaminowych, zakończenie i rozdanie nagród. Fizyczny artefakt (dyskietka/transponder z numerem startowym) jest kluczowym łącznikiem między światem fizycznym a cyfrowym.

**Q2: Kto inicjuje kluczowe zdarzenia (aktorzy i systemy)?**
A2:
- **Uczestnik/Zawodnik** — rejestracja, zmiany dystansu, zgłoszenia w trakcie wyścigu
- **Administrator** — konfiguracja maratonu, zarządzanie zapisami, starty indywidualne
- **Główny organizator zawodów** — nagrody, decyzje najwyższego poziomu
- **Sędzia trasy** — rejestracja startów grup, nakładanie kar regulaminowych
- **Obsługa punktu kontrolnego** — weryfikacja zawodników, wydanie pakietów, kaucje, obsługa punktów na trasie
- **Pracownik obsługi / administrator** — obsługa zwrotów i nadpłat
- **Interfejs banku** *(zewnętrzny)* — przetwarzanie przelewów
- **System pomiaru czasu** *(zewnętrzny/wewnętrzny)* — rejestracja czasów z transponderów

**Q3: Jakie są skutki kluczowych zdarzeń — co się zmienia w domenie?**
A3: **Dystans jest podstawowym wymiarem organizacyjnym maratonu.** Różne dystanse mają różne dni/godziny startu (np. 600 km startuje w sobotę, 100 km w niedzielę). To tworzy twardy fizyczny constraint: zawodnik nie może zmienić dystansu na dłuższy jeśli tamten dystans już wystartował.

Pivotal events (punkty-of-no-return):

| # | Zdarzenie | Skutek w domenie |
|---|---|---|
| 1 | **Opłacono uczestnictwo** | Uczestnik staje się zawodnikiem; w trakcie opłacania nie można zmienić dystansu |
| 2 | **Wygenerowano grupy startowe** | Zmiana grupy ograniczona regułami; zmiana dystansu możliwa tylko na krótszy, bez zwrotu opłaty |
| 3 | **Wystartowano zawodników** | Zawody w toku; zapisy zamknięte; możliwa rezygnacja (klasyfikowany ostatni) lub wydłużenie dystansu (bez dodatkowych opłat, musi zmieścić się w czasie) |
| 4 | **Zakończono przejazdy wszystkich dystansów** | Koniec rywalizacji; czas na ceremonię |

**Q4: Gdzie są hotspoty — punkty bólu i niejasności?**
A4:
- **Zmiana dystansu ma 4 stany z różnymi regułami** (przed opłatą / po opłacie / po wygenerowaniu grup / w trakcie wyścigu) — najkompleksiejszy proces
- **Nierozstrzygnięty hotspot z ES:** *"co zrobić jak rozpocznie się generowanie list a czekamy na opłacenie niedopłaty?"* — brak odpowiedzi w materiale
- **Rezygnacja w trakcie ≠ dyskwalifikacja** — zawodnik rezygnujący z przyczyn zdrowotnych klasyfikowany jest jako ostatni na danym dystansie (ważna reguła domenowa!)
- **Zgubienie dyskietki** — fizyczny artefakt w cyfrowym procesie (kaucja jako zabezpieczenie)
- **Przedłużenie dystansu w trakcie wyścigu** z ograniczeniem czasowym (musi zmieścić się w 24h oknie)

**Q5: Co jest poza granicami tej domeny?**
A5: Marketing i pozyskiwanie uczestników, fizyczna logistyka trasy (oznakowanie, rozmieszczenie punktów), zarządzanie obsługą i wolontariuszami, ogólne rozliczenia finansowe organizatora.

**Q6: Jakie są zależności z zewnętrznymi systemami?**
A6:
- **Interfejs banku** — dwukierunkowy: system inicjuje opłacenie, bank potwierdza przelew
- **System pomiaru czasu** — jednostronny: dane z transponderów wchodzą do systemu jako zdarzenia

---

## Wnioski

1. Maraton ma stałą strukturę czasową (Sob 15:00 – Ndz 15:00), która tworzy twarde, fizyczne ograniczenia na zmiany dystansu — dystans nie jest tylko "etykietą", determinuje dzień startu
2. **Dystans jest kluczowym wymiarem** — determinuje dzień startu, przynależność do grupy, okno czasowe i tabelę wyników
3. **Zmiana dystansu** jest prawdopodobnie najkompleksiejszym procesem w systemie — 4 stany, różne reguły finansowe, możliwe tylko w określonych kierunkach (przed grupami: dowolna; po grupach: tylko krótszy; w trakcie: tylko dłuższy jeśli zdąży)
4. **Fizyczny artefakt (dyskietka)** jest wąskim gardłem styku fizycznego i cyfrowego — jego utrata wymaga dedykowanego procesu z kaucją
5. Rezygnacja w trakcie ≠ dyskwalifikacja — ważna reguła biznesowa wpływająca na tabele wyników
6. System ma **8–9 wyraźnych bounded contexts** (widocznych w ES)

---

## Otwarte pytania / niewiadome

- **[KLUCZOWE]** Jakie dystanse są dostępne? (poza 600 km i 100 km — czy są inne, np. 200 km, 50 km?)
- Czy każdy dystans ma zawsze inną godzinę/dzień startu, czy to jest konfigurowane per edycja maratonu?
- Jak dokładnie obliczana jest dopłata przy zmianie dystansu po opłacie — tylko różnica ceny czy też opłata manipulacyjna?
- Co dzieje się z kaucją przy rezygnacji w trakcie wyścigu?
- Czym technicznie jest "dyskietka" — chip RFID, transponder, fizyczny numer startowy?

---

## Wejście do Fazy 2

- 8–9 bounded contexts do zbadania: granice, zależności, klasyfikacja (core/supporting/generic)
- Pivotal events wyznaczają naturalne granice między fazami w życiu maratonu
- **Hotspot niedopłata/generowanie list** wymaga rozstrzygnięcia w Fazie 2
- **Zmiana dystansu** jest kandydatem na cross-cutting concern lub osobny bounded context
- Stała struktura czasowa (Sob–Ndz) musi być uwzględniona przy modelowaniu grup startowych
