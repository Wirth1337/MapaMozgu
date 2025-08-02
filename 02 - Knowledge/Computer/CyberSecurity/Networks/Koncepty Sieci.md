---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - theme/default
  - status/in-progress
  - review/pending
date: 2025-08-02
last_updated: 2025-08-02
---

# [[Network Main]]

## OSI 

[OSI](https://www.youtube.com/watch?v=0y6FtKsg6J4)

**Open Systems Interconnection (OSI)** to 7 stopniowy framework który standaryzuje funkcje telekomunikacji 

## **All People Say They Never Download Porn**

- **All** → Application (7. Aplikacji)  
- **People** → Presentation (6. Prezentacji)  
- **Say** → Session (5. Sesji)  
- **They** → Transport (4. Transportu)  
- **Never** → Network (3. Sieci)  
- **Download** → Data Link (2. Łącza danych)  
- **Porn** → Physical (1. Fizyczna) 
![[Pasted image 20250802212940.png]]

# Model OSI jako szlak handlowy

Wyobraź sobie, że transport towarów z jednego miasta do drugiego to warstwy modelu OSI. Każda z 7 warstw odpowiada innej fazie handlu.

| Warstwa OSI (nr) | Etap w szlaku handlowym                             | Opis analogii                                                                                 |
|------------------|------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| **7. Aplikacji** | Spotkanie kupca z klientem                          | Kupiec prezentuje gotowy towar i przyjmuje zamówienie. Tu użytkownik (klient) wchodzi w interakcję z usługą. |
| **6. Prezentacji** | Pakowanie i etykietowanie towaru                  | Towar jest odpowiednio zapakowany, opisany i zaszyfrowany (np. folią zabezpieczającą).         |
| **5. Sesji**      | Ustalenie warunków handlowych                       | Kupiec i klient negocjują ceny, terminy, metody płatności i czas dostawy.                     |
| **4. Transportu** | Organizacja transportu                              | Wybór odpowiedniego samochodu/kontenera, zapewnienie ubezpieczenia i monitoringu przesyłki.   |
| **3. Sieci**      | Wytyczenie trasy                                    | Planowanie i optymalizacja drogi — który most, autostrada czy skrót wybrać, aby dostarczyć na czas. |
| **2. Łącza danych** | Przekazywanie przesyłki między przewoźnikami       | Załadunek i rozładunek na kolejne etapy – od lokalnego kuriera do spedytora dalekiego zasięgu. |
| **1. Fizyczna**   | Drogi, tory i samoloty                              | Realna infrastruktura: asfalt, tory, ścieżki transportowe oraz pojazdy, kontenery i skrzynie. |

**Jak to działa w praktyce?**  
1. **Aplikacja** – Kupiec (aplikacja) oferuje produkt lub usługę.  
2. **Prezentacja** – Towar jest przygotowany, opakowany, często zaszyfrowany lub skompresowany, gotowy do wysyłki.  
3. **Sesja** – Sprzedawca i kupujący ustanawiają sesję handlową: negocjacje, warunki transakcji, sesja może być wielokrotna (ciągłe zamówienia).  
4. **Transport** – Od zapewnienia pojazdu do pilotażu i monitoringu przesyłki – odpowiedzialność za to, żeby paczka dotarła w całości i w kolejności.  
5. **Sieć** – Decyzje, którymi drogami pojechać: wybór autostrad, dróg lokalnych, przejść granicznych – czyli routing pakietów.  
6. **Łącza danych** – Przekazanie towaru od jednej firmy przewozowej do drugiej, przeładunek na terminalach – czyli enkapsulacja i dekapsulacja ramek.  
7. **Fizyczna** – Stan dróg, taboru, kontenerów – infrastruktura przesyłu energii i materiałów (kable, fale radiowe, światłowody).


![[Pasted image 20250802215703.png]]

---

## 1.Warstwa Fizyczna

Czyste strumienie danych przez fizyczne medium jak kable huby itp.

## 2.Warstwa Data Linka 

Transfer Node <-> Node, zapewnia synchronizacje, wykrywanie błędów w transmisji danych.
- switche 
- bridge
Używają [MAC](https://en.wikipedia.org/wiki/MAC_address) adresów do identyfikacji urządzeń sieciowych.
## 3. Warstwa Network

Wysyłanie pakietów różnymi trasami aż osiągną docelową sieć. Routery używają do tego 
[IP](https://www.kaspersky.com/resource-center/definitions/what-is-an-ip-address) żeby identyfikować urządzenia i określać najefektywniejsza ścieżkę.
## 4. Warstwa Transport

End to end komunikacja dla aplikacji. Odpowiedzialna za 
- dostarczanie danych 
- segmentacje
- kontrole przepływu
- sprawdzanie błędów

Protokoły TCP `(Transmission Control Protocol)` i UDP `(User Datagram Protocol)`
działają na tej warstwie. 

- TCP solidne oparte na połączeniu transmisje z error Recovery
- UDP szybkie connectionless komunikacja bez gwarantowanego dostarczenia.

## 5. Session Layer

Zarządza sesjami między aplikacjami. Ustala komunikację znane jako sesje. Protokoły i APIs operują na tej warstwie kordynując komunikację między systemami i aplikacjami.

## 6. Presentation Layer

Tłumacz między aplikacjami a formatem sieciowym. Reprezentacja danych, czyli dane wysyłane przez jeden system są do odczytu przez drugi. 
- szyfrowanie
- deszyfrowanie
- kompresja
- konwersja formatów
Protokoły szyfrowania i Techniki Kompresji zabezpieczają i optymalizują transmisję danych.

## 7.

## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)