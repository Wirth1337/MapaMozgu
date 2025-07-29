---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - theme/default
  - status/in-progress
  - review/pending
date: 2025-07-29
last_updated: 2025-07-29
---

# [[Linux Main|Study - {{Linux}}]] 

#networking 

# Konfiguracja sieci w systemie Linux

Konfiguracja sieci w systemach Linux to kluczowa umiejętność – zwłaszcza dla pentesterów – umożliwiająca przygotowanie środowisk testowych, manipulację ruchem i analizę potencjalnych luk w zabezpieczeniach. Ważne jest zrozumienie sposobu zarządzania interfejsami sieciowymi (np. przewodowymi i bezprzewodowymi) oraz protokołów sieciowych (TCP/IP, DNS, DHCP, FTP itp.). Znajomość tych mechanizmów pozwala na precyzyjne dostosowywanie konfiguracji sieciowej w celu zoptymalizowania testów i wyników.

## Konfigurowanie interfejsów sieciowych

W nowoczesnych dystrybucjach Linux (np. Ubuntu, Debian) do zarządzania interfejsami sieciowymi zamiast przestarzałych narzędzi `ifconfig` i `route` stosuje się pakiet iproute2. Polecenia te są szybsze i bardziej przejrzyste[askubuntu.com](https://askubuntu.com/questions/1025568/has-netstat-been-replaced-with-a-new-tool#:~:text=52)[redhat.com](https://www.redhat.com/en/blog/route-ip-route#:~:text=%5Broot%40rhel%20~%5D,1). Przykładowe komendy:

- Wyświetlanie interfejsów i adresów:
```

ip addr show
ip link show
```

- Aktywacja interfejsu (przykład dla eth0):
	`sudo ip link set eth0 up`

- Przydzielenie adresu IPv4 i maski sieci (np. 192.168.1.2/24):
	- `sudo ip addr add 192.168.1.2/24 dev eth0`

- Ustawienie bramy domyślnej dla interfejsu (np. 192.168.1.1):
	`sudo ip route add default via 192.168.1.1`

Zamiast `netstat`, do przeglądania połączeń sieciowych używa się polecenia `ss` (Socket Statistics), które jest szybsze i zastępuje `netstat`[askubuntu.com](https://askubuntu.com/questions/1025568/has-netstat-been-replaced-with-a-new-tool#:~:text=52). Na przykład `ss -tulnp` pokaże aktywne gniazda i nasłuchujące porty. Do listowania otwartych plików i portów można też użyć `lsof -i`.

W systemach korzystających z _NetworkManager_ lub _systemd-networkd_ konfigurację sieci często definiuje się w plikach YAML (netplan) lub przez GUI. Przykładowo w Ubuntu 22.04+ konfiguracja statycznego IP w pliku `/etc/netplan/*.yaml` może wyglądać tak[code.mendhak.com](https://code.mendhak.com/ubuntu-2404-set-static-ip-address-using-netplan/#:~:text=,Cloudflare%20and%20one%20Google%20DNS)[code.mendhak.com](https://code.mendhak.com/ubuntu-2404-set-static-ip-address-using-netplan/#:~:text=To%20then%20apply%20the%20changes%2C):

```
network:
  ethernets:
    eth0:
      dhcp4: false
      addresses: [192.168.1.2/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [1.1.1.1, 8.8.8.8]
  version: 2
```

Po zapisaniu zmian stosujemy konfigurację poleceniem:
	`sudo netplan apply`
co zapisuje ustawienia w systemd-networkd lub NetworkManager[code.mendhak.com](https://code.mendhak.com/ubuntu-2404-set-static-ip-address-using-netplan/#:~:text=To%20then%20apply%20the%20changes%2C).

**DNS:** Serwery DNS definiuje się zwykle w pliku `/etc/resolv.conf` lub w odpowiednich ustawieniach narzędzia zarządzającego siecią. W systemach z _systemd-resolved_ plik ten może być nadpisywany, dlatego lepiej skonfigurować DNS przez edycję pliku `resolv.conf` w `/etc/systemd/` lub przez interfejs _NetworkManager_. Dla przykładu, aby użyć publicznych DNS Google, wpisujemy w `/etc/resolv.conf` lub konfiguracji interfejsu:
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

Aby ustawić DNS trwale, wykorzystaj plik konfiguracyjny netplan lub właściwości połączenia w NetworkManagerze[code.mendhak.com](https://code.mendhak.com/ubuntu-2404-set-static-ip-address-using-netplan/#:~:text=,Cloudflare%20and%20one%20Google%20DNS).

**Trwałość ustawień:** Aby zmiany przetrwały restart, należy edytować pliki konfiguracyjne. Na starszych systemach był to `/etc/network/interfaces`, gdzie określano parametry (adres, maskę, gateway, DNS) dla interfejsów. W nowoczesnym Ubuntu/Debian wprowadzono _netplan_, dlatego domyślnie należy zapisywać statyczne ustawienia w plikach YAML w `/etc/netplan/`. Przykładowy wpis w `/etc/network/interfaces` (starsza metoda) wyglądałby tak:

```
auto eth0
iface eth0 inet static
   address 192.168.1.2
   netmask 255.255.255.0
   gateway 192.168.1.1
   dns-nameservers 8.8.8.8 8.8.4.4
```

Po wprowadzeniu zmian w konfiguracji należy zrestartować usługę sieciową (`sudo systemctl restart networking` lub `sudo systemctl restart NetworkManager`), aby zastosować ustawienia.

---

## Kontrola dostępu do sieci (NAC)

Model **kontroli dostępu do sieci (Network Access Control, NAC)** określa, które urządzenia lub użytkownicy mogą korzystać z zasobów sieciowych. Popularne modele NAC to:

- **Discretionary Access Control (DAC)** – kontrolę dostępu ustala właściciel zasobu, definiując kto i jakie operacje (odczyt, zapis, wykonanie) może wykonać. Jest to elastyczne podejście, ale łatwiej tu o błędy konfiguracyjne.
    
- **Mandatory Access Control (MAC)** – uprawnienia nadawane są przez system operacyjny według wcześniej zdefiniowanych etykiet bezpieczeństwa. Użytkownicy nie mogą samodzielnie zmieniać polityk, co czyni model bezpieczniejszym, lecz mniej elastycznym. Używa się go np. w środowiskach rządowych czy bankowych do ścisłej separacji danych.
    
- **Role-Based Access Control (RBAC)** – uprawnienia przypisuje się na podstawie ról w organizacji. Użytkownicy dziedziczą uprawnienia swojej roli, co upraszcza zarządzanie i zmniejsza liczbę błędów. RBAC ułatwia zapewnienie minimalnych uprawnień (least privilege) – użytkownik ma dostęp tylko do tych zasobów, które są mu niezbędne do pracy.
    

Praktyczne zabezpieczenia NAC w Linuxie obejmują m.in.:

- **SELinux (Security-Enhanced Linux)** – wdraża model MAC. Polityki SELinux określają, jakie procesy i użytkownicy mają dostęp do plików i zasobów systemowych. Pozwala to ściśle ograniczyć działania procesów (podobnie do „containment”). SELinux jest bardzo dokładny, ale jego konfiguracja jest skomplikowana[techtarget.com](https://www.techtarget.com/searchdatacenter/tip/Compare-two-Linux-security-modules-SELinux-vs-AppArmor#:~:text=SELinux%20is%20more%20difficult%20to,thereby%20leaving%20a%20system%20vulnerable).
    
- **AppArmor** – lżejszy system MAC bazujący na profilach aplikacji. Profil definiuje, do jakich plików i zasobów proces może uzyskać dostęp. Łatwiejszy w konfiguracji niż SELinux i domyślnie włączony np. w Ubuntu, choć mniej granularny[techtarget.com](https://www.techtarget.com/searchdatacenter/tip/Compare-two-Linux-security-modules-SELinux-vs-AppArmor#:~:text=SELinux%20is%20more%20difficult%20to,thereby%20leaving%20a%20system%20vulnerable).
    
- **TCP Wrappers** – prosta metoda ograniczania dostępu do usług sieciowych na poziomie adresów IP. Pliki `/etc/hosts.allow` i `/etc/hosts.deny` określają, które adresy mogą łączyć się do usług typu telnet, sshd, ftp itp. Jako prostsze rozwiązanie na poziomie hosta przydaje się np. do zablokowania nieautoryzowanych hostów.
    

Każdy z tych mechanizmów ma podobny cel – zabezpieczyć system przed nieautoryzowanym dostępem – jednak różnią się zakresem i sposobem działania. SELinux i AppArmor wprowadzają kontrolę MAC na poziomie aplikacji/procesów, podczas gdy TCP Wrappers skupia się tylko na dostępie do usług sieciowych. SELinux oferuje największe możliwości (i złożoność), AppArmor jest wygodniejszy dla administratora, a TCP Wrappers zapewnia podstawowe filtrowanie ruchu na wyjściu/po stronie serwera.

---

## Monitorowanie sieci

Monitorowanie ruchu sieciowego pozwala wychwycić anomalie i potencjalne ataki. Na Linuxie popularne są narzędzia takie jak:

- **Syslog / rsyslog / journald** – systemy logowania zdarzeń. Zbierają logi systemu i aplikacji (w tym komunikaty sieciowe) i mogą wysyłać je na serwer zdalny lub do systemów analizy.
    
- **ss** i **lsof** – pokazują bieżące gniazda (sockets) i pliki otwarte przez procesy, co pozwala monitorować aktywne połączenia.
    
- **Tcpdump** – narzędzie do przechwytywania pakietów sieciowych, umożliwia analizę ruchu na żywo (np. filtrowanie po porcie).
    
- **Wireshark (gui) / tshark (cli)** – służą do analizy przechwyconych pakietów (np. z pliku pcap). Popularne przy analizie protokołów i odszyfrowywaniu ruchu.
    
- **ELK Stack (Elasticsearch, Logstash, Kibana)** – platforma do zbierania i analizy logów. Przy dużej ilości danych sieciowych pozwala na indeksowanie, wyszukiwanie i wizualizację anomalnych wzorców ruchu.
    

Dzięki tym narzędziom można np. wykryć nietypowy ruch (port scanning, nietypowy ruch DNS itp.), odczytać przesyłane dane (przechwycenie połączeń FTP czy HTTP w postaci niezaszyfrowanej), a także monitorować obciążenie sieci i wykorzystanie zasobów. W praktyce pentester, analizując ruch, może odkryć przesyłanie haseł lub skanowanie wnętrza sieci.

---

## Rozwiązywanie problemów sieciowych

Typowe problemy sieciowe to: **brak łączności**, **problemy z DNS**, **utrata pakietów**, **spadek wydajności**. Najczęstsze przyczyny to niewłaściwe ustawienia (błędne IP, maska, gateway), problemy sprzętowe (uszkodzone kable, porty), błędy konfiguracji DNS czy przestarzałe urządzenia sieciowe.

Podstawowe narzędzia diagnostyczne:

- **ping** – sprawdza łączność zdalnego hosta (wysyłając pakiety ICMP Echo Request). Na przykład `ping 8.8.8.8` potwierdzi, czy jest osiągalny serwer DNS Google oraz zmierzy czas odpowiedzi.
    
- **traceroute** (w niektórych systemach `traceroute` lub `tracepath`) – pokazuje trasę pakietów do hosta z kolejnymi węzłami pośrednimi. Na przykład `traceroute example.com` wyświetli kolejne skoki (routery), przez które przechodzą pakiety.
    
- **ss / netstat** – pokazuje aktywne połączenia i porty. Na przykład `ss -tulnp` wylistuje wszystkie procesy nasłuchujące na portach TCP/UDP z numerami PID. Starsze `netstat -a` wciąż może działać, ale zaleca się `ss`[askubuntu.com](https://askubuntu.com/questions/1025568/has-netstat-been-replaced-with-a-new-tool#:~:text=52).
    
- **ip addr, ip route** – pokazują stan interfejsów (adresy IP) i tablicę routingu. Pomocne w weryfikacji, czy adresy i trasy są poprawne.
    
- **dig / nslookup** – narzędzia do testowania DNS. Pozwalają sprawdzić, czy zapytania DNS zwracają poprawne rekordy i jakim serwerem DNS są obsługiwane.
    
- **nmap** – skaner sieciowy, który może wykryć otwarte porty na zdalnym hoście i próbować identyfikować usługi.
    

Przykładowo, `ping` potwierdzi tylko, że host odpowiada, ale nie mówi, co jest przyczyną opóźnień. Jeśli ping nie działa, a `traceroute` pokazuje brak odpowiedzi już na pierwszym skoku, warto sprawdzić lokalne ustawienia IP i gatewaya. Jeśli problemem jest DNS (np. ping do domeny nie działa, ale po IP tak), można użyć `nslookup example.com` by zobaczyć, jakiego IP udaje się zdobyć z serwera DNS.

---

## Uszczelnianie systemu (hardening)

Dodatkowe mechanizmy zabezpieczające Linuxa to np. SELinux, AppArmor i TCP Wrappers (omówione wcześniej), ale także zapory sieciowe (iptables, nftables, firewalld/ufw) czy inne LSM (Linux Security Modules). SELinux i AppArmor już opisane służą kontroli dostępu do zasobów. Dodatkowo:

- **UFW / iptables / nftables** – pozwalają filtrować ruch sieciowy na poziomie jądra. Na przykład `ufw` (na Ubuntu) upraszcza tworzenie reguł (dopuszczanie/odrzucanie portów).
    
- **Konfiguracja usług** – warto wyłączać/odinstalowywać nieużywane usługi, usuwać lub ograniczać konta użytkowników. Każda niepotrzebna usługa to potencjalne wejście dla atakującego.
    
- **Regularne aktualizacje** – utrzymanie aktualnych łat bezpieczeństwa jądra i oprogramowania sieciowego.
    

Podsumowując, SELinux oferuje najwięcej kontroli i jest włączony w wielu dystrybucjach z rodziny RHEL, ale jest najtrudniejszy w konfiguracji[techtarget.com](https://www.techtarget.com/searchdatacenter/tip/Compare-two-Linux-security-modules-SELinux-vs-AppArmor#:~:text=SELinux%20is%20more%20difficult%20to,thereby%20leaving%20a%20system%20vulnerable). AppArmor domyślnie chroni Ubuntu i jest łatwiejszy w użyciu, choć mniej elastyczny[techtarget.com](https://www.techtarget.com/searchdatacenter/tip/Compare-two-Linux-security-modules-SELinux-vs-AppArmor#:~:text=SELinux%20is%20more%20difficult%20to,thereby%20leaving%20a%20system%20vulnerable). TCP Wrappers to prosty sposób ograniczenia dostępu do niektórych usług według adresu IP. Wszystkie te narzędzia razem stanowią „drugą linię obrony” – nawet jeśli atakujący przeniknie przez firewall, to silne polityki MAC ograniczą szkody.

---

## Zadania praktyczne

**Zalecane ćwiczenia (opcjonalne):** Ustaw w swoim środowisku zadania związane z SELinux, AppArmor i TCP Wrappers, np.:

- **SELinux:**
    
    1. Zainstaluj SELinux (np. `selinux-basics` + `policycoreutils` w Debian/Ubuntu lub odpowiednie pakiety w RHEL).
        
    2. Skonfiguruj politykę SELinux uniemożliwiającą zwykłemu użytkownikowi odczyt określonego pliku.
        
    3. Zezwól tylko jednemu użytkownikowi na korzystanie z danej usługi sieciowej, innym zabroń.
        
    4. Stwórz regułę SELinux blokującą dostęp określonemu użytkownikowi lub grupie do wybranej usługi sieciowej.
        
- **AppArmor:**
    
    1. Stwórz profil AppArmor, który zablokuje użytkownikowi dostęp do wskazanego pliku.
        
    2. Skonfiguruj profil tak, aby tylko jeden użytkownik mógł uruchamiać daną usługę sieciową, a pozostałym odmów dostęp.
        
    3. Utwórz regułę AppArmor uniemożliwiającą konkretnej grupie użytkowników dostęp do wskazanego serwisu sieciowego.
        
- **TCP Wrappers:**
    
    1. Skonfiguruj `/etc/hosts.allow` i `/etc/hosts.deny`, aby zezwolić konkretnemu adresowi IP na dostęp do wybranej usługi (np. SSH).
        
    2. Dodaj regułę odmawiającą dostępu do tej usługi z określonego adresu IP.
        
    3. Skonfiguruj dostęp do usługi tylko dla zakresu adresów IP (np. cała podsieć 192.168.1.0/24).
        

---
## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)