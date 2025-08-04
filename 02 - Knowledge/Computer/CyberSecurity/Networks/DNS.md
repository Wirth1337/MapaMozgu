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

---

## 📖 DNS (Domain Name System)

### 🔹 Co to jest?

DNS to **książka telefoniczna internetu** – tłumaczy czytelne nazwy domen na **numeryczne IP**, dzięki czemu nie musimy pamiętać złożonych adresów.

---

## 🌐 Domain Names vs. IP Addresses

|Typ|Opis|
|---|---|
|**Domain Name**|Czytelny adres, np. `www.example.com`|
|**IP Address**|Numeryczna etykieta, np. `93.184.216.34`|
DNS łączy te dwa światy — wpisujesz `www.google.com`, a otrzymujesz odpowiednie IP.

---
## 🌲 Hierarchia DNS

1. **Root Servers**
    
2. **TLD** (Top-Level Domains): `.com`, `.org`, `.net`, `.uk` itp.
    
3. **Second-Level Domains**: np. `example` w `example.com`
    
4. **Subdomains/Hostname**: np. `www` w `www.example.com` lub `accounts` w `accounts.google.com`
    

![[Pasted image 20250804155924.png]]

---

## 🛠️ URL Breakdown

- **Scheme**: `https://`
    
- **Subdomain**: `www.`
    
- **Second-Level Domain**: `example`
    
- **TLD**: `.com`
    
- **Path/Page**: `/index.html`
    

---

## 🔍 DNS Resolution Process

1. **Wpisanie nazwy**  
    W przeglądarce wpisujesz `www.example.com`.
    
2. **Cache lokalny**  
    Sprawdzenie, czy adres jest już w lokalnej pamięci podręcznej.
    
3. **Recursive DNS Query**  
    Jeśli nie, zapytanie trafia do serwera rekursywnego (ISP lub Google DNS).
    
4. **Root Server**  
    Serwer rekursywny pyta Root Server → wskazanie serwera TLD `.com`.
    
5. **TLD Name Server**  
    TLD odpowiada, przekierowując do autorytatywnego serwera dla `example.com`.
    
6. **Authoritative Name Server**  
    Autorytatywny serwer zwraca IP dla `www.example.com`.
    
7. **Odpowiedź do klienta**  
    Serwer rekursywny przekazuje IP do Twojego komputera → nawiązanie połączenia.
    

> Cały proces trwa w ułamkach sekundy, zapewniając płynne przejście od nazwy do serwera.

![[Pasted image 20250804155931.png]]
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