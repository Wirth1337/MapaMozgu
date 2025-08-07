---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - theme/default
  - status/in-progress
  - review/pending
date: 2025-08-07
last_updated: 2025-08-07
---

# Study: [[Network Main]]

## 🧠 Proxy – Zrozumienie Systemowe

> **Proxy** to pośrednik w komunikacji sieciowej, działający zazwyczaj na warstwie 7 modelu OSI, który **przechwytuje, analizuje i ewentualnie modyfikuje ruch sieciowy** między klientem a serwerem.

---
### 🔑 Kluczowe pojęcia (dla systemowego myślenia)

| Pojęcie              | Opis systemowy                                                                                |
| -------------------- | --------------------------------------------------------------------------------------------- |
| **Mediator**         | Urządzenie/serwis pośredniczące w komunikacji, które ma zdolność **analizy ruchu**            |
| **Granica systemu**  | Proxy tworzy nową granicę między segmentami systemu (np. użytkownik – Internet)               |
| **Warstwa OSI**      | Proxy zazwyczaj działa na **Layer 7** – warstwa aplikacji, analizuje treść pakietów           |
| **Świadomość proxy** | Aplikacje (i malware) mogą być proxy-aware – potrafią wykryć i wykorzystać konfigurację proxy |
| **Obserwowalność**   | Proxy umożliwia **inspekcję ruchu**, co zwiększa zdolność monitorowania i reagowania          |
| **Dezintermediacja** | Przeciwdziałanie proxy, np. przez tunelowanie lub używanie DNS jako kanału C2                 |

---

### 🧩 Typy Proxy

#### 🟦 **Forward Proxy (Proxy dedykowane)**

- **Opis**: Klient wysyła żądanie do proxy, a ono przekazuje je dalej.
    
- **Zastosowanie**:
    
    - Ochrona komputerów przed bezpośrednim dostępem do Internetu (web filtering)
        
    - Burp Suite – narzędzie testowe dla aplikacji webowych
        
- **Zalety systemowe**:
    
    - Kontrola ruchu wychodzącego
        
    - Ochrona przed malware (które musiałoby być "proxy-aware")
        
- **Przykłady**:
    
    - BurpSuite, WebSense, proxy korporacyjne
        

📌 **Systemowa analogia**: Jak firewall, ale bardziej inteligentny – potrafi analizować _co_ wychodzi, nie tylko _gdzie_.

![[Pasted image 20250807140012.png]]
---

#### 🟩 **Reverse Proxy**

- **Opis**: Klient łączy się z proxy, które przekazuje ruch do ukrytego serwera.
    
- **Zastosowanie**:
    
    - Ukrywanie topologii sieci wewnętrznej
        
    - Ochrona przed DDoS (np. Cloudflare)
        
    - WAF (Web Application Firewall) jak ModSecurity
        
    - Ataki/pentest (np. pivoting przez SSH reverse proxy)
        
- **Zalety systemowe**:
    
    - Redukcja powierzchni ataku
        
    - Centralne zarządzanie bezpieczeństwem aplikacji
        
- **Przykłady**:
    
    - Nginx jako reverse proxy
        
    - ModSecurity
        
    - Cloudflare (działa jako reverse proxy i WAF)
        

📌 **Systemowa analogia**: Reverse proxy to _brama wejściowa_ systemu – przepuszcza tylko ruch spełniający określone reguły.
![[Pasted image 20250807140021.png]]
---

#### 🟨 **Transparent vs. Non-Transparent Proxy**

| Typ                 | Klient wie? | Konfiguracja po stronie klienta | Przykłady                     |
| ------------------- | ----------- | ------------------------------- | ----------------------------- |
| **Transparent**     | ❌ Nie       | Brak                            | Intercept proxy w sieci LAN   |
| **Non-Transparent** | ✅ Tak       | Tak – ręczna konfiguracja       | BurpSuite z ustawionym hostem |
## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)