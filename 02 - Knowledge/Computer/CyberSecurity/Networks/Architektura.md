---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - theme/default
  - status/in-progress
  - review/pending
date: 2025-08-04
last_updated: 2025-08-04
---

# Study: [[Network Main]]

## 🌐 Architektura Internetu

Architektura Internetu opisuje, jak dane są **organizowane**, **przesyłane** i **zarządzane** w sieciach. Różne modele architektury odpowiadają różnym potrzebom – niektóre oferują prosty układ **klient-serwer** (np. strona WWW), inne opierają się na podejściu **rozproszonym** (np. platformy do udostępniania plików). Zrozumienie tych modeli pozwala pojąć, dlaczego sieci projektuje się tak, a nie inaczej.

- Różne architektury rozwiązują różne problemy.
    
- Często spotyka się **hybrydowe** rozwiązania, łączące cechy kilku modeli.
    
- Każdy model ma swoje **zalety** i **wady** w zakresie skalowalności, wydajności, bezpieczeństwa i łatwości zarządzania.
    

---

## 🤝 Peer-to-Peer (P2P)

### 🔹 Co to jest?

W sieci **Peer-to-Peer (P2P)** każdy węzeł (komputer lub inne urządzenie) pełni rolę **zarówno klienta, jak i serwera**. Umożliwia to bezpośrednią wymianę zasobów (pliki, moc obliczeniowa, przepustowość) bez potrzeby centralnego serwera.

- **W pełni zdecentralizowane**: brak centralnego punktu koordynującego.
    
- **Częściowo scentralizowane**: centralny serwer koordynuje, ale nie przechowuje danych.
    

### 🖼️ Przykład

Grupa znajomych chce się wymieniać zdjęciami z wakacji:

1. Instalują aplikację P2P do udostępniania plików (np. BitTorrent).
    
2. Każdy wybiera folder ze zdjęciami do udostępnienia.
    
3. Po połączeniu wszyscy mogą przeglądać i pobierać zdjęcia **bezpośrednio** z udostępnionych folderów.
    

W systemie torrentowym każdy, kto ma plik (**seeder**), może go wysyłać, a pozostali pobierają części jednocześnie z wielu źródeł.
![[Pasted image 20250804161652.png]]

---

### 📈 Zalety i wady

| Zalety                      | Opis                                                                               |
| --------------------------- | ---------------------------------------------------------------------------------- |
| **Skalowalność**            | Dodanie kolejnych węzłów zwiększa zasoby (dyski, CPU, przepustowość).              |
| **Odporność**               | Gdy jeden węzeł zniknie, reszta nadal działa.                                      |
| **Podział kosztów**         | Obciążenie (przepustowość, przechowywanie) rozkłada się na wszystkich uczestników. |
| Wady                        | Opis                                                                               |
| **Złożoność zarządzania**   | Trudniej wprowadzać aktualizacje i polityki bezpieczeństwa w całej sieci.          |
| **Niska niezawodność**      | Zbyt wielu odchodzących użytkowników może pozbawić dostępu do zasobów.             |
| **Wyzwania bezpieczeństwa** | Każdy węzeł jest potencjalnym punktem ataku lub źródłem luk.                       |

---

## 🖥️ Architektura klient-serwer

Model **Client-Server** to jedna z najpopularniejszych architektur w Internecie.

- **Klienci** (urządzenia użytkowników) wysyłają żądania (np. przeglądarka prosi o stronę WWW).
    
- **Serwery** przetwarzają te żądania i zwracają odpowiedzi (np. treść strony).
    

![[Pasted image 20250804161818.png]]
### 🔍 Przykład: sprawdzanie pogody

1. Otwierasz przeglądarkę na telefonie/komputerze.
    
2. Wpisujesz adres `weatherexample.com` i naciskasz Enter.
    
