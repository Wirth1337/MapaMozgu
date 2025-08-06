---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - theme/default
  - status/in-progress
  - review/pending
date: 2025-08-06
last_updated: 2025-08-06
---

# Study: [[Network Main]]

## 🛡️ Firewall

**Firewall** to urządzenie (hardware, software lub hybryda), które **monitoruje ruch sieciowy** i **wdraża zasady** (polityki, ACL) określające, które pakiety można przepuścić, a które zablokować.  
Wyobraź to sobie jak strażnika przy wejściu do budynku – wpuszcza tylko tych, którzy spełniają reguły.

### 🔍 Jak działa?

- Analiza pakietów na podstawie:
    
    - Adresów IP źródła i celu
        
    - Numerów portów
        
    - Protokołu (TCP/UDP)
        
- Filtracja ruchu: zezwól lub odrzuć
    
- Logowanie zdarzeń i alertowanie
    

### 📂 Typy firewalli

| Typ                           | Warstwa OSI                 | Funkcjonalność                                                               | Przykład                                                    |
| ----------------------------- | --------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Packet Filtering**          | 3 (Network) & 4 (Transport) | Sprawdza IP, porty i protokół                                                | ACL routera                                                 |
| **Stateful Inspection**       | 3 & 4                       | Śledzi kontekst połączeń, dopuszcza tylko odpowiedź na inicjatywę wychodzącą | Pozwól przychodzące tylko do wcześniej nawiązanych połączeń |
| **Application Layer (Proxy)** | 7 (Application)             | Analiza zawartości (np. HTTP), blokowanie złośliwych żądań                   | Proxy filtrujące HTTP                                       |
| **Next-Generation (NGFW)**    | 3–7                         | Statefulness + DPI, IDS/IPS, kontrola aplikacji                              | Blokowanie znanych złośliwych adresów, inspekcja TLS        |
![[Pasted image 20250806151528.png]]
---

## 🕵️ IDS/IPS

**Intrusion Detection System (IDS)**

- Monitoruje ruch i zdarzenia, **wykrywa** anomalie lub sygnatury ataków, **generuje alerty**.
    

**Intrusion Prevention System (IPS)**

- Podobnie do IDS, ale **aktywnie blokuje** złośliwy ruch w czasie rzeczywistym.
    

### 🔍 Techniki detekcji

- **Signature-based** – porównanie z bazą znanych exploitów
    
- **Anomaly-based** – wykrywanie odchyleń od normalnego zachowania
    

### 📂 Typy IDS/IPS

| Typ           | Lokalizacja           | Opis                                                     | Przykład                                   |
| ------------- | --------------------- | -------------------------------------------------------- | ------------------------------------------ |
| **NIDS/NIPS** | W sieci (core, brama) | Sensor/urządzenie analizujące cały przepływ pakietów     | Sensor przy przełączniku rdzeniowym        |
| **HIDS/HIPS** | Na hoście (serwer/PC) | Agent monitorujący ruch i logi na pojedynczym urządzeniu | Oprogramowanie antywirusowe z modułem HIPS |

### 🔀 Umiejscowienie w architekturze

1. **Za firewallem** – firewall odfiltrowuje podstawowe zagrożenia, IDS/IPS bada resztę ruchu
    
2. **W DMZ** – monitorowanie ruchu do serwerów publicznych
    
3. **Na hoście** – wykrywanie ataków lokalnych na serwerze/komputerze
![[Pasted image 20250806151602.png]]
---

| Praktyka                          | Opis                                                                    |
| --------------------------------- | ----------------------------------------------------------------------- |
| **Jasne polityki**                | Zasada najmniejszego uprzywilejowania – pozwól tylko temu, co konieczne |
| **Regularne aktualizacje**        | Firewall, IDS/IPS, sygnatury, OS – zawsze najnowsze łaty                |
| **Monitorowanie i logi**          | Regularna analiza logów firewalli i alertów IDS/IPS                     |
| **Wielowarstwowe zabezpieczenia** | Obrona w głębi – firewall + IDS/IPS + antywirus + EDR                   |
| **Testy penetracyjne**            | Symulowane ataki, by zweryfikować skuteczność polityk i urządzeń        |

## References
- [Reference 1](link)
- [Reference 2](link)