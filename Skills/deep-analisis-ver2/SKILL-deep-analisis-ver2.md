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

**Pytania orientacyjne (wybierz te które nie są znane):**
- Co jest przedmiotem analizy (system, proces, decyzja)?
- Kto jest odbiorcą wyników — techniczni, biznesowi, oboje?
- Co już wiadomo, co jest pewne?
- Gdzie są największe niewiadome?
- Jaki poziom szczegółowości jest potrzebny — tylko Big Picture, do Process Level, pełna analiza?

**Pytanie o materiały wejściowe:**
Czy istnieje gotowy Event Storming lub inny materiał analityczny?
Jeśli tak → przejdź do sekcji **"Wejście z gotowym materiałem"** zanim przejdziesz dalej.

---

### Tryby pracy — wybór przed Fazą 1

Po orientacji, przed pierwszym pytaniem merytorycznym, zaproponuj użytkownikowi tryb pracy.
Opisz krótko każdy tryb i zapytaj który wybiera. Tryb można zmienić w dowolnym momencie.

#### Tryb 1 — Agentowy (Claude orkiestruje)

Claude dzieli domenę na konteksty i przydziela je agentom wewnętrznym.
Każdy agent odpytuje swój wycinek. Główny wątek orkiestruje i pilnuje spójności między agentami.
Użytkownik obserwuje, może wstrzymać proces, zadać pytanie, skoryguować kierunek w dowolnym momencie.

Kiedy warto: duża domena, wiele kontekstów, użytkownik chce szybkiego pokrycia i ogląda wyniki.

#### Tryb 2 — Ekspercki (użytkownik jako ekspert domenowy)

Claude jest orkiestratorem i zadaje pytania. Użytkownik odpowiada dla każdego kontekstu i fazy.
Claude śledzi spójność między odpowiedziami i sygnalizuje konflikty.

Kiedy warto: użytkownik ma głęboką wiedzę domenową i chce ją wyekstrahować krok po kroku.

#### Tryb 3 — Mixed (współpraca)

Część kontekstów trafia do agentów, jeden lub więcej kontekstów użytkownik prowadzi sam
(z pytaniami orkiestratora). Główny wątek uspójnia wyniki z obu źródeł.
Możliwa sytuacja: ciężki, wrażliwy kontekst → użytkownik. Reszta → agenci.

Kiedy warto: niejednorodna domena, część obszarów jest wrażliwa lub wymaga wiedzy której agenci nie mają.

#### Tryb 4 — Użytkownik jako orkiestrator

Użytkownik decyduje co i kiedy badać. Agenci dostają konteksty od użytkownika.
Claude (główny wątek) wspiera, ale nie prowadzi — reaguje na pytania i pilnuje spójności na żądanie.

Kiedy warto: użytkownik ma gotowy plan analizy i chce narzędzia, nie prowadzenia.

**Przejście między trybami:**
W każdym momencie użytkownik może powiedzieć "przełącz na tryb X" lub "przejmij to agentami" lub "dalej sam poprowadzę".
Orkiestrator potwierdza przełączenie i kontynuuje od aktualnego punktu bez powtarzania.

---

### Faza 1 — Big Picture

**Odpowiednik ES:** Big Picture Event Storming
**Cel:** Zarys całości. Przestrzeń problemu, aktorzy, kluczowe zdarzenia domenowe, hotspoty, wstępne granice.
Nie schodzimy tu do szczegółów procesu — pytania dotyczą całego obrazu, nie jednego toru.

---

#### 1a. Wejście z gotowym ES (jeśli istnieje)

Jeśli użytkownik dostarcza gotowy Event Storming:

1. Oceń co jest pokryte: zdarzenia, aktorzy, hotspoty, granice
2. Zapytaj: **"Czy chcesz przejść przez istniejące zdarzenia jedno po drugim i uzupełnić brakujące elementy na poziomie Big Picture?"**
   - Jeśli tak → przechodzisz zdarzenie po zdarzeniu, dla każdego pytasz: co brakuje, co jest niejasne, czy hotspot jest zaadresowany
   - Jeśli nie → identyfikujesz luki zbiorczo i pytasz tylko o nie
