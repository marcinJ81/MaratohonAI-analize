---
name: deep-analysis
description: >
  Pogłębiona analiza problemów złożonych — architektonicznych, biznesowych, domenowych —
  oparta na pytaniach i wyciąganiu wniosków, inspirowana Event Stormingiem (Big Picture →
  Process Level → Design Level). Każda faza buduje na poprzedniej, generuje osobny plik MD
  i może być skalowana do złożoności problemu. Triggeruj gdy użytkownik chce dogłębnie
  zrozumieć problem, a nie tylko porównać opcje: "przeanalizujmy to od podstaw", "chcę
  zrozumieć domenę", "zróbmy analizę", "jak to powinno działać", "zacznijmy od big picture",
  "zamodelujmy to", "rozbijmy to na części". Użyj też gdy istnieje Event Storming który
  wymaga uzupełnienia lub rozwinięcia.
---

# Deep Analysis Skill

## Zasada nadrzędna

Analiza to **pętla zwrotna**, nie liniowy proces.
Każda faza zadaje pytania → zbiera odpowiedzi → wyciąga wnioski → zasilają następną fazę.
Materiały zewnętrzne (ES, diagramy, dokumenty) wchodzą w pętlę jako dane wejściowe do dowolnej fazy.

---

## Uruchomienie — zanim zaczniesz

### 1. Oceń punkt startu

Przed fazą pierwszą zapytaj:

- Czy istnieje już Event Storming lub inna analiza? → jeśli tak, przejdź do sekcji **"Wejście z gotowym materiałem"**
- Jaki jest **cel analizy** — zrozumienie domeny, decyzja architektoniczna, planowanie, coś innego?
- Jaki jest **horyzont** — jednorazowe rozpoznanie czy podstawa pod dalsze projektowanie?
- Czy użytkownik chce wszystkie fazy, czy skupić się na konkretnej?

### 2. Wejście z gotowym materiałem (ES lub inna analiza)

Jeśli materiał istnieje:

1. Oceń które fazy są już pokryte — zaznacz jako `[pokryte]`
2. Zidentyfikuj luki: brakujące pytania, niedomknięte wnioski, pominięte konteksty
3. Zaproponuj od której fazy zacząć i co uzupełnić
4. Nie powtarzaj tego co jest — **buduj na tym co masz**

### 3. Ustal skalę złożoności

| Złożoność | Sygnały | Podejście |
|---|---|---|
| Mała | jeden proces, mało aktorów, znana domena | 1–2 fazy, po 5–8 pytań każda |
| Średnia | kilka procesów, zależności między kontekstami | 3–4 fazy, po 8–12 pytań |
| Duża | wiele subdomen, integracje, pivotal events | wszystkie fazy, możliwy podział faz na podfazy |
| Bardzo duża | złożona domena enterprise, wiele bounded contexts | każdy bounded context może być osobną instancją faz 2–3 |

Skalę ustala użytkownik po obejrzeniu propozycji — nie zakładaj.

---

## Struktura faz

### Faza 0 — Orientacja (zawsze, krótka)

**Cel:** Ustalenie ram analizy zanim zadasz pierwsze pytanie merytoryczne.

Wynik: nie ma osobnego pliku MD — to wstęp do Fazy 1.

Pytania orientacyjne (wybierz te które nie są znane):
- Co jest przedmiotem analizy (system, proces, decyzja)?
- Kto jest odbiorcą wyników — techniczni, biznesowi, oboje?
- Co już wiadomo, co jest pewne?
- Gdzie są największe niewiadome?
- Jaki poziom szczegółowości jest potrzebny?

---

### Faza 1 — Big Picture

**Odpowiednik ES:** Big Picture Event Storming
**Cel:** Zarys całości. Przestrzeń problemu, aktorzy, zdarzenia na poziomie domenowym, wstępne granice.

**Zasady:**
- Pytania dotyczą całości, nie szczegółów
- Każde pytanie i odpowiedź są konsekwencją poprzedniego
- Nie schodzimy do implementacji
- Zatrzymujemy się gdy obraz całości jest wystarczająco czytelny

**Obszary pytań (kolejność jest ważna):**

