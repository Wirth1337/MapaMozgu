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


## 📡 MAC Address

### 🔹 Co to jest?

MAC (Media Access Control) address to **unikalny identyfikator** przypisany do **karty sieciowej (NIC)** urządzenia. Pozwala on na jednoznaczną identyfikację urządzenia w sieci lokalnej.

### 🧠 Warstwa OSI

- Działa na **warstwie 2** – _Data Link Layer_.
    
- Kluczowy w komunikacji **wewnątrz segmentu sieci lokalnej (LAN)**.
    

### 🧩 Budowa adresu MAC

- **Długość**: 48 bitów
    
- **Format**: 6 par znaków szesnastkowych, oddzielonych dwukropkami lub myślnikami  
    np. `00:1A:2B:3C:4D:5E`
    

#### 📦 Struktura:

- **Pierwsze 24 bity**: _Organizationally Unique Identifier (OUI)_ – identyfikator producenta
    
- **Ostatnie 24 bity**: numer seryjny urządzenia
    

### 🌍 Cechy

- Zaprojektowany tak, aby był **globalnie unikalny**
    
- Zapobiega konfliktom adresów w sieci

![[Pasted image 20250804151536.png]]

---
## 🔁 How MAC Addresses Are Used in Network Communication

### 🖧 Rola w sieci LAN

Adresy MAC są **kluczowe w komunikacji lokalnej** (LAN), ponieważ umożliwiają **dostarczanie ramek danych do właściwego urządzenia fizycznego**.

- Ramka danych zawiera adres MAC **docelowy**, co pozwala przełącznikowi (switchowi) przesłać dane do odpowiedniego portu.
    
- Adresy MAC nie są routowalne – działają tylko w obrębie jednej sieci lokalnej.
    

### 🧭 ARP (Address Resolution Protocol)

- ARP to **protokół tłumaczący adresy IP na adresy MAC**.
    
- Umożliwia urządzeniom odnalezienie fizycznego adresu MAC przypisanego do znanego adresu IP.
    

### 📘 Przykład komunikacji

Załóżmy, że dwa komputery są podłączone do tego samego switcha:

|Komputer|IP Address|MAC Address|
|---|---|---|
|A|`192.168.1.2`|`00:1A:2B:3C:4D:5E`|
|B|`192.168.1.5`|`00:1A:2B:3C:4D:5F`|
![[Pasted image 20250804151603.png]]

## 🌐 IP Addresses

### 🔹 Co to jest?

**Internet Protocol (IP) address** to **numeryczna etykieta** przypisana każdemu urządzeniu korzystającemu z protokołu IP.

- Działa na **warstwie 3** – _Network Layer_ OSI.
    
- Umożliwia lokalizację i komunikację urządzeń między różnymi sieciami.
    

### 🧩 Wersje

- **IPv4**
    
    - 32-bitowy adres.
        
    - Format: cztery liczby dziesiętne (0–255) oddzielone kropkami, np. `192.168.1.1`.
        
- **IPv6**
    
    - 128-bitowy adres.
        
    - Format: osiem grup po cztery cyfry szesnastkowe, np.  
        `2001:0db8:85a3:0000:0000:8a2e:0370:7334`.
        

---

## 🔁 Wykorzystanie adresów IP w komunikacji

- **Routing**  
    Routery analizują docelowy adres IP, by wybrać **optymalną ścieżkę** przez wiele sieci.
    
- **Elastyczność**  
    – IP może być **dynamicznie przydzielane** (DHCP) lub stałe (static).  
    – W odróżnieniu od [[MAC Address]], IP nie jest „wypalone” w sprzęcie.
    

---

## 🔌 Ports (porty)

### 🔹 Co to jest?

Port to **numer** (0–65535) przypisany procesowi/usłudze, pozwalający na **wielowątkowość** ruchu na jednym IP.

- Działa na **warstwie 4** – _Transport Layer_ OSI.
    
- Protokóły: **TCP**, **UDP**.
    

### 📝 Klient vs. Serwer

- **Klient**: inicjuje połączenie, wybiera **źródłowy port** (ephemeral).
    
- **Serwer**: nasłuchuje na znanym **portu docelowym**, odpowiada na przychodzące zapytania.
    

### 📑 Kategorie portów

| Zakres      | Nazwa                       | Przykłady                                      |
| ----------- | --------------------------- | ---------------------------------------------- |
| 0–1023      | Well-Known (systemowe)      | 80 = HTTP, 443 = HTTPS, 20/21 = FTP            |
| 1024–49151  | Registered (rejestrowane)   | 1433 = MS SQL Server, 3306 = MySQL, 3389 = RDP |
| 49152–65535 | Dynamic/Private (ephemeral) | Losowo przydzielane klientom                   |
## Przykład użycia `netstat`

`netstat -an`

![[Pasted image 20250804151912.png]]

---

## 🌐 Przykład: Przeglądanie Internetu (Web Request Flow)

### 1. DNS Lookup

- Przeglądarka tłumaczy nazwę domeny (np. `example.com`) na adres IP
    
    - Przykład: `93.184.216.34`
        

### 2. Data Encapsulation

1. Przeglądarka generuje **żądanie HTTP**.
    
2. Żądanie jest **opakowane w TCP**, wskazując port docelowy:
    
    - `80` (HTTP) lub `443` (HTTPS)
        
3. Do pakietu dodawany jest nagłówek IP z docelowym adresem `93.184.216.34`.
    
4. W LAN komputer używa **ARP**, aby poznać MAC bramy (domyślnego routera).
    

### 3. Data Transmission

- Komputer wysyła **ramkę Ethernet** z własnym MAC jako źródło i MAC routera jako cel.
    
- Router odbiera ramkę, usuwa nagłówek Ethernet i przekazuje pakiet IP dalej wg adresu IP.
    
- Po drodze kolejne routery przekazują pakiet na podstawie tablic routingu.
    

### 4. Server Processing

- Serwer docelowy odbiera pakiet IP.
    
- Na warstwie transportowej przekazuje dane do aplikacji nasłuchującej na porcie `80` lub `443`.
    
- Aplikacja HTTP generuje odpowiedź.
    

### 5. Response Transmission

1. Serwer wysyła odpowiedź **z powrotem** do klienta:
    
    - Docelowym adresem IP/portem jest adres i **tymczasowy port** klienta (wybrany losowo).
        
2. Pakiet wraca przez Internet, router po routerze, aż do LAN klienta.
    
3. W LAN router opakowuje pakiet w ramkę Ethernet do MAC klienta.
    
4. Klient odbiera i przetwarza odpowiedź HTTP.


## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)