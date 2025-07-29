---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - status/in-progress
  - review/pending
  - theme/cybersecurity
  - theme/os
date: 2025-07-28
last_updated: 2025-07-28
---

# [[Linux Main|Study - {{Linux}}]]

## Ogólnie
Wyobraź sobie nowego pracownika Alexa który dosatje PC z Linuxem żeby wykonywać swoją pracę. Jako administrator musisz stworzyć dla niego nowe konto i dodać go do odpowiednich grup żeby mógł wykonywac swoją pracę. Dodatkowo może wydarzyć się sytuacja w której alex musi wykonac komendy z wyższymi uprawnieniami albo jako inny użytkownik żeby wykonać konkretne zadania

## Zawartość
### SUDO
`/etc/shadow` jest plikiem systemowym który zawiera zakodowane informacje o hasłach dla wszystkich kont użytkowników. Jest do odczytu i edycjii tylko dla roota.
Żeby wykonać zadanie za pomoca wyższych uprawnień można użyć `sudo` czyli "superuser do". To pozwala wykonywać zadania bez logowania sie jako root co jest najlepsze dla bezpieczeństwa systemu.

### CheatSheet
| **Polecenie**           | **Opcje**                                                                                                  | **Opis / Przykład użycia**                                |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| `sudo`                  | `-u <użytkownik>` (wykonaj jako inny użytkownik)`-i` (pełna sesja roota)`-s` (interaktywny shell)          | `<code>sudo -u www-data systemctl restart apache2</code>` |
| `su`                    | `-` lub `-l` (ładuje pełne środowisko celu)`-c '<komenda>'` (wykonaj jednorazowo)                          | `<code>su - john</code>`                                  |
| `useradd`               | `-m` (katalog domowy)`-d <ścieżka>` (własna ścieżka)                  `-s <powłoka>``-G <grupy>``-u <UID>` | `<code>useradd -m -s /bin/zsh -G sudo,dev alice</code>`   |
| `userdel`               | `-r` (usuń katalog domowy i pocztę)`-f` (wymuś usunięcie)                                                  | `<code>userdel -r bob</code>`                             |
| `usermod`               | `-aG <grupy>` (dodaj do grup)`-g <główna_grupa>``-d <katalog>` (zmień home)`-s <powłoka>`                  | `<code>usermod -aG docker john</code>`                    |
| `passwd`                | `-l` (zablokuj konto)`-u` (odblokuj)`-d` (usuń hasło)`-e` (wymuś zmianę przy następnym logowaniu)          | `<code>passwd -l mike</code>`                             |
| `addgroup` / `groupadd` | `--system` (grupa systemowa)`-g <GID>`                                                                     | `<code>addgroup --system backup</code>`                   |
| `delgroup` / `groupdel` | —                                                                                                          | `<code>delgroup oldteam</code>`                           |
| `groups`                | —                                                                                                          | `<code>groups alice</code>`                               |
| `id`                    | `-u` (UID)`-g` (GID głównej grupy)`-G` (lista wszystkich GID)                                              | `<code>id john</code>`                                    |
| `whoami`                | —                                                                                                          | `<code>whoami</code>`                                     |
| `getent passwd`         | `<użytkownik>` — zwróć tylko ten rekord                                                                    | `<code>getent passwd root</code>`                         |
| `getent group`          | `<grupa>` — zwróć tylko rekord grupy                                                                       | `<code>getent group sudo</code>`                          |
| `chown`                 | `-R` (rekursywnie)`--from=<stary>:<stara>` (zmień tylko z określonego)                                     | `<code>chown -R www-data:www-data /var/www</code>`        |
| `chmod`                 | Symbolicznie: `u+rwx,g+rx,o-rwx`Oktalnie: `644`, `755`, `4755`                                             | `<code>chmod 750 /usr/local/bin/script.sh</code>`         |
| `umask`                 | `<mask>` (np. `022`, `0077`)                                                                               | `<code>umask 027</code>`                                  |
| `sudo visudo`           | `-f <plik>` (edytuj inny niż `/etc/sudoers`)                                                               | `<code>sudo visudo -f /etc/sudoers.d/custom</code>`       |
| `loginctl`              | `list-sessions``list-users``terminate <ID>`                                                                | `<code>loginctl terminate 3</code>`                       |
| `faillog`               | `-u <użytkownik>` (dla konkretnego)`-r` (reset)                                                            | `<code>faillog -u john</code>`                            |
| `lastlog`               | `-u <użytkownik>`                                                                                          | `<code>lastlog -u alice</code>`                           |
| `last`                  | `-n <liczba>` (ile wpisów)`-f <plik>`                                                                      | `<code>last -n 20</code>`                                 |
| `who` / `w`             | `-u` (wyświetl idle time w who)`--help`                                                                    | `<code>w</code>`                                          |
| `finger`                | —                                                                                                          | `<code>finger mike</code>`                                |
| `pam-auth-update`       | `--package <moduł>` (dodaj/usuń moduł)`--force`                                                            | `<code>pam-auth-update --package tcb</code>`              |
| `newgrp`                | `<grupa>` — przełącz bieżący shell do nowej grupy                                                          | `<code>newgrp docker</code>`                              |
| `chage`                 | `-l` (listuj status)`-E <data>` (wygaśnięcie)`-m <dni>` (min)`-M <dni>` (max)`-I <dni>` (inaktywność)      | `<code>chage -l alice</code>`                             |
| `mkhomedir_helper`      | `[użytkownik]`                                                                                             | `<code>mkhomedir_helper john</code>`                      |
| —                       | **Sticky Bit**: `<code>chmod +t /tmp</code>` (zabezpieczenie plików w ścieżkach tymczasowych)              | —                                                         |

## Tipy
1. **Unikaj `chmod 777`** – to jak wystawić całe repo w publicznym Internecie.
2. **Używaj ról i grup** zamiast przydzielać każdemu pojedyncze uprawnienia.
3. **Zawsze testuj plik sudoers** za pomocą `visudo`, by nie zablokować dostępu root.
4. **Stale monitoruj** `/var/log/auth.log` (Debian/Ubuntu) lub `/var/log/secure` (RHEL).
5. **Regularnie rotuj hasła** i sprawdzaj wygaśnięcia (`chage -l użytkownik`).
6. **Utrzymuj porządek** w `/etc/skel` – nowi użytkownicy od razu dostaną wzorcową konfigurację.
7. **Minimalizuj dostęp** do root – korzystaj z `sudo` w trybie „least privilege”.
8. **Ustaw VPZ (Sticky Bit) na katalogach tymczasowych**: `<code>chmod +t /tmp</code>`.
## Zadania
- Which option needs to be set to create a home directory for a new user using "useradd" command??
	- `-m` - katalog domowy
- Which option needs to be set to lock a user account using the "usermod" command? (long version of the option)
	- Używając `man usermod` wiemy że to funkcja `--lock` inaczej `-l`
-  Which option needs to be set to execute a command as a different user using the "su" command? (long version of the option)
	- `--command` czyli inaczej `-c` 


## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)