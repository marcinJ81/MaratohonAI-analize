# Analiza porównawcza: deep-analysis ver1 vs ver2

## Co jest identyczne

Obie wersje mają ten sam frontmatter (name, description), tę samą zasadę nadrzędną (pętla zwrotna), identyczne: Faza 3, fazy specjalistyczne (S1–S3), pętla zwrotna, zasady pytań, format MD, relacja z innymi skillami.

---

## Różnice — ver2 dodaje lub rozszerza

### 1. Faza 0 — Orientacja (drobna zmiana)

| Ver1 | Ver2 |
|---|---|
| "Jaki poziom szczegółowości jest potrzebny?" | "...potrzebny — tylko Big Picture, do Process Level, pełna analiza?" |
| brak | Dodany blok: "Pytanie o materiały wejściowe" z explicit pytaniem o gotowy ES |

---

### 2. Nowa sekcja: Tryby pracy (tylko ver2)

Największy nowy element. Ver2 wprowadza wybór trybu przed Fazą 1:

| Tryb | Opis |
|---|---|
| Agentowy | Claude orkiestruje, deleguje konteksty wewnętrznym agentom |
| Ekspercki | Użytkownik jako ekspert domenowy, Claude zadaje pytania |
| Mixed | Część kontekstów do agentów, część prowadzi użytkownik |
| Użytkownik jako orkiestrator | Użytkownik decyduje co badać, Claude reaguje na żądanie |

Tryb można zmienić w trakcie bez powtarzania. W ver1 brak jakiegokolwiek odpowiednika.

---

### 3. Faza 1 — Big Picture (drastyczne rozszerzenie)

**Ver1:** Prosta lista 6 obszarów pytań.

**Ver2:** 4 pod-sekcje:
- **1a** — dedykowana obsługa wejścia z gotowym ES (opcja: zdarzenie po zdarzeniu vs. identyfikacja luk zbiorczo)
- **1b** — nowy koncept **bleed-through**: pytanie czy read modele lub ogólne zasady biznesowe mają wejść już na tym etapie; jasna lista co wnosimy / czego nie wnosimy
- **1c** — 3 bloki pytań (13 pytań vs 6 w ver1): przestrzeń problemu → hotspoty → spójność obrazu + opcjonalny blok read modeli
- **1d** — decyzja o przejściu (podobna, + notatka o zakończeniu na Big Picture dla biznesu)

---

### 4. Faza 2 — Process Level (dramatyczne rozszerzenie)

**Ver1:** Prosta lista 7 pytań + uwaga o możliwym podziale na 2a/2b/2c.

**Ver2:** 8 pod-sekcji:

| Pod-sekcja | Zawartość |
|---|---|
| 2a | Pytanie o bleed-through z Design Level (reguły biznesowe) |
| 2b | **Heurystyki odkrywania subdomen** — 4 heurystyki: językowa, właścicielska, zmienności, skupienia uwagi |
| 2c | **Typologia subdomen** — Core / Supporting / Generic z kryteriami decyzji |
| 2d | **Pivotal Events** — dedykowana sekcja z 4 pytaniami |
| 2e | Zarysowanie procesu jako ciągu — 7 pytań (rozgałęzienia, zależności, przepływ odwrotny) |
| 2f | **Core Domain — decyzja** — 4 pytania identyfikujące core domain |
| 2g | Opcjonalne reguły biznesowe (jeśli wybrano bleed-through) |
| 2h | **Pytania weryfikujące spójność procesu** — 5 pytań zamykających |

---

## Koncepty wprowadzone wyłącznie w ver2

1. **Bleed-through** — świadoma decyzja o tym, czy elementy z wyższych faz (read modele, reguły, komendy) mają "przesiąkać" do niższych faz; każda faza ma tablicę `[+] można dodać` / `[–] nie wnosimy`
2. **Tryby pracy** — elastyczność co do roli Claude vs. użytkownika
3. **Heurystyki subdomenowe** — konkretne narzędzia wykrywania granic (językowa, właścicielska, zmienności, skupienia)
4. **Typologia DDD** — explicit Core/Supporting/Generic z pytaniami kwalifikującymi
5. **Pivotal Events** jako osobny element analizy z pytaniami

---

## Ocena

**Ver1** jest szkieletem: dobra struktura, poprawne fazy, za mała liczba pytań w Fazach 1–2 by wyciągnąć z rozmowy wartościowy materiał.

**Ver2** to dojrzała wersja: ten sam szkielet, ale wypełniony — Faza 2 dostaje narzędzia DDD (heurystyki, typologia, pivotal events), Faza 1 dostaje mechanizm bleed-through i weryfikację spójności, a tryby pracy dają elastyczność do różnych kontekstów użycia.

**Rekomendacja:** ver2 jest kandydatem do zastąpienia ver1. Faza 3 w ver2 pozostała bez zmian — to jedyny obszar gdzie ver2 nie dorobiła się analogicznego pogłębienia.
