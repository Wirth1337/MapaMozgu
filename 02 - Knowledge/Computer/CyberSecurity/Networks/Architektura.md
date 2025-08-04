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




## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)