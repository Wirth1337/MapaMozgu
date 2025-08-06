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

## Objective

### 📍 **1. Polecenie `ifconfig -a` – przegląd interfejsów sieciowych**

Wyświetlono 3 interfejsy:

- **ens3** – publiczny interfejs sieciowy (adres: `209.50.61.235`), przez który łączysz się z internetem.
    
- **lo** – **loopback** (`127.0.0.1`), służy do komunikacji **z samym sobą**.
    
- **tun0** – interfejs tunelowy, typowy dla VPN-ów (adres `10.10.14.21`).
    

🔎 Ciekawostki:

- `lo` ma bardzo duże MTU: **65536**, bo nie musi iść przez fizyczną sieć.
    
- Adres IPv6 loopbacka to `::1`.
    
- Adres `127.0.0.1` jest **lokalny tylko dla danego komputera** – usługi na nim nie są widoczne z zewnątrz.
    

---

### 🧪 **2. Polecenie `netstat -tulnp4` – otwarte porty IPv4**

Pokazuje, które usługi nasłuchują na jakich adresach IP i portach.

🔐 Przykłady z Pwnbox:

- `127.0.0.1:5901` → **VNC** (zdalny pulpit) działa tylko lokalnie (via loopback).
    
- `127.0.0.1:631` → lokalny serwer drukowania (ipp).
    
- `0.0.0.0:22` → **SSH** nasłuchuje na wszystkich interfejsach.
    
- `209.50.61.235:80` → **HTTP**, dzięki temu można otworzyć Pwnbox przez przeglądarkę.
    

🧠 Co oznacza `0.0.0.0`?  
Usługa nasłuchuje na **wszystkich dostępnych interfejsach** – lokalnych i publicznych.

---

### 🌐 **3. Polecenie `netstat -tulp4` – wyjście z nazwami usług**

Usługa `netstat` pokazuje teraz adresy w formacie `nazwa_hosta:usługa`, np.:

- `localhost:ipp` zamiast `127.0.0.1:631`
    
- `htb-5mix2gkv1a:http` zamiast `209.50.61.235:80`
    

👁 Dzięki temu widzimy:

- **`localhost`** oznacza `127.0.0.1`
    
- **`http`** = port 80, **`ssh`** = port 22 itd.
    

---

### 💡 **Wnioski i zastosowania loopbacka**

- **Loopback** pozwala serwerowi komunikować się z własnymi usługami **bez wystawiania ich publicznie**.
    
- **Bezpieczeństwo**: np. baza danych dostępna tylko lokalnie, ale aplikacja webowa (dostępna z zewnątrz) pobiera z niej dane jako pośrednik.
    
- **Testowanie**: 127.0.0.1 często używane do testów lokalnych serwerów, np. `localhost:3000` dla aplikacji webowej w fazie rozwoju.
    

---

### 🔁 **Port Forwarding (Przekierowanie portów)**

📦 Przykład:

- Windows host przekierowuje port `8888` na port `22` Linuksa (SSH).
    
- Nawet jeśli Linux nie ma publicznego IP, host pośredniczy w ruchu.
    

➡️ **To umożliwia dostęp do usług z prywatnych/ukrytych systemów.**

|Pojęcie|Znaczenie|
|---|---|
|`ifconfig -a`|Wyświetla wszystkie interfejsy (aktywny i nieaktywne)|
|`netstat -tulnp4`|Pokazuje otwarte porty IPv4 z numerami portów i PID|
|Loopback `lo`|Interfejs lokalny, IP: `127.0.0.1`, tylko lokalne połączenia|
|Port 80|HTTP – dostęp do serwera przez przeglądarkę|
|Port forwarding|Przekierowanie ruchu z jednego portu/IP na inny – nawet między systemami|

---

### 🔍 Sprawdzenie trasy pakietów:
`ip route get <target IP>`

### 📡 **Test połączenia z maszyną docelową**
🛠 Narzędzie: `ping`
`ping -c 4 <target IP>`

- **icmp_seq**: kolejność pakietów ICMP
    
- **ttl** (Time To Live): ile “skoków” pozostało – wyższe liczby zwykle oznaczają, że host jest blisko (np. ta sama sieć VPN)
    
- **time**: czas odpowiedzi (latencja), np. ~71 ms
    