3. Nie powtarzaj tego co jest — **buduj na istniejącym materiale**
4. Zaznacz w pliku MD które elementy pochodzą z ES a które zostały dodane w tej sesji

---

#### 1b. Pytanie o bleed-through z wyższych faz

Przed przejściem do pytań merytorycznych zapytaj o zakres tej fazy.
ES nie ma sztywnych granic — użytkownik decyduje czy elementy z Process Level lub Design Level
mają przesiąknąć do Big Picture.

Zaproponuj:

```
Big Picture bazowe (domyślne):
  zdarzenia domenowe, aktorzy, zewnętrzne systemy, hotspoty, granice domeny

Opcjonalnie można dodać (zapytaj czy chcesz):
  [+] Read modele — co jest wyświetlane / prezentowane użytkownikowi w kluczowych momentach
  [+] Ogólne zasady biznesowe — reguły które wpływają na przepływ, bez szczegółów implementacji

Nie wnosimy na tym etapie:
  [–] Komendy (to Process Level / Design Level)
  [–] Polityki i reakcje na zdarzenia (to Design Level)
  [–] Agregaty i niezmienniki (to Design Level)
```

Uzasadnienie: Read modele i ogólne zasady mogą pomóc odbiorcom biznesowym lepiej zrozumieć proces.
Ale nie pchaj tego jeśli cel to tylko wstępne rozpoznanie domeny.
Jeśli celem jest prezentacja dla biznesu — read modele są często wartościowe już na tym etapie.

---

#### 1c. Obszary pytań Big Picture

Pytania są szerokie na początku, zawężają się w kierunku spójności całości.
Każde pytanie wynika z poprzedniej odpowiedzi.

**Blok 1 — Przestrzeń problemu (szerokie, otwierające):**
```
1. Co się dzieje w tej domenie — opisz słowami co tu robimy, bez technicznych szczegółów?
2. Jakie zdarzenia są absolutnie kluczowe — bez których domena nie istnieje?
3. Kto lub co inicjuje te zdarzenia (ludzie, czas, zewnętrzne systemy)?
4. Co jest efektem końcowym — co domena produkuje lub zmienia w świecie?
5. Co jest poza granicami tej domeny — co świadomie ignorujemy?
```

**Blok 2 — Hotspoty i napięcia:**
```
6. Gdzie są miejsca bólu, tarcia, niejasności — gdzie ludzie się kłócą lub nie wiedzą co powinno się stać?
7. Gdzie procesy się rwą lub wymagają ręcznej interwencji?
8. Jakie zależności zewnętrzne są krytyczne (inne systemy, inne domeny)?
```

**Blok 3 — Spójność obrazu (weryfikujące):**
```
9. Czy każde kluczowe zdarzenie ma swojego inicjatora?
10. Czy każde zdarzenie ma skutek — czy wiemy co się po nim dzieje?
11. Czy są obszary domeny które jeszcze nie padły — może brakuje kawałka mapy?
```

**Blok opcjonalny — Read modele (jeśli wybrano bleed-through):**
```
12. W których momentach procesu ktoś musi coś zobaczyć żeby podjąć decyzję?
13. Co musi być widoczne i dla kogo?
```

---

#### 1d. Decyzja o przejściu

Użytkownik potwierdza czy Big Picture jest wystarczająco pełny.

Pytaj: "Czy obraz całości jest wystarczający by przejść do poziomu procesu, czy coś wymaga doprecyzowania?"

Jeśli cel analizy to tylko Big Picture (np. prezentacja dla biznesu) → wygeneruj plik MD i zakończ lub przejdź do faz S1–S3 jeśli wybrano.

---