3. Przeglądarka wysyła żądanie do serwera [WWW](http://WWW).
    
4. Serwer wyszukuje dane pogodowe i odsyła je jako stronę HTML/CSS/JS.
    
5. Twoja przeglądarka renderuje odebraną stronę – widzisz prognozę.

---

## 🏷️ Modele warstw (tiers)
|Model|Opis|
|---|---|
|**Single-Tier**|Klient, serwer i baza danych na jednej maszynie. Proste, ale słabo skalowalne i mniej bezpieczne.|
|**Two-Tier**|Warstwa prezentacji (klient) + warstwa danych (serwer DB). Desktopowe aplikacje z bezpośrednim połączeniem klient→DB.|
|**Three-Tier**|Dodaje warstwę **aplikacyjną** między klientem a bazą danych. Logika biznesowa na dedykowanym serwerze.|
|**N-Tier**|Więcej niż trzy warstwy: wiele serwerów aplikacyjnych, każdy z osobną rolą. Bardzo skalowalne.|

## ✅ Zalety vs. ⚠️ Wady klient-serwer

|Zalety|Opis|
|---|---|
|**Centralne zarządzanie**|Łatwiejsze wdrażanie aktualizacji i polityk bezpieczeństwa.|
|**Bezpieczeństwo**|Jednolita polityka bezpieczeństwa na serwerach.|
|**Optymalizacja wydajności**|Serwery dedykowane do konkretnych zadań (web, app, DB).|

|Wady|Opis|
|---|---|
|**Pojedynczy punkt awarii**|Awaria serwera przerywa dostęp dla wszystkich klientów.|
|**Wysokie koszty utrzymania**|Konfiguracja i eksploatacja serwerów wymaga zasobów i specjalistów.|
|**Kongestia sieci**|Duży ruch klientów może spowalniać lub zakłócać połączenia.|

---

## 🔀 Architektura hybrydowa

**Architektura hybrydowa** łączy elementy modeli **klient-serwer** oraz **Peer-to-Peer (P2P)**.  
W tym modelu:

- **Centralne serwery** odpowiadają za uwierzytelnianie użytkowników, zarządzanie sesjami oraz inne funkcje kontrolne.
    
- **Transfer danych** (np. pliki, multimedia) odbywa się **bezpośrednio między urządzeniami** użytkowników (peerami), co pozwala odciążyć serwery i zwiększyć wydajność.
    

---

### 🎥 Przykład: Aplikacja do wideokonferencji

1. **Logowanie i autoryzacja**  
    Użytkownik uruchamia aplikację i loguje się. Dane logowania (login, hasło) są weryfikowane przez centralny serwer.
    
2. **Rozpoczęcie spotkania**  
    Serwer koordynuje, kto bierze udział w spotkaniu, zarządza dostępem i informuje uczestników o nowym połączeniu.
    
3. **Transmisja danych**  
    W momencie rozpoczęcia rozmowy wideo, przesyłanie obrazu i dźwięku odbywa się **bezpośrednio między urządzeniami użytkowników**, z pominięciem serwera.  
    Dzięki temu ograniczane są opóźnienia i zwiększa się jakość połączenia.
![[Pasted image 20250806144032.png]]
### 📈 Zalety i wady

| Zaleta        | Opis                                                                           |
| ------------- | ------------------------------------------------------------------------------ |
| **Wydajność** | Serwer jest odciążany – dane przesyłane są bezpośrednio między peerami.        |
| **Kontrola**  | Centralny serwer nadal może zarządzać użytkownikami, indeksami lub katalogami. |

| Wada                    | Opis                                                                             |
| ----------------------- | -------------------------------------------------------------------------------- |
| **Złożoność wdrożenia** | Wymaga starannie zaprojektowanej integracji komponentów scentralizowanych i P2P. |
| **Punkt awarii**        | Awaria serwera centralnego może uniemożliwić logowanie lub inicjację sesji.      |

---

## ☁️ Architektura chmurowa (Cloud Architecture)

**Architektura chmurowa** odnosi się do infrastruktury obliczeniowej, która jest **hostowana i zarządzana przez zewnętrznych dostawców usług**, takich jak AWS, Microsoft Azure czy Google Cloud.  
Działa ona w modelu **klient-serwer** i oferuje **dostęp na żądanie** do zasobów takich jak serwery, przestrzeń dyskowa, aplikacje i bazy danych – wszystko za pośrednictwem Internetu.  
Użytkownicy **korzystają z usług**, nie mając dostępu do ani kontroli nad fizycznym sprzętem.

---

### 📦 Przykłady:

- **Google Drive**, **Dropbox** – aplikacje w modelu **SaaS** (Software as a Service), gdzie użytkownik korzysta z usługi przez przeglądarkę lub aplikację, nie martwiąc się o infrastrukturę.
### 🔑 Kluczowe cechy architektury chmurowej:

| Cecha                                     | Opis                                                                           |
| ----------------------------------------- | ------------------------------------------------------------------------------ |
| 1. **Samodzielna obsługa na żądanie**     | Możliwość automatycznego uruchamiania usług bez udziału człowieka.             |
| 2. **Szeroki dostęp sieciowy**            | Usługi dostępne z dowolnego urządzenia z dostępem do Internetu.                |
| 3. **Pula zasobów współdzielona**         | Dynamiczne przydzielanie zasobów pomiędzy wieloma użytkownikami.               |
| 4. **Szybka elastyczność (skalowalność)** | Możliwość dynamicznego zwiększania lub zmniejszania zasobów.                   |
| 5. **Usługa mierzona**                    | Płacisz tylko za rzeczywiste zużycie zasobów (np. moc obliczeniowa, transfer). |

### 📈 Zalety i wady

#### ✅ Zalety:

|Zaleta|Opis|
|---|---|
|**Skalowalność**|Łatwe dostosowanie zasobów do potrzeb.|
|**Niższe koszty utrzymania**|Sprzęt zarządzany przez dostawcę.|
|**Elastyczność**|Dostęp do usług z dowolnego miejsca na świecie.|

#### ❌ Wady:

|Wada|Opis|
|---|---|
|**Uzależnienie od dostawcy (vendor lock-in)**|Trudności przy migracji do innego dostawcy chmury.|
|**Bezpieczeństwo i zgodność**|Przekazanie danych stronie trzeciej może budzić obawy o prywatność.|
|**Wymóg połączenia z Internetem**|Brak stabilnego połączenia uniemożliwia korzystanie z usług.|

---
## 🧠 Architektura zdefiniowana programowo (SDN – Software-Defined Networking)

**SDN** to nowoczesne podejście do budowy sieci, które **oddziela warstwę kontrolną (Control Plane)** od **warstwy przesyłu danych (Data Plane)**.

- Tradycyjnie oba elementy były zintegrowane w jednym urządzeniu (np. ruterze).
    
- W **SDN** warstwa kontrolna jest **centralizowana w kontrolerze programowym**, który podejmuje decyzje o trasowaniu ruchu.
    
- Urządzenia sieciowe (np. przełączniki) jedynie **wykonują polecenia** otrzymywane od kontrolera.
    

### 📈 Zalety i wady

#### ✅ Zalety:

| Zaleta                              | Opis                                                                              |
| ----------------------------------- | --------------------------------------------------------------------------------- |
| **Centralne sterowanie**            | Upraszcza zarządzanie siecią.                                                     |
| **Programowalność i automatyzacja** | Możliwość zmiany konfiguracji sieci przez oprogramowanie, bez ręcznego działania. |
| **Skalowalność i wydajność**        | Możliwość dynamicznego optymalizowania tras i zasobów.                            |

#### ❌ Wady:

|Wada|Opis|
|---|---|
|**Wrażliwość na awarie kontrolera**|Utrata kontrolera może zakłócić pracę całej sieci.|
|**Złożoność wdrożenia**|Wymaga specjalistycznej wiedzy i nowej infrastruktury.|

---

### 💼 Przykład zastosowania:

Duże firmy i dostawcy chmur wykorzystują SDN do dynamicznego przydzielania pasma, sterowania ruchem w czasie rzeczywistym i automatyzacji zarządzania politykami sieciowymi.

## 📊 Porównanie architektur sieciowych

| Architektura      | Centralizacja               | Skalowalność                  | Łatwość zarządzania                | Typowe zastosowania               |
| ----------------- | --------------------------- | ----------------------------- | ---------------------------------- | --------------------------------- |
| **P2P**           | Zdecentralizowana           | Wysoka (wraz z liczbą peerów) | Złożone (brak kontroli centralnej) | Udostępnianie plików, blockchain  |
| **Klient-Serwer** | Centralna                   | Umiarkowana                   | Łatwa (dzięki serwerowi)           | Strony internetowe, e-mail        |
| **Hybrydowa**     | Częściowo centralna         | Wyższa niż klient-serwer      | Bardziej złożona                   | Komunikatory, wideokonferencje    |
| **Chmurowa**      | Centralna u dostawcy        | Wysoka                        | Łatwa (outsourcing)                | SaaS, PaaS, przechowywanie danych |
| **SDN**           | Centralna warstwa kontrolna | Wysoka (policy-driven)        | Średnia (wymaga narzędzi)          | Centra danych, duże sieci firmowe |

---
## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)