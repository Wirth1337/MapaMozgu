---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - status/in-progress
  - review/pending
  - theme/cybersecurity
date: 2025-07-29
last_updated: 2025-07-29
---

# [[Linux Main|Study - {{Linux}}]] 

## Ogólnie
#networking
Sieć to pajęczyna. Ogromna wielowymiarowa pajęczyna która, tak jak ta OG przesyła drgania a komputer jako pająk te drgania odczytuje.

Wyobraź sobie scenariusz w którym przeprowadzasz pentest i napotykasz na hosta który jest podłączony do innego servera przez niezaszyfrowany FTP serwer. Dzięki temu możemy zapisać jego credentials w pliku tekstowym. Sytuacja byłaby mniej prawdopodobna jeżeli użytkownik byłby świadomy że FTP nie szyfruje danych ani połączenia. 

## Treść
## SSH
Secure Shell (SSH) jest protokołem sieciwym który zapewania zabezpieczoną transmijse danych i komend przez sieć.

Najczęściej używanym serwerem SSH jest OpenSSH czyli open-sourcowa implementacja SSH

---

### Instalacja SSH
![[Pasted image 20250729104120.png]]
`sudo apt install openssh-server -y`
**Żeby sprawdzić czy serwer działa uzywamy następującej komendy.
![[Pasted image 20250729104204.png]]
`systemctl status ssh`

SSH - Logging In
![[Pasted image 20250729105652.png]]
`ssh login@ip`

**SSH można konfigurować za pomocą edycji pliku `/etc/ssh/sshd_config` Tutaj możemy:**
- Ustawić maksymalną liczbę połączeń
- Użycie haseł i kluczy dla loginów
- Sprawdzanie klucza hosta

Możemy użyć SSH by się bezpiecznie zalogować do zdalnego systemu i wykonać komendy lub uzyć tunelowania i przekierowywania portów żeby wysyłać dane przez zaszyfrowane połączenie żeby zweryfikować ustawienia sieci i innych ustawień bez możliwości przechwycenia danych przez osoby trzecie.

---
## NFS

**Network File System (NFS)** jest protokołem sieciowym który umożliwia przechowywanie i zarządzanie plikami na zdalnym systemie tak jakby były przechowywane na lokalnym systemie.

Przykładowo administrator używa NFS żeby przechowywać i zarządzać plikami centalnie i umożliwić łatwą kolaboracje i zarządzanie danymi

Dla Linuxa:

- NFS-UTILS (Ubuntu)
- NFS-Ganesha (Solaris)
- OpenNFS (Redhat Linux)

Można też z niego skorzystać żeby replikować systemy plików między serwerami. Ale również do:
- Uzyskania kontroli
- Transferu plików w czasie rzeczywistym
- Wsparcia dla wielu użytkowników którzy mają dostęp do danych w jednym momencie

**Można użyć tej usługi tak jak FTP gdy nie ma zainstalowanego klienta FTP na targetowanym systemie albo NFS działa zamiast FTP**

### Instalacja NFS
![[Pasted image 20250729111107.png]]
`sudo apt install nfs-kernel-server -y`

**Żeby sprawdzić czy serwer działa używamy komendy**
![[Pasted image 20250729111157.png]]
`systemctl status nfs-kernel-server`

Możemy konfigurować NFS za pomocą pliku `/etc/exports`. 

Co można w nim zrobić?:
- szybkość transferu
- użycie szyfrowania

Poziom dostępu ustala który użytkownik oraz system mogą używać wspólnych folderów i jakie akcje mogą wykonywać 

---

## 📂 NFS Export Options

