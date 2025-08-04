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

## 7. Application Layer

Interfejs miedzy sięcią i aplikacjami

`HTTP (Hypertext Transfer Protocol)` - dla przeglądania sieci
`FTP (File Transfer Protocol)` - przesył plików
`SMTP (Simple Mail Transfer Protocol)` - maile
`DNS (Domain Name System)` - przypisywanie domen do adresów IP


## Jak to działa

Przesyłamy plik: 
- Application Layer inicjuje ządanie przesyłu plików. 
- Presentation Layer je szyfruje. 
- Session Layer stabilizuje sesje komunikacji między dwoma urządzeniami. 
- Transport Layer rozbija pliki na segmenty żeby zapewnić transmisje bez błędów
- Network Layer Sprawdza najlepszą trasę dla plików.
- Data Link Layer zbiera dane w ramki przygotowując je do node-to-node delivery
- Physical Layer przesyła bity poprzez fizyczne medium

---

## TCP/IP Model

`Transmission Control Protocol/Internet Protocol (TCP/IP) model` wersja tego wyżej ale praktyczna.

![[Pasted image 20250804141915.png]]

## 1. Link Layer

Fizyczne aspekty przesyłania danych i ramkowanie (1 i 2 z OSI).

## 2. Internet Layer

Adresowanie urządzeń i routing pakietów przez sieć. IP i [ICMP](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol) 3 warstwa w OSI.

## 3. Transport Layer
Na warstwie transportowej model TCP/IP zapewnia usługi komunikacji typu end-to-end, które są niezbędne dla funkcjonowania internetu. Obejmuje to wykorzystanie protokołu TCP (Transmission Control Protocol) do niezawodnej komunikacji oraz protokołu UDP (User Datagram Protocol) do szybszych, bezpołączeniowych usług. Warstwa ta zapewnia, że pakiety danych są dostarczane w kolejności i bez błędów, co odpowiada warstwie transportowej modelu OSI.

## 4. Application Layer

Protokoły odpowiadajace za wymiane danych miedzy aplikacjami.
Takie jak 5,6,7 warstwa OSI


![[Pasted image 20250804142158.png]]

### Przykład dostępu do strony internetowej

Podczas uzyskiwania dostępu do strony internetowej współpracują ze sobą różne warstwy modelu TCP/IP. W warstwie aplikacji przeglądarka korzysta z protokołu HTTP, aby zażądać strony. Następnie żądanie trafia do warstwy transportowej, gdzie TCP zapewnia niezawodny przesył danych. Warstwa internetowa (Internet Layer) zajmuje się trasowaniem pakietów danych dzięki protokołowi IP. Na końcu, w warstwie interfejsu sieciowego (Network Interface Layer), dane są fizycznie przesyłane przez sieć, co umożliwia wyświetlenie strony.

### Rola modeli

Model TCP/IP jest praktycznym fundamentem transmisji danych w sieciach i jest aktywnie stosowany w różnych środowiskach sieciowych. Z kolei model OSI, choć nie jest bezpośrednio wdrażany, pełni istotną funkcję jako teoretyczne narzędzie do zrozumienia działania sieci. Umożliwia on usystematyzowanie wiedzy i ułatwia analizę procesów sieciowych. Oba modele – TCP/IP i OSI – uzupełniają się, tworząc spójną całość łączącą teorię z praktyką w dziedzinie sieci komputerowych.

| Protokół | Opis |
|----------|------|
| **HTTP (Hypertext Transfer Protocol)** | Głównie używany do przesyłania stron internetowych. Działa na **warstwie aplikacji**, umożliwiając komunikację między przeglądarkami a serwerami w celu dostarczania treści WWW. |
| **FTP (File Transfer Protocol)** | Umożliwia przesyłanie plików między systemami, również działa na **warstwie aplikacji**. Pozwala użytkownikom na przesyłanie (upload) i pobieranie (download) plików z/do serwerów. |
| **SMTP (Simple Mail Transfer Protocol)** | Odpowiada za przesyłanie wiadomości e-mail. Działa na **warstwie aplikacji**, zapewniając dostarczenie wiadomości między serwerami pocztowymi do właściwych odbiorców. |
| **TCP (Transmission Control Protocol)** | Zapewnia niezawodne przesyłanie danych dzięki mechanizmom sprawdzania błędów i odzyskiwania danych. Działa na **warstwie transportowej** i ustanawia połączenie między nadawcą a odbiorcą, gwarantując uporządkowane dostarczenie danych. |
| **UDP (User Datagram Protocol)** | Umożliwia szybkie, bezpołączeniowe przesyłanie danych, bez sprawdzania błędów. Idealny do zastosowań, gdzie liczy się szybkość bardziej niż niezawodność (np. transmisje strumieniowe). Działa na **warstwie transportowej**. |
| **IP (Internet Protocol)** | Kluczowy do trasowania pakietów między różnymi sieciami. Działa na **warstwie internetowej**, odpowiada za adresowanie i kierowanie pakietów od źródła do celu. |


---

## 📡 Transmisja danych w sieciach komputerowych

**Transmisja** to proces przesyłania sygnałów (danych) między urządzeniami za pomocą określonego medium. Wyróżniamy kilka kluczowych aspektów transmisji:

### 🔄 Rodzaje transmisji

- **Analogowa** – wykorzystuje ciągłe sygnały (np. radio, telewizja analogowa).
    
- **Cyfrowa** – oparta na sygnałach dyskretnych (bity), wykorzystywana w nowoczesnych sieciach komputerowych.
    

### 🔁 Tryby transmisji

- **Simplex** – komunikacja tylko w jednym kierunku (np. klawiatura → komputer).
    
- **Half-duplex** – komunikacja w obie strony, ale naprzemiennie (np. krótkofalówki).
    
- **Full-duplex** – jednoczesna komunikacja dwustronna (np. rozmowy telefoniczne).
    

### 🌐 Media transmisyjne

- **Przewodowe:**
    
    - _Skrętka_ – typowy w sieciach Ethernet.
        
    - _Kabel koncentryczny_ – używany m.in. w telewizji kablowej.
        
    - _Światłowód_ – bardzo szybka transmisja na duże odległości.
        
- **Bezprzewodowe:**
    
    - _Fale radiowe_ – np. Wi-Fi, sieci komórkowe.
        
    - _Mikrofale_ – transmisja satelitarna.
        
    - _Podczerwień_ – krótkie dystanse, np. piloty.
        

---



## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)