✅ Wynik: 4/4 pakiety otrzymane, 0% strat – maszyna **osiągalna**.

---

### 🔍 **Skanowanie portów – `nmap`**

🛠 Narzędzie: `nmap` – służy do skanowania **otwartych portów TCP/UDP**
`nmap <target IP>`

|Port|Protokół|Usługa|Opis|
|---|---|---|---|
|21|TCP|FTP|Protokół przesyłania plików|
|80|TCP|HTTP|Serwer WWW|
|135|TCP|MSRPC|Zdalne wywoływanie procedur w Windows|
|139|TCP|NetBIOS-SSN|Sieciowe usługi SMB|
|445|TCP|Microsoft-DS|SMB over TCP (file sharing)|
|3389|TCP|RDP|Zdalny pulpit (Remote Desktop)|
|5357|TCP|WSDAPI|Protokół wykrywania urządzeń Windows|
🧠 Uwaga:

- Porty 135, 139, 445, 3389, 5357 – charakterystyczne dla **systemów Windows**
    
- Port 80 – umożliwia dostęp przez przeglądarkę (http)

## **Notatka – Analiza portów 21 (FTP) i 80 (HTTP) w środowisku testowym**

### 🔍 **1. Skanowanie portów Nmap**

W pierwszym kroku wykonano skanowanie portów 21 i 80 za pomocą komendy:
`nmap -p21,80 -sC -sV <adres IP>`
- `-p21,80` – skanuj tylko porty 21 i 80
    
- `-sC` – uruchamia domyślne skrypty NSE
    
- `-sV` – identyfikacja wersji usług
📌 Wyniki:
- Port 21: **FTP Microsoft ftpd** – dostępny login anonimowy
    
- Port 80: **HTTPAPI Microsoft** – niewiele informacji, możliwe filtrowanie żądań

---
### 📂 **2. Łączenie się z serwerem FTP (port 21)**

Użyto **netcata (nc)** do połączenia z serwerem FTP:
`nc <IP> 21`
Logowanie jako użytkownik anonimowy:
```
USER anonymous
PASS anything
PASV
```
Serwer odpowiada, aktywując **tryb pasywny** (Passive Mode), który wskazuje nowy port do transmisji danych. Port ten obliczamy według wzoru:
`PORT = p1 * 256 + p2`
Przykład:
```
227 Entering Passive Mode (10,129,233,197,194,40)
→ PORT = 194 * 256 + 40 = 49704
```

---

### 📁 **3. Przeglądanie i pobieranie plików z FTP**

#### ➤ Lista plików:

W nowym terminalu łączymy się z dynamicznym portem:
`nc -v <IP> 49704`
W pierwszym terminalu wpisujemy komendę:
`LIST`
✅ Otrzymujemy nazwę pliku: `Note-From-IT.txt`
#### ➤ Pobieranie pliku:

Ponownie aktywujemy PASV i obliczamy nowy port:
```
PASV → 227 Entering Passive Mode (...,194,50)
→ PORT = 194 * 256 + 50 = 49714
```
Łączymy się z tym portem i wpisujemy:
`RETR Note-From-IT.txt`

📨 Plik zostaje pobrany i zawiera istotną informację:

> Serwer WWW (HTTP) filtruje zapytania na podstawie nagłówka `User-Agent`. Aby uzyskać dostęp do strony, trzeba użyć `User-Agent: Server Administrator`.

---

### 🌐 **4. Połączenie z HTTP (port 80) i obejście filtru**

#### Połączenie:
`nc -v <IP> 80`
Wysyłamy zapytanie HTTP ręcznie:
```
GET / HTTP/1.1
Host: <IP>
User-Agent: Server Administrator
```
✅ Serwer odpowiada kodem **200 OK** i zwraca stronę HTML – w komentarzu ukryta jest **flaga HTB{...}**.

### 📚 **Podsumowanie**

- **FTP** wykorzystuje dwa kanały:
    
    - Port 21: kanał kontrolny (logowanie, polecenia)
        
    - Port dynamiczny: kanał danych (LIST, RETR)
        
- **HTTP** wymaga poprawnie sformułowanych nagłówków, np. `User-Agent`, aby umożliwić dostęp do treści
    
- **Netcat** to przydatne narzędzie do surowej komunikacji z usługami TCP/UDP, pozwalające lepiej zrozumieć działanie protokołów

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)