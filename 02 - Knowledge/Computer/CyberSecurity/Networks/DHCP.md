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

## 🔄 DHCP (Dynamic Host Configuration Protocol)

### 🔹 Co to jest?

DHCP to **protokół zarządzania siecią**, automatyzujący konfigurację urządzeń w sieci IP.

- Automatycznie przydziela:
    
    - **Adres IP**
        
    - **Maskę podsieci**
        
    - **Brama domyślna** (default gateway)
        
    - **Serwery DNS**
        
- Redukuje przeciążenie administracyjne i zapobiega konfliktom adresów.

## ⚙️ Role w DHCP

| Rola            | Opis                                                                                            |
| --------------- | ----------------------------------------------------------------------------------------------- |
| **DHCP Server** | Urządzenie (router lub dedykowany serwer) zarządzające pulą adresów IP i parametrów sieciowych. |
| **DHCP Client** | Każde urządzenie w sieci zgłaszające się po konfigurację (laptop, smartphone itp.).             |

## 🗺️ Proces DORA

1. **Discover**  
    Klient wysyła broadcast DHCP Discover, aby odnaleźć serwery DHCP.
    
2. **Offer**  
    Serwery DHCP odpowiadają DHCP Offer, proponując adres IP i pozostałe parametry.
    
3. **Request**  
    Klient akceptuje ofertę, wysyłając DHCP Request z żądaniem przydziału.
    
4. **Acknowledge**  
    Serwer potwierdza przydział przez DHCP Acknowledge. Klient może korzystać z adresu.
    

![[DORA-3.gif]]
---

## ⏲️ Lease i odnowienie

- Każdy przydział ma **czas dzierżawy** (lease time), np. 24 h.
    
- **Odnowienie**: przed wygaśnięciem klient wysyła DHCP Request, aby przedłużyć lease.
    
- Jeśli serwer potwierdzi (DHCP Acknowledge), urządzenie dalej korzysta z tego samego adresu IP.
    

---

## 👩‍💻 Przykład scenariusza

1. **Alice** podłącza laptop do sieci – nie ma IP.
    
2. Laptop wysyła **Discover** → DHCP Server.
    
3. Serwer odpowiada **Offer** z adresem `192.168.1.10`.
    
4. Laptop wysyła **Request** akceptując `192.168.1.10`.
    
5. Serwer potwierdza **Acknowledge**.
    
6. Laptop używa `192.168.1.10` przez czas lease (np. 24 h).
    
7. Przed wygaśnięciem lease laptop wysyła **Request** odnowieniowy → serwer odnowi lease przez **Acknowledge**.
    
---

![[Pasted image 20250804152750.png]]

## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)