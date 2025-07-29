---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - theme/default
  - status/in-progress
  - review/pending
date: 2025-07-30
last_updated: 2025-07-30
---

# Study: [[Linux Main|Linux]]


# Logi systemowe w Linuxie

Logi systemowe to kluczowe narzędzie do monitorowania i analizy działania systemu. Zawierają informacje o zdarzeniach systemowych, aktywności użytkowników i aplikacji oraz zdarzeniach bezpieczeństwa[loggly.com](https://www.loggly.com/ultimate-guide/linux-logging-basics/#:~:text=,var%2Flog%2Fsecure)[global.ptsecurity.com](https://global.ptsecurity.com/en/research/analytics/how-to-detect-10-popular-pentester-techniques/#:~:text=,Network%20traffic). Dla pentesterów analiza logów pozwala wykrywać nietypowe zachowania – np. powtarzające się nieudane logowania, podejrzane połączenia sieciowe czy zmiany w plikach konfiguracyjnych – co wskazuje na potencjalne włamanie. Po testach bezpieczeństwa sprawdzamy logi, aby zweryfikować, czy nasze działania uruchomiły alerty (np. IDS) lub pozostały niezauważone. Dlatego ważne jest właściwe skonfigurowanie logowania (odpowiednie poziomy zapisu, zabezpieczenie plików logów) oraz regularne ich przeglądanie[digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-view-and-configure-linux-logs-on-ubuntu-debian-and-centos#:~:text=Linux%20uses%20the%20concept%20of,log%20file%20to%20be%20deleted)[sematext.com](https://sematext.com/blog/journald-logging-tutorial/#:~:text=you%E2%80%99ll%20see%20your%20own%20logs,or%20based%20on%20free%20space). W nowoczesnych dystrybucjach większość usług loguje do systemd-journald (binarny dziennik), który udostępnia je za pomocą `journalctl`, ale nadal często spotkamy tradycyjne pliki logów w katalogu `/var/log`.

---
## Dzienniki jądra (kernel log)

Dzienniki jądra rejestrują zdarzenia związane z samym jądrem systemu: ładowaniem sterowników, komunikacją sprzętową i wewnętrznymi błędami. Tradycyjnie trafiają one do `/var/log/kern.log` (w systemach Debian/Ubuntu) lub do `/var/log/messages` (w RHEL/CentOS)[loggly.com](https://www.loggly.com/ultimate-guide/linux-logging-basics/#:~:text=,var%2Flog%2Fsecure). Przykłady wpisów to błędy dysków, problemy z USB, czy komunikaty o błędach pamięci. Na ich podstawie można wykryć podatne sterowniki, potencjalne ataki na sprzęt lub przyczyny awarii. W systemach opartych na systemd jądro kieruje logi do journald; można je podejrzeć poleceniem `journalctl --dmesg` (wyświetla tylko komunikaty kernela)[sematext.com](https://sematext.com/blog/journald-logging-tutorial/#:~:text=,dmesg).

---
## Dzienniki systemowe (syslog)

Dzienniki systemowe zawierają ogólne zdarzenia systemowe: uruchomienia i zatrzymania usług, komunikaty demona init/systemd, błędy systemowe, informacje o zadaniach zaplanowanych (cron), restartach systemu itp. W systemach Debian/Ubuntu większość z nich znajduje się w `/var/log/syslog`, a w RHEL/CentOS w `/var/log/messages`[loggly.com](https://www.loggly.com/ultimate-guide/linux-logging-basics/#:~:text=,var%2Flog%2Fsecure). Przykładowe wpisy mogą wyglądać następująco:

```
Mar 01 12:00:01 server CRON[1234]: (root) CMD (sh /usr/local/bin/backup.sh)  
Mar 01 12:05:22 server sshd[3010]: Failed password for user from 10.0.0.1 port 2222 ssh2  
Mar 01 12:05:23 server kernel: [ 1234.567890] usb 1-1: New USB device found, idVendor=abcd, idProduct=1234  
Mar 01 12:06:00 server systemd[1]: Started Some Service.  
```

- W powyższym przykładzie widać zapis zadania `cron`, nieudane logowanie SSH, informację jądra o podłączeniu urządzenia USB oraz komunikat systemd o uruchomieniu usługi.
    
- Analizując `syslog`, warto zwrócić uwagę na nieudane próby logowania (znak możliwych ataków brute-force), błędy usług, nieoczekiwane restarty czy komunikaty o remoncie systemu (np. „re-mounted. Opts: errors=remount-ro”).
    
- W systemd można też filtrować takie logi: np. `journalctl -u sshd.service` pokaże wszystkie logi związane z demonem SSH[sematext.com](https://sematext.com/blog/journald-logging-tutorial/#:~:text=journalctl%20_SYSTEMD_UNIT%3Dsshd).
    

Zgodnie z dokumentacją, logi `syslog` można przesyłać do zewnętrznych systemów (SIEM) lub analityki za pomocą demona syslog (np. rsyslog, syslog-ng)[loggly.com](https://www.loggly.com/ultimate-guide/linux-logging-basics/#:~:text=1,uses%20port%20514%20for%20plaintext).

---

## Dzienniki uwierzytelniania (auth.log)

Rejestry uwierzytelniania śledzą próby logowania się użytkowników, zarówno udane, jak i nieudane. Na systemach Debian/Ubuntu informacje te trafiają do `/var/log/auth.log`, a na RedHat/CentOS do `/var/log/secure`[digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-view-and-configure-linux-logs-on-ubuntu-debian-and-centos#:~:text=As%20you%20can%20see%2C%20Debian,var%2Flog%2Fsecure). Znajdziemy tam wpisy SSH (`sshd`), użycia `sudo`, PAM i inne zdarzenia związane z autoryzacją. Przykład:
```
Mar 01 13:00:00 server sshd[5678]: Accepted publickey for user1 from 192.168.0.100 port 34567 ssh2: RSA SHA256:abcdef...  
Mar 01 13:00:02 server sudo:   user1 : TTY=pts/0 ; PWD=/home/user1 ; USER=root ; COMMAND=/bin/ls  
Mar 01 13:02:34 server sshd[5678]: Failed password for invalid user admin from 10.0.0.2 port 2233 ssh2  
```

- Pierwszy wpis pokazuje poprawne zalogowanie użytkownika `user1` za pomocą klucza publicznego, drugi – użycie komendy z `sudo`, trzeci – nieudaną próbę logowania. Tego typu dane są kluczowe do wykrywania nieautoryzowanych prób dostępu.
    
- Można wykryć np. ataki brute-force (liczne nieudane logowania SSH), przypadki podniesienia uprawnień (`sudo`) czy zmiany haseł.
    
- W systemd-journald te same zdarzenia dostępne są w dzienniku systemowym i można je przeszukiwać np. `journalctl -u sshd -p warning` (pokazuje logowania z co najmniej poziomem ostrzeżenia)[sematext.com](https://sematext.com/blog/journald-logging-tutorial/#:~:text=,dmesg).
    

---
## Dzienniki aplikacji

Większość kluczowych aplikacji zapisuje własne logi w podkatalogach `/var/log`. Przykładowe ścieżki domyślne to:

- **Apache (serwer WWW)** – logi dostępu i błędów w `/var/log/apache2/` (Debian/Ubuntu)[loggly.com](https://www.loggly.com/ultimate-guide/linux-logging-basics/#:~:text=Some%20applications%20also%20write%20log,explain%20in%20the%20next%20section).
    
- **Nginx (serwer WWW)** – logi w `/var/log/nginx/` (np. `access.log`, `error.log`).
    
- **MySQL/MariaDB** – logi w `/var/log/mysql/` (np. `mysql.log` lub `error.log`)[loggly.com](https://www.loggly.com/ultimate-guide/linux-logging-basics/#:~:text=Some%20applications%20also%20write%20log,explain%20in%20the%20next%20section).
    
- **PostgreSQL** – logi w `/var/log/postgresql/`, często z nazwą wersji (np. `postgresql-12-main.log`).
    
- **OpenSSH** – część logów (poza auth) idzie do `/var/log/auth.log` lub systemd-journald (już opisane wyżej).
    
- **Systemd** – systemd-journald przechowuje logi binarne w `/var/log/journal/`; można je odczytać `journalctl`.
    

Ponadto, istnieją **logi audytowe i dostępu** (ang. _audit_, _access_).

- _Logi dostępu_ (access logs) rejestrują próby dostępu użytkowników do zasobów (np. katalogi, pliki) czy zakończenie operacji.
    
- _Logi audytowe (auditd)_ – to systemowy mechanizm audytu, który może rejestrować szczegółowe informacje o zdarzeniach bezpieczeństwa i dostępie do plików. Domyślny plik to `/var/log/audit/audit.log`[linux-audit.com](https://linux-audit.com/linux-audit-log-files-in-var-log-audit/#:~:text=By%20default%20the%20Linux%20audit,related%20information%20such%20as%20events).
    
- **Auditd** może logować wywołania systemowe (syscall), np. każde uruchomienie programu (`execve`) czy modyfikację plików. Przydatne jest użycie narzędzi `ausearch` i `aureport` do analizy tych logów[linux-audit.com](https://linux-audit.com/linux-audit-log-files-in-var-log-audit/#:~:text=Tools)[global.ptsecurity.com](https://global.ptsecurity.com/en/research/analytics/how-to-detect-10-popular-pentester-techniques/#:~:text=,the%20names%20of%20publicly%20available). Na przykład każde uruchomienie nowego procesu przez użytkownika będzie widoczne dzięki zdarzeniu typu `execve` z auditd[global.ptsecurity.com](https://global.ptsecurity.com/en/research/analytics/how-to-detect-10-popular-pentester-techniques/#:~:text=,the%20names%20of%20publicly%20available).
    

Dzięki logom aplikacji i audytu można wykrywać m.in. ataki typu SQL injection, nietypowe żądania HTTP, czy nieautoryzowane zmiany w plikach konfiguracyjnych. Przykładowa linia z logu dostępu (aplikacji) może wyglądać tak:

`2023-03-07T10:15:23+00:00 servername privileged.sh: student accessed /root/secret_data.txt`
co sygnalizuje, że proces `privileged.sh` próbował odczytać chroniony plik.

---
## Dzienniki bezpieczeństwa

Niektóre narzędzia bezpieczeństwa prowadzą własne logi:

- **Fail2ban** – demony blokujące próby bruteforce logują zdarzenia (np. IP źródłowe) najczęściej do `/var/log/fail2ban.log` lub do sysloga (konfiguracja domyślnie trafia do `/var/log/messages`).
    
- **Zapora (np. UFW, iptables)** – logi ruchu lub blokad trafiają do `/var/log/ufw.log` (dla UFW) lub do ogólnego sysloga.
    
- **SELinux/AppArmor** – jeśli są włączone, logują próby naruszeń reguł bezpieczeństwa (np. niedozwolonych operacji) do `/var/log/audit/audit.log` lub specjalnych plików (`/var/log/audit/audit.log` dla SELinux lub `/var/log/kern.log` dla AppArmor w Debianie).
    
- **Narzędzia SIEM i zewnętrzne skrypty** – mogą agregować lokalne logi i wysyłać je do centralnego systemu (np. ELK, Splunk).
    

Analiza takich logów pozwala wykrywać nienormalne wzorce – np. wielokrotne zatrzymania/uruchomienia usługi, zmiany reguł zapory, czy pojawienie się nowych reguł/kont. Znajomość lokalizacji logów bezpieczeństwa (Fail2ban, UFW, SELinux) pomaga w kompleksowej ocenie stanu bezpieczeństwa systemu.

---
## Rotacja i przechowywanie logów

Logi mogą szybko zajmować dużo miejsca, dlatego stosuje się rotację plików logów. W systemach Linux używa się narzędzia **logrotate**, które okresowo tworzy nowe pliki logów i archiwizuje starsze (np. kompresując je), aby zapobiec przepełnieniu dysku[digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-view-and-configure-linux-logs-on-ubuntu-debian-and-centos#:~:text=Linux%20uses%20the%20concept%20of,log%20file%20to%20be%20deleted). Domyślna konfiguracja (np. w `/etc/logrotate.conf` i `/etc/logrotate.d/`) określa, ile starych wersji ma być zachowanych. Specjalnym przypadkiem są pliki `wtmp`/`btmp` (rejestrujące logowania), które rotowane są miesięcznie.

W systemd/journald zarządzanie rozmiarem dziennika odbywa się w pliku konfiguracyjnym `/etc/systemd/journald.conf`. Można tam ustawić limity (np. `SystemMaxUse`) i reguły automatycznej rotacji, aby starych logów nie robiło się za dużo[sematext.com](https://sematext.com/blog/journald-logging-tutorial/#:~:text=you%E2%80%99ll%20see%20your%20own%20logs,or%20based%20on%20free%20space). Journald np. pozwala ograniczyć maksymalną zajętą przestrzeń dyskową przez logi lub usuwać najstarsze wpisy w razie braku miejsca.

Ważne jest też bezpieczne przechowywanie: zwykle tylko root lub użytkownicy z grupy `adm` (lub `systemd-journal`) mają prawo odczytu plików logów. Można też wysyłać kopie logów na zdalny serwer logów, aby uniknąć ich utraty przy zaatakowaniu lokalnego systemu.

---

## Narzędzia do analizy logów

Logi analizuje się zarówno interaktywnie, jak i za pomocą skryptów. Podstawowe narzędzia to:

- `tail -f /var/log/plik.log` – monitorowanie nowych wpisów w czasie rzeczywistym.
    
- `grep`, `egrep` – wyszukiwanie konkretnych ciągów tekstu (IP, nazw użytkowników, komunikatów błędów) w logach.
    
- `less`, `more` – przeglądanie dużych plików logów z nawigacją.
    
- `sed`, `awk` – przetwarzanie i filtrowanie logów skryptowo.
    

Dla logów journald używamy `journalctl`: można odfiltrować wpisy po czasie (`--since`, `--until`), priorytecie (`-p`) czy jednostce systemd (`-u`)[sematext.com](https://sematext.com/blog/journald-logging-tutorial/#:~:text=journalctl%20_SYSTEMD_UNIT%3Dsshd). Przykładowo, `journalctl -u sshd -p err` wyświetli tylko błędne i krytyczne logowania SSH. Nowe wersje systemd oferują też opcję `--grep` do filtrowania po treści.

W przypadku audytów bezpieczeństwa można stosować dedykowane narzędzia: `ausearch` wyszukuje określone typy zdarzeń w `/var/log/audit/audit.log`, a `aureport` tworzy podsumowania (np. liczba nieudanych prób logowania czy modyfikacji plików)[linux-audit.com](https://linux-audit.com/linux-audit-log-files-in-var-log-audit/#:~:text=Although%20the%20log%20file%20is,both%20of%20them%20and%20how).

Poprawna analiza logów – systematyczna i objęta regułami – pozwala szybko zidentyfikować incydenty i miejsca wymagające wzmocnienia zabezpieczeń. Dzięki cytowanym źródłom i narzędziom można efektywnie wykorzystać zapisy systemowe do celów diagnostyki i bezpieczeństwa[loggly.com](https://www.loggly.com/ultimate-guide/linux-logging-basics/#:~:text=,var%2Flog%2Fsecure)[global.ptsecurity.com](https://global.ptsecurity.com/en/research/analytics/how-to-detect-10-popular-pentester-techniques/#:~:text=,the%20names%20of%20publicly%20available).

**Źródła:** Oficjalna dokumentacja i praktyki zarządzania logami w Linuksie[loggly.com](https://www.loggly.com/ultimate-guide/linux-logging-basics/#:~:text=,var%2Flog%2Fsecure)[linux-audit.com](https://linux-audit.com/linux-audit-log-files-in-var-log-audit/#:~:text=By%20default%20the%20Linux%20audit,related%20information%20such%20as%20events)[digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-view-and-configure-linux-logs-on-ubuntu-debian-and-centos#:~:text=As%20you%20can%20see%2C%20Debian,var%2Flog%2Fsecure)[sematext.com](https://sematext.com/blog/journald-logging-tutorial/#:~:text=you%E2%80%99ll%20see%20your%20own%20logs,or%20based%20on%20free%20space)[global.ptsecurity.com](https://global.ptsecurity.com/en/research/analytics/how-to-detect-10-popular-pentester-techniques/#:~:text=,the%20names%20of%20publicly%20available), a także artykuły eksperckie dotyczące systemd-journald i audytu systemowego[sematext.com](https://sematext.com/blog/journald-logging-tutorial/#:~:text=journalctl%20_SYSTEMD_UNIT%3Dsshd)[global.ptsecurity.com](https://global.ptsecurity.com/en/research/analytics/how-to-detect-10-popular-pentester-techniques/#:~:text=,Network%20traffic).

























































## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)