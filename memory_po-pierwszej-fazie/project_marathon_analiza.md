---
name: project-marathon-analiza
description: "Stan analizy domeny systemu zarządzania maratonem kolarskim — cel, tryb, ukończone fazy, następny krok"
metadata: 
  node_type: memory
  type: project
  originSessionId: 9ac76472-eb0d-4378-b972-a47aacdf0644
---

System zarządzania maratonem kolarskim. Analiza oparta na istniejącym Event Stormingu (18 diagramów w `/home/domek/Marathon_Project/Dane_wejsciowe/`).

**Cel:** odświeżenie wiedzy o procesie + uzupełnienie ES
**Odbiorcy:** techniczni i biznesowi
**Skill:** deep-analysis ver2 (`/home/domek/Marathon_Project/Skills/deep-analisis-ver2/SKILL-deep-analisis-ver2.md`)
**Tryb pracy:** Tryb 1 — Agentowy (Claude orkiestruje)
**Zakres Big Picture:** bazowy (bez read modeli)

**Why:** Użytkownik chce dogłębnie zrozumieć domenę i uzupełnić istniejący ES przed dalszym projektowaniem.

**How to apply:** Przy wznowieniu sesji załadować skill deep-analysis ver2, przypomnieć stan analizy i kontynuować od następnego kontekstu.

---

## Ukończone

- Faza 0 — Orientacja: zakończona
- Faza 1 — Big Picture: **Rejestracja** — zakończona
  - Plik: `/home/domek/Marathon_Project/Analiza/Faza1_BigPicture_Rejestracja.md`
  - Konfiguracja maratonu — poza zakresem (decyzja użytkownika)

## Następny krok

Faza 1 — Big Picture: **Generacja grup startowych**
(wejście: pivotal event "Opłacono uczestnictwo" + numer startowy)

## Kluczowe ustalenia z Rejestracji

- Dwa kanały: online (async płatność) i biuro zawodów (sync płatność) — kanały sekwencyjne w czasie
- Pivotal event "Opłacono uczestnictwo" rozdziela uczestnika od zawodnika — dotyczy obu kanałów
- Ścieżka hybrydowa: rejestracja online + płatność w biurze — nieudokumentowana w ES
- Limity miejsc liczone od zawodników (po opłacie), nie od uczestników w toku
- Anulowanie uczestnictwa — brakująca ścieżka w ES
- Jedna zmiana dystansu per zawodnik (reguła regulaminowa)
- "Obsługa punktu kontrolnego" = personel pomiaru czasu na trasie, aktor zewnętrzny w kontekście Rejestracji
- Promocje (-100%) — decyzja uznaniowa, inicjatywa ludzka poza systemem

## Otwarty hotspot (H1)

Niedopłata vs generowanie grup startowych — do ustalenia z biznesem, udokumentować w regulaminie jako edge case.
