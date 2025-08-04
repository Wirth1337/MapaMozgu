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

#  [[Network Main]]

| Komponent | Opis |
|-----------|------|
| **Urządzenia końcowe** | Komputery, smartfony, tablety, urządzenia IoT / inteligentne urządzenia. |
| **Urządzenia pośredniczące** | Przełączniki (switch), routery, modemy, punkty dostępowe (access point). |
| **Media i komponenty programowe sieci** | Kable, protokoły, oprogramowanie do zarządzania i zapory sieciowe (firewalle). |
| **Serwery** | Serwery WWW, plików, poczty e-mail, baz danych. |


---
## Urządzenia pośredniczące

**Rola**: Pośredniczą w przepływie danych między urządzeniami końcowymi – zarówno w ramach jednej sieci (LAN), jak i między różnymi sieciami (np. internetem).

### 🔁 Główne funkcje

- **Forwarding (przekazywanie pakietów)** na podstawie adresów.
    
- **Zarządzanie ruchem** (traffic management) – optymalizacja ścieżek przesyłu danych.
    
- **Zapobieganie przeciążeniom**.
    
- **Bezpieczeństwo** – zapory ogniowe, listy kontroli dostępu.
    
- **Obsługa protokołów i warstw OSI**:
    
    - Router: Layer 3 (Network)
        
    - Switch: Layer 2 (Data Link)
        

---

## 🔌 Network Interface Card (NIC)

**Opis**: Karta sieciowa umożliwiająca urządzeniu fizyczne połączenie z siecią.

- **Warstwa OSI**: Data Link (Layer 2)
    
- **Adres MAC**: unikalny identyfikator urządzenia.
    
- **Rodzaje**:
    
    - **Wired NIC** – np. Ethernet.
        
    - **Wireless NIC** – Wi-Fi adaptery.
        

**Przykład**:

- Komputer stacjonarny z przewodowym NIC (Ethernet).
    
- Laptop z bezprzewodowym NIC (Wi-Fi).
    

---

## 🌐 Router

**Funkcja**: Przekierowuje pakiety między różnymi sieciami (np. LAN ↔ Internet).

- **Warstwa OSI**: Network Layer (Layer 3).
    
- **Używa**: Routing tables + protokoły jak OSPF, BGP.
    
- **Zarządzanie ruchem**: wybór optymalnych ścieżek.
    
- **Zabezpieczenia**: firewall, ACL (Access Control Lists).
    

**Przykład**:  
Router domowy łączy wszystkie urządzenia (komputery, smartfony, TV) z Internetem.

---

## 🔀 Switch

**Funkcja**: Łączy urządzenia wewnątrz jednej sieci LAN, przekazując dane tylko do odbiorcy.

- **Warstwa OSI**: Data Link (Layer 2).
    
- **Używa**: adresów MAC.
    
- **Zalety**: zmniejsza kolizje i poprawia wydajność sieci.
    

**Przykład**:  
W biurze switch łączy komputery pracowników, drukarki i serwery.

---

## ⚙️ Hub (przestarzały)

**Opis**: Proste urządzenie rozgłaszające dane do wszystkich portów bez filtrowania.

- **Warstwa OSI**: Physical (Layer 1).
    
- **Wady**: brak inteligentnego zarządzania ruchem → kolizje i niska wydajność.
    

**Przykład**:  
Dawne domowe sieci z kilkoma komputerami – dziś zastępowane switchami.

---

## 🧱 Network Media & Software Components

Sieć to nie tylko sprzęt – to także fizyczne nośniki transmisji i oprogramowanie sterujące komunikacją. Oba elementy są kluczowe dla działania i bezpieczeństwa sieci.

### 🔌 Network Media (Nośniki transmisji)

**Funkcja**: Tworzą fizyczną (lub bezprzewodową) drogę transmisji danych między urządzeniami.

#### 🔧 Przewodowe

- **Ethernet (skrętka)** – np. Cat5e, Cat6.
    
- **Światłowody** – bardzo szybkie, odporne na zakłócenia.
    

#### 📡 Bezprzewodowe

- **Wi-Fi** – mobilność, wygoda.
    
- **Bluetooth** – krótkodystansowa komunikacja urządzeń.
    

---

### 🔌 Cabling & Connectors (Okablowanie i złącza)

- **Przykład**: RJ-45 – standardowy wtyk dla Ethernetu.
    
- Jakość kabla i złącza wpływa na:
    
    - prędkość,
        
    - niezawodność,
        
    - odporność na zakłócenia.
        

---

### 📜 Network Protocols (Protokoły sieciowe)

**Funkcja**: Ustalają zasady formatowania, przesyłu, adresowania i interpretacji danych.

#### Główne funkcje:

- Segmentacja danych
    
- Adresowanie
    
- Routing
    
- Sprawdzanie błędów
    
- Synchronizacja transmisji
    

#### Przykładowe protokoły:

| Protokół       | Zastosowanie             |
| -------------- | ------------------------ |
| **TCP/IP**     | Komunikacja w internecie |
| **HTTP/HTTPS** | Przeglądanie WWW         |
| **FTP**        | Transfer plików          |
| **SMTP**       | Wysyłka e-maili          |
### Topic 2
Main content here...

## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)