| Opcja                    | Opis                                                                                                                    |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| `rw`                     | 🟢 Umożliwia **odczyt i zapis** zdalnym klientom. Pozwala tworzyć, edytować i usuwać pliki w udziałach NFS.             |
| `ro`                     | 🔒 Umożliwia **tylko odczyt** danych z udziału NFS – klient nie może nic zmienić ani utworzyć.                          |
| `no_root_squash`         | 🚨 **Nie ogranicza** uprawnień roota na kliencie – root klienta działa jako root na serwerze (może być niebezpieczne!). |
| `root_squash`            | 🧱 Domyślnie włączone: root klienta działa jako **anonimowy użytkownik (nobody)** – zwiększa bezpieczeństwo.            |
| `sync`                   | 🛡️ Dane są zapisywane **natychmiast na dysku serwera** przed potwierdzeniem. Bezpieczniejsze, ale wolniejsze.          |
| `async`                  | ⚡ Dane mogą być **buforowane i zapisywane później** – szybsze, ale ryzyko utraty danych przy awarii.                    |
| `all_squash`             | 👥 **Wszyscy użytkownicy** z klienta są mapowani do anonimowego użytkownika (`nobody`). Dobre do publicznych udziałów.  |
| `anonuid`                | 🆔 Ustawia **UID użytkownika anonimowego**, na którego mapowani są użytkownicy przy użyciu `*_squash`.                  |
| `anongid`                | 🆔 Ustawia **GID użytkownika anonimowego**, podobnie jak `anonuid`, ale dla grupy.                                      |
| `no_subtree_check`       | 📁 Wyłącza sprawdzanie, czy plik należy do eksportowanego katalogu. Zwiększa wydajność, zmniejsza bezpieczeństwo.       |
| `subtree_check`          | 🔍 Domyślnie włączone: sprawdza, czy dostępny plik naprawdę należy do udostępnionego katalogu.                          |
| `insecure`               | 🔓 Pozwala klientom łączyć się z portów wyższych niż 1024 – mniej bezpieczne.                                           |
| `secure`                 | 🔐 Domyślnie: tylko połączenia z portów < 1024 (uprzywilejowanych) – bardziej zaufane.                                  |
| `no_wdelay`              | 🕒 Wyłącza opóźnienia przy zapisie – użyteczne, gdy tylko jeden klient korzysta z udziału.                              |
| `wdelay`                 | 🕰️ Domyślnie włączone: wprowadza minimalne opóźnienie przy zapisie, aby zwiększyć wydajność grupowych zapisów.         |
| `fsid=0` lub `fsid=root` | 🧭 Używane do zdefiniowania **root katalogu** eksportu dla NFSv4 – wymagane dla głównego udziału.                       |
| `crossmnt`               | 🔗 Pozwala klientom przechodzić do **zamontowanych podkatalogów** w udostępnionym katalogu.                             |

Plik `/etc/exports` może wyglądać tak:
`/srv/nfs/data 192.168.1.0/24(rw,sync,no_subtree_check)
`
Oznacza to, że cały subnet ma dostęp do `/srv/nfs/data` z prawem zapisu, z synchronizacją danych i bez sprawdzania poddrzewa katalogów.

### Create NFS Share
![[Pasted image 20250729122019.png]]

Jeżeli utworzyliśmy NFS share i chcemy żeby działał z docelowym systemem musimy z zmountować.

Robimy to następującą komendą
![[Pasted image 20250729122134.png]]
```
mkdir ~/target_nfs 
mount 10.129.12.17:/home/john/dev_scripts ~/target_nfs
tree ~/target_nfs
```

Powyższe polecenia:

1. Tworzą lokalny punkt montowania `~/target_nfs`,
    
2. Montują zdalny udział NFS (`/home/john/dev_scripts`) z hosta `10.129.12.17` do tego punktu,
    
3. Pozwalają na eksplorację jego zawartości lokalnie przy pomocy narzędzia `tree`.
    

W efekcie zyskujemy dostęp do zdalnego katalogu `dev_scripts` tak, jakby był częścią naszego lokalnego systemu plików — możemy go przeglądać, edytować i wykonywać pliki, jeśli pozwalają na to uprawnienia.

---

## Web Server 
#apache
Web Server jest oprogramowaniem która dostarcza dane, dokumenty, aplikacje i różne funkcje przez internet.

Używa on **Hypertext Transfer Protocol (HTTP)** żeby transmitować dane do klientów takich jak przeglądarki internetowe i odbierać requesty.

Następnie owe dane są renderowane jako **Hypertext Markup Language (HTML)** w środku przeglądarki klienta umożliwiając tworzenie dynamicznych stron internetowych które odpowiadają według requestów użytkownika.

Najczęściej używanymi webserverami na Linuxie są:
- Apache
- Nginx
- Lighttpd
- Caddy

Najbardziej powszechnym jest Apache z racji szerokiej kompatybilności z OS takimi jak Ubuntu i cała reszta Linuxowej śmietanki towarzyskiej (Arch BTW)

Dla pentesterów web sever oferuje przeróżne możliwości.
- faciliate file transfer 
- pozwala testerom logować się i działać na targetowanym systemie przez porty HTTP i HTTPS 

