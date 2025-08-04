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

# [[Network Main]]

## 🔄 NAT (Network Address Translation)

### 🔹 Co to jest?

NAT to mechanizm realizowany przez router lub podobne urządzenie, który **modyfikuje nagłówki IP** w pakietach, tłumacząc **prywatne adresy IP** urządzeń wewnątrz sieci na **jeden (lub kilka) publiczny adres IP** przypisany interfejsowi WAN.

---

## 🌐 Publiczne vs. Prywatne IP

- **Publiczny IP**
    
    - Globalnie unikalny, przydzielany przez ISP.
        
    - Dostępny i routowalny z każdego miejsca w Internecie (np. `8.8.8.8`, `142.251.46.174`).
        
- **Prywatny IP**
    
    - Używany wewnątrz LAN (RFC 1918).
        
    - **Nie jest routowalny** w Internecie (zakresy:  
        `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`).
        
    - Chroni sieć wewnętrzną przed bezpośrednim dostępem z zewnątrz.
        

---

## ⚙️ Jak działa NAT – przykład domowej sieci

1. **Sieć LAN**
    
    - Laptop: `192.168.1.10`
        
    - Smartphone: `192.168.1.11`
        
    - Console: `192.168.1.12`
        
    - Router LAN: `192.168.1.1`
        
2. **Interfejs WAN**
    
    - Router WAN (publiczny): `203.0.113.50`
        
3. **Tłumaczenie pakietu**
    
    - Laptop wysyła pakiet do `www.google.com`.
        
    - Router zmienia **źródłowy** IP `192.168.1.10` → `203.0.113.50`, przypisując unikalny port (np. `:5555`).
        
    - Pakiet trafia do serwera, który odpowiada na `203.0.113.50:5555`.
        
    - Router według tablicy NAT mapuje `203.0.113.50:5555` → `192.168.1.10:5555` i przekazuje dalej.


![[Pasted image 20250804153539.png]]

---
## 🏷️ Typy NAT

| Typ                    | Opis                                                                                                        |
| ---------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Static NAT**         | Jeden-do-jednego: stałe mapowanie prywatnego ↔ publicznego adresu.                                          |
| **Dynamic NAT**        | Z puli publicznych IP: serwer przydziela adres według potrzeb, z ograniczonego zakresu.                     |
| **PAT (NAT Overload)** | Najpopularniejszy w sieciach domowych – wiele prywatnych IP dzieli jedną publiczną, rozróżniane są portami. |

---

## ✅ Zalety i ⚠️ Wady

**Zalety:**

- Oszczędza zasoby IPv4 (wiele urządzeń → jedno IP publiczne).
    
- Podstawowa ochrona sieci wewnętrznej przed bezpośrednim atakiem.
    
- Elastyczność w planowaniu adresacji wewnętrznej.
    

**Wady / ograniczenia:**

- Utrudnia hostowanie usług publicznych za NAT (wymaga przekierowań portów).
    
- Może łamać protokoły end-to-end (np. VoIP, P2P).
    
- Dodaje warstwę złożoności przy diagnozowaniu problemów sieciowych.





## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)