```
1. Co się dzieje w tej domenie — jakie zdarzenia są kluczowe?
2. Kto inicjuje te zdarzenia (aktorzy zewnętrzni, systemy)?
3. Jakie są skutki tych zdarzeń — co się zmienia w domenie?
4. Gdzie są punkty bólu, tarcia, niejasności (hotspots)?
5. Co jest poza granicami tej domeny (co ignorujemy)?
6. Jakie są zależności z zewnętrznymi systemami lub domenami?
[dalej — zależnie od złożoności]
```

**Podsumowanie Fazy 1 (plik MD):**

```markdown
# Faza 1 — Big Picture: [nazwa domeny/systemu]

## Streszczenie poprzedniej fazy
(przy Fazie 1 = punkt startu lub opis materiałów wejściowych)

## Pytania i odpowiedzi
Q1: [pytanie]
A1: [odpowiedź]

Q2: [pytanie]
A2: [odpowiedź]
...

## Wnioski
- [wniosek 1]
- [wniosek 2]
...

## Otwarte pytania / niewiadome
- [co pozostaje niejasne]

## Wejście do następnej fazy
[co z tej fazy przenosi się do Fazy 2]
```

**Decyzja o przejściu:**
Użytkownik potwierdza czy Big Picture jest wystarczająco pełny.
Pytaj: "Czy obraz całości jest wystarczający by przejść do poziomu procesu, czy coś wymaga doprecyzowania?"

---

### Faza 2 — Process Level

**Odpowiednik ES:** Process Level Event Storming
**Cel:** Zarysowanie subdomen, ciągu zdarzeń i skutków, zależności między częściami procesu, wstępne granice kontekstów.

**Zaczyna się od streszczenia Fazy 1** — wypisz kluczowe wnioski i otwarte pytania które wpłynęły na tę fazę.

**Obszary pytań:**

```
1. Jak przebiega główny przepływ — od zdarzenia inicjującego do skutku końcowego?
2. Gdzie są punkty decyzyjne (rozgałęzienia przepływu)?
3. Które zdarzenia są pivotal events (zmieniają stan domeny nieodwracalnie)?
4. Jakie są subdomeny — gdzie granice są naturalne?
5. Co jest domeną główną (core), co wspierającą (supporting), co generyczną?
6. Jakie są zależności między subdomenami?
7. Gdzie mogą być konflikty własności danych lub procesów?
[dalej — zależnie od złożoności]
```

**Uwaga o skali:**
Jeśli domena jest bardzo rozległa, Faza 2 może być podzielona:
- 2a — mapowanie przepływu głównego
- 2b — analiza subdomen
- 2c — granice kontekstów i zależności

Podział decyduje użytkownik po obejrzeniu zakresu.

**Podsumowanie Fazy 2:** analogiczny format jak Faza 1.

**Decyzja o przejściu:**
"Czy mamy wystarczający obraz procesu i subdomen by schodzić do poziomu projektowania, czy coś wymaga doprecyzowania?"

---

### Faza 3 — Design Level

**Odpowiednik ES:** Design Level / Software Design
**Cel:** Szczegóły — komendy, agregaty, polityki, zdarzenia domenowe w obrębie konkretnego bounded context.

**Zaczyna się od streszczenia Faz 1 i 2** — wypisz bounded contexts które będą analizowane, zdecyduj od którego zacząć.

**Jeśli jest wiele bounded contexts — każdy to osobna instancja Fazy 3.**

**Obszary pytań (per bounded context):**

```
1. Jakie komendy wchodzą do tego kontekstu?
2. Co jest agregatem — co chroni niezmienniki biznesowe?
3. Jakie polityki (policies) reagują na zdarzenia z innych kontekstów?
4. Jakie zdarzenia ten kontekst publikuje na zewnątrz?
5. Jakie są niezmienniki biznesowe (invariants) — co nigdy nie może być naruszone?
6. Jakie są reguły walidacji i ich źródło (domena vs aplikacja)?
[dalej — zależnie od złożoności]
```

**Podsumowanie Fazy 3:** analogiczny format, per bounded context.

---

### Fazy specjalistyczne (po Design Level lub równolegle)

Uruchamiane na żądanie użytkownika lub gdy pojawią się sygnały w poprzednich fazach.

#### Faza S1 — Analiza ryzyka

**Cel:** Identyfikacja zagrożeń wynikających z analizy faz 1–3.

Obszary:
- Ryzyka architektoniczne (coupling, spójność kontekstów)
- Ryzyka biznesowe (niedomknięte procesy, brak właściciela)
- Ryzyka implementacyjne (złożoność, zależności zewnętrzne)
- Ryzyka danych (spójność, migracja, backward compatibility)

Każde ryzyko: opis → prawdopodobieństwo → wpływ → mitygacja.

#### Faza S2 — Analiza skrajnych przypadków

**Cel:** Co się dzieje gdy coś idzie nie tak lub gdy warunki są graniczne.

Obszary:
- Przypadki graniczne dla agregatów i niezmienników
- Scenariusze awarii (co gdy zewnętrzny system nie odpowiada?)
- Scenariusze błędnych danych wejściowych
- Scenariusze współbieżności

#### Faza S3 — Możliwości rozwoju

**Cel:** Co ta architektura umożliwia, a co utrudnia w przyszłości.

Obszary:
- Gdzie jest miejsce na rozszerzenie bez zmiany istniejących kontekstów
- Gdzie zmiany będą kosztowne (tight coupling)
- Jakie nowe subdomeny mogą pojawić się w horyzoncie 1–2 lat
- Co wymaga decyzji teraz by nie blokować przyszłości

---

## Pętla zwrotna

Po każdej fazie (i w jej trakcie):

- Jeśli nowa odpowiedź zmienia wniosek z poprzedniej fazy → **wróć, zaktualizuj plik MD tamtej fazy, zaznacz zmianę**
- Jeśli pojawia się pytanie które dotyczy wcześniejszej fazy → **otwórz tamten plik, dodaj jako "pytanie retrospektywne"**
- Materiały zewnętrzne (dokumenty, diagramy, kod) → **wskaż do której fazy należą i wprowadź jako dane wejściowe**

Format adnotacji retrospektywnej w pliku MD:

```markdown
## Korekty i uzupełnienia (retrospektywne)

[data/kontekst]: Wniosek W3 wymaga korekty po Fazie 2.
Poprzednio: [stary wniosek]
Aktualnie: [nowy wniosek]
Powód: [co zmieniło ocenę]
```

---

## Zasady zadawania pytań

- Jedno pytanie na raz — użytkownik odpowiada, dopiero wtedy następne
- Chyba że kontekst jest jasny — wtedy można zgrupować 2–3 powiązane
- Każde pytanie wynika z poprzedniej odpowiedzi lub otwartej niewiadomej
- Jeśli użytkownik odpowiada "nie wiem" → zaznacz jako otwartą niewiadomą i idź dalej
- Nie pytaj o rzeczy które można rozsądnie założyć z kontekstu
- Pytaj o fakty i decyzje — nie o opinie bez zakorzenienia w problemie

---

## Format każdego pliku MD

```markdown
# Faza [numer] — [nazwa]: [kontekst/domena]
Data: [opcjonalnie]

## Streszczenie poprzedniej fazy
[kluczowe wnioski które wpłynęły na tę fazę]
[otwarte pytania które zostały przeniesione]

## Pytania i odpowiedzi
Q1: [pytanie]
A1: [odpowiedź]

Q2: [pytanie]
A2: [odpowiedź]

...

## Wnioski
- [wniosek]

## Otwarte pytania / niewiadome
- [co pozostaje niejasne]

## Wejście do następnej fazy
[co z tej fazy przenosi się dalej]

## Korekty i uzupełnienia (retrospektywne)
[wypełniane gdy późniejsza faza coś zmienia]
```

---

## Relacja z innymi skillami

- Jeśli analiza schodzi do konkretnego refaktoru kodu → `legacy-refactor-assessment`
- Jeśli wynikiem jest implementacja DDD w .NET → `legacy-dotnet-ddd-refactoring`
- Jeśli potrzebna szybka analiza porównawcza (A vs B) bez głębokiego rozumienia → `analysis-assistant`
- Ten skill jest nadrzędny gdy cel to **zrozumienie**, nie tylko **wybór**