Dodatkowo Serwery WWW mogą być wykorzystywane do ataków phishingowych poprzez hostowanie fałszywych stron logowania w celu przechwytywania danych uwierzytelniających. Umożliwiają również testowanie i wykrywanie podatności w sieci.

---
## Instal Apache WebServer

**Apache** to popularny serwer WWW oferujący funkcje bezpiecznego hostowania aplikacji oraz logowania ruchu w celu analizy ataków.  
Aby go zainstalować, użyj polecenia:
![[Pasted image 20250729130237.png]]
`sudo apt install apache2 -y`

Dla Apache2 żeby określić które foldery mogą być dostępne, możemy edytować plik `/etc/apache2/apache2.conf`. Ten plik zawiera ustawienia globalne. 

---

## Apache Configuration
![[Pasted image 20250729130431.png]]
Ten fragment konfiguracji określa, że domyślny folder `/var/www/html` jest dostępny, użytkownicy mogą korzystać z opcji `Indexes` i `FollowSymLinks`, zmiany w plikach w tym katalogu mogą być nadpisywane dzięki `AllowOverride All`, a `Require all granted` przyznaje dostęp wszystkim użytkownikom.  
Przykładowo, jeśli chcemy przesłać pliki na system docelowy, możemy umieścić je w katalogu `/var/www/html` i pobrać na target za pomocą `wget`, `curl` lub innego narzędzia.

Możemy również dostosować ustawienia dla konkretnego katalogu za pomocą pliku `.htaccess`, który tworzymy w danym folderze. Pozwala to na lokalną konfigurację, np. kontroli dostępu, bez edytowania głównego pliku konfiguracyjnego Apache.  
Dodatkowo, możemy dodać moduły takie jak `mod_rewrite`, `mod_security` czy `mod_ssl`, które zwiększają bezpieczeństwo aplikacji webowej.

---
## Instal Python & Web Server

**Python Web Server** to prosta i szybka alternatywa dla Apache, pozwalająca na udostępnienie folderu jednym poleceniem.  
Aby z niego skorzystać, instalujemy `python3`, a następnie uruchamiamy:
`python3 -m http.server`
![[Pasted image 20250729130634.png]]

Python Server będzie odpalony na porcie `TCP/8000` wraz z folderem

Żeby hostować dodatkowy folder używamy 
`python3 -m http.server --directory /home/cry0l1t3/target_files`
![[Pasted image 20250729130819.png]]
To uruchomi Python Web Server na porcie `TCP/8000` i dzięki temu mamy dostęp do folderu `/home/cry0l1t3/target_files` za pomocą przeglądarki.

Możemy również transferować pliki do innego systemu przez wpisanie linka w przeglądarce i pobraniu plików. 

Można też hostować Python Web Server na innym porcie niż standardowymy
`python3 -m http.server 443`
![[Pasted image 20250729131129.png]]
To go zhostuje na porcie 443

---

## VPN

Virtual Private Network (VPN) funkcjonuje jako bezpieczny niewidzialny tunel który łączy nas z inną siecią pozwalając nam na niezakłócony i zabezpieczony dostęp. Osiąga się to za pomocą zaszyfrowanego tunelu pomiędzy klientem i serverem, zapewniając że wszystkie dane transmitowane przez to połączenie pozostają zabezpieczone.

Wśród popularnych VPN dla serwerów linuxowych:
- OPENVPN
- L2TP/IPsec
- PPTP
- SSTP
- SoftEther

OpenVPN jest najpopularniejszy bo jest open-source i jest kompatybilny z przeróżnymi distro Linuxa.
- szyfrowanie
- tunelowanie
- kształtowanie ruchu
- network routing
- przystosowanie do dynamicznych środowisk sieciowych

Dla pentesterów OpenVPN offeruje niesamowite możliwości.
- bezpieczne połączenie do sieci wewnętrznych
- sprawdzanie zabezpieczeń
- identyfikacja i zaadresowanie potencjalnych zagrożeń

---

### Install OpenVPN

Komenda: `sudo apt install openvpn -y`
![[Pasted image 20250729132116.png]]

OpenVPN można konfigurować poprzez edycje pliku `/etc/openvpn/server.conf`
Żeby się podłączyć do serwera OpenVPN możemy użyć pliku `.ovpn` który otrzymaliśmy od serwera i zapisac go na naszym systemie za pomocą komendy: `sudo openvpn --config internal.ovpn`
![[Pasted image 20250729132329.png]]
## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)