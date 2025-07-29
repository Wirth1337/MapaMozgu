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

## Ogólnie
#networking
Sieć to pajęczyna. Ogromna wielowymiarowa pajęczyna która, tak jak ta OG przesyła drgania a komputer jako pająk te drgania odczytuje.

Wyobraź sobie scenariusz w którym przeprowadzasz pentest i napotykasz na hosta który jest podłączony do innego servera przez niezaszyfrowany FTP serwer. Dzięki temu możemy zapisać jego credentials w pliku tekstowym. Sytuacja byłaby mniej prawdopodobna jeżeli użytkownik byłby świadomy że FTP nie szyfruje danych ani połączenia. 

## Treść
## SSH
Secure Shell (SHH) jest protokołem sieciwym który zapewania zabezpieczoną transmijse danych i komend przez sieć.

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

---
#

## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)