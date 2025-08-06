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

# Study: {{title}}

---

# 🚀 Przebieg żądania HTTP w modelu klient-serwer

Poniżej krok po kroku, co się dzieje, gdy użytkownik na laptopie próbuje otworzyć stronę **[www.example.com](http://www.example.com)**.

---

## 1. Połączenie z siecią bezprzewodową (WLAN)

1. Laptop wyszukuje dostępne SSID i wybiera właściwą sieć.
    
2. Jeśli sieć chroniona jest WPA2/WPA3, użytkownik podaje hasło/poświadczenia.
    
3. Po uwierzytelnieniu nawiązywane jest połączenie, a DHCP przejmuje konfigurację IP.
    

---

## 2. Sprawdzenie konfiguracji lokalnej (DHCP)

| Krok             | Opis                                                                                                                  |
| ---------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Przydział IP** | Jeśli brak ważnego IP, laptop wysyła żądanie DHCP Discover do routera.                                                |
| **DHCP ACK**     | Router (serwer DHCP) odpowiada, przyznając prywatny adres (np. `192.168.1.10`), maskę podsieci, bramę domyślną i DNS. |
## 3. Rozwiązywanie nazwy domeny (DNS)

1. Laptop wysyła zapytanie DNS do skonfigurowanego serwera (ISP lub Google DNS).
    
2. DNS zwraca adres IP `www.example.com` (np. **93.184.216.34**).
    

---

## 4. Enkapsulacja i transmisja w LAN

1. **Warstwa aplikacji**  
    – Przeglądarka tworzy żądanie HTTP/HTTPS.
    
2. **Warstwa transportowa**  
    – Żądanie opakowane w segment TCP (port 80 lub 443).
    
3. **Warstwa internetowa**  
    – Segment TCP spakowany w pakiet IP:
		Źródłowy IP: 192.168.1.10  
		Docelowy IP: 93.184.216.34
4. **Warstwa łącza**  
	– Pakiet IP opakowany w ramkę Ethernet/Wi-Fi:
		Źródłowy MAC: karta sieciowa laptopa  
		Docelowy MAC: interfejs LAN routera  
5. **Laptop sprawdza tablicę ARP lub wysyła ARP Request, by poznać MAC bramy, po czym wysyła ramkę do routera.**


---
## 5. Network Address Translation (NAT)

1. Router odbiera ramkę, usuwa nagłówki warstwy łącza.
    
2. W nagłówku IP zamienia prywatny adres (`192.168.1.10`) na swój publiczny (np. **203.0.113.45**) i rejestruje mapowanie portów.
    
3. Forwarduje pakiet do sieci ISP, skąd trafia przez wiele routerów po drodze do serwera **93.184.216.34**.
    

---

## 6. Serwer odbiera żądanie i odpowiada

1. Pakiet dociera do sieci docelowej, ewentualnie przechodzi przez firewall — sprawdzenie portu 80/443.
    
2. Oprogramowanie serwera WWW (Apache/Nginx/IIS) przetwarza żądanie.
    
3. Serwer generuje odpowiedź (HTML, CSS, JS, zasoby).
    
4. Odpowiedź trafia w drogę powrotną:
	1. Źródłowy IP: 93.184.216.34  
		Docelowy IP: 203.0.113.45

## 7. Dekapsulacja i wyświetlenie strony

1. Router przyjmuje pakiet, dzięki tabeli NAT mapuje publiczny IP → `192.168.1.10`, wysyła ramkę do laptopa.
    
2. Laptop odbiera ramkę, usuwa nagłówki Ethernet/Wi-Fi, IP i TCP.
    
3. Dane aplikacji trafiają do przeglądarki, która renderuje stronę.

![[Pasted image 20250806152212.png]]



## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)