### Faza 2 — Process Level

**Odpowiednik ES:** Process Level Event Storming
**Cel:** Odkrycie subdomen i ich typów, wyznaczenie granic kontekstów, zidentyfikowanie pivotal events,
zarysowanie ciągu zdarzeń jako procesu, odkrycie core domain. Opcjonalnie: wstępne reguły biznesowe.

**Zaczyna się od streszczenia Fazy 1** — wypisz kluczowe wnioski, hotspoty i otwarte pytania.

---

#### 2a. Pytanie o bleed-through z Design Level

Przed przejściem do pytań merytorycznych zapytaj:

```
Process Level bazowe (domyślne):
  subdomeny, ich typy, pivotal events, przepływ procesu, granice kontekstów, core domain

Opcjonalnie można dodać (zapytaj czy chcesz):
  [+] Wstępne reguły biznesowe — zasady które wpływają na przepływ między subdomenami
      (nie agregaty, nie komendy — tylko zasady na poziomie "co musi być prawdą żeby X nastąpiło")

Nie wnosimy na tym etapie:
  [–] Komendy i polityki (to Design Level)
  [–] Agregaty i niezmienniki (to Design Level)
  [–] Szczegóły implementacji
```

Uzasadnienie: Reguły na poziomie procesu pomagają zrozumieć gdzie są naturalne granice kontekstów
i gdzie mogą być konflikty własności. Ale nie zmuszaj — to decyzja użytkownika.

---

#### 2b. Odkrycie subdomen — heurystyki

Subdomeny odkrywamy przez pytania, nie przez zakładanie. Używaj tych heurystyk:

**Heurystyka 1 — Językowa:**
```
Czy różne grupy ludzi używają różnych słów na ten sam byt?
(np. "zamówienie" dla sprzedaży vs "zlecenie" dla produkcji)
→ sygnał granicy kontekstu
```

**Heurystyka 2 — Właścicielska:**
```
Kto jest właścicielem tej części procesu — kto decyduje gdy coś idzie nie tak?
Gdzie zmiana jednej reguły nie powinna wymagać zmiany w drugiej części?
→ sygnał granicy subdomeny
```

**Heurystyka 3 — Zmienności:**
```
Co zmienia się często a co rzadko?
Co zmienia się niezależnie od reszty?
→ co zmienia się razem, prawdopodobnie należy do tej samej subdomeny
```

**Heurystyka 4 — Skupienia uwagi:**
```
Gdzie biznes inwestuje najwięcej uwagi, czasu, zasobów?
Gdzie błąd jest najdroższy?
→ tam prawdopodobnie jest core domain
```

**Pytania odkrywające subdomeny:**
```
1. Gdzie w procesie zmienia się język — te same rzeczy nazywane inaczej przez różne grupy?
2. Gdzie jest naturalne "cięcie" — co można wyłączyć bez wpływu na resztę?
3. Które części procesu mogłyby działać jako oddzielny system lub zespół?
4. Gdzie zmiany wymagań najczęściej generują zmiany w wielu miejscach jednocześnie?
```

---

#### 2c. Typologia subdomen

Po odkryciu kandydatów na subdomeny, dla każdej zapytaj:

```
Core domain (domena główna):
  Czy to jest to co odróżnia nas od konkurencji?
  Czy tu inwestujemy w unikalną logikę biznesową?
  → Tu budujemy, nie kupujemy. Tu jest Rich Domain Model.

Supporting domain (domena wspierająca):
  Czy to wspiera core ale samo w sobie nie jest przewagą?
  Czy moglibyśmy to kupić lub outsource'ować?
  → Budujemy prosto, CRUD jest OK.

Generic domain (domena generyczna):
  Czy to jest coś co każda firma robi tak samo?
  (np. wysyłka emaili, płatności, logowanie)
  → Kupujemy gotowe rozwiązanie lub używamy SaaS.
```

Dla każdej kandydatury na subdomenę: nazwij → określ typ → uzasadnij.

---

#### 2d. Pivotal Events

Pivotal events to zdarzenia które zmieniają stan domeny w sposób nieodwracalny lub
wyraźnie rozdzielają fazy procesu. Często wyznaczają naturalne granice kontekstów.

Pytania:
```
1. Które zdarzenia są punktem bez powrotu — po nich coś zmienia się na zawsze?
2. Które zdarzenia wyraźnie kończą jedną fazę a zaczynają kolejną?
3. Gdzie jest granica odpowiedzialności — po tym zdarzeniu inny zespół/system przejmuje?
4. Które zdarzenia są widoczne dla klienta / zewnętrznego obserwatora?
```

Zaznacz pivotal events wyraźnie na mapie fazy — to naturalni kandydaci na granice BC.

---

#### 2e. Zarysowanie procesu jako ciągu

Po odkryciu subdomen i pivotal events przejdź przez cały przepływ:

```
1. Jakie jest zdarzenie inicjujące cały proces?
2. Co się dzieje krok po kroku — od inicjacji do zakończenia?
3. Gdzie są rozgałęzienia (alternatywne ścieżki)?
4. Gdzie subdomeny przekazują sobie sterowanie lub dane?
5. Jakie są zależności między subdomenami — kto czeka na kogo?
6. Gdzie mogą być konflikty własności danych (dwie subdomeny chcą tego samego)?
7. Gdzie jest przepływ odwrotny (błędy, kompensacje, cofnięcia)?
```

---

#### 2f. Core Domain — decyzja

Na podstawie zebranych informacji zidentyfikuj core domain.
Jeśli nie jest oczywista, zapytaj:

```
1. Za co klienci płacą — co jest unikalną wartością?
2. Czego nie możemy kupić gotowego bo to nasza przewaga?
3. Gdzie błąd jest najdroższym błędem biznesowym?
4. Co wymaga największego nakładu wiedzy domenowej do zaprojektowania?
```

Wynik: nazwana core domain + uzasadnienie. Zaznacz w pliku MD.

---

#### 2g. Opcjonalne reguły biznesowe (jeśli wybrano bleed-through)

Jeśli użytkownik wybrał bleed-through z Design Level:

Pytaj o reguły które wpływają na granice lub przepływ między kontekstami:
```
1. Co musi być prawdą żeby [subdomena A] mogła przekazać do [subdomeny B]?
2. Jakie warunki muszą być spełnione żeby pivotal event mógł nastąpić?
3. Czy są zasady które obejmują wiele subdomen jednocześnie?
```

Zapisuj jako "reguła procesu: [treść]" — bez przypisania do agregatu czy polityki.
Te reguły będą wejściem do Design Level.

---

#### 2h. Spójność procesu — pytania weryfikujące

Na końcu fazy (lub w jej trakcie gdy jest sygnał niespójności):

```
1. Czy każda subdomena ma właściciela (kto decyduje gdy jest problem)?
2. Czy każdy pivotal event jest osiągalny — czy istnieje ścieżka która do niego prowadzi?
3. Czy nie ma wysp — subdomen które nie komunikują się z resztą?
4. Czy nie ma duplikatów — tych samych pojęć w różnych subdomenach bez uzgodnienia?
5. Czy przepływ ma koniec — co jest ostatecznym zdarzeniem procesu?
```

---

**Uwaga o skali:**
Jeśli domena jest bardzo rozległa, Faza 2 może być podzielona:
- 2.1 — mapowanie przepływu głównego i pivotal events
- 2.2 — odkrycie subdomen i typologia
- 2.3 — granice kontekstów, zależności, core domain

Podział decyduje użytkownik po obejrzeniu zakresu.

**Decyzja o przejściu:**
"Czy mamy wystarczający obraz procesu, subdomen i core domain by schodzić do poziomu projektowania,
czy coś wymaga doprecyzowania?"

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
