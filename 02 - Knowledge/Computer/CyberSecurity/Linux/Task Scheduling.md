---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - theme/default
  - status/in-progress
  - review/pending
date: 2025-07-28
last_updated: 2025-07-28
---

# [[Linux Main|Study - {{Linux}}]]

## Ogólnie
Kto nie lubi automatyzować wszystkiego czego nam się nie chce robić. Task Scheduling daje nam taką opcję przy naprzykład.
- Aktualizacje oprogramowania
- Wykonanie Skryptów
- Zarządzanie Bazą Danych
- Automatyzacja Backupów

---
## Treść
### Systemd
Systemd jest usługą w Linuxie która zaczyna processy i skrypty w określonym czasie.

1. Stwórz Timer (ustaw kiedy twój mytimer.service powinien działać)
2. Utwórz usługe (wykonaj komendę lub skrypt)
3. Aktywuj Timer

---
## Stwórz Timer

Żeby utworzyć timer dla systemd musimy utworzyc folder w którym skrypt timera będzie przechowywany.
![[Pasted image 20250728190724.png]]
`sudo mkdir /etc/systemd/system/mytimer.timer.d`
`sudo vim /etc/systemd/system/mytimer.timer`

Następnie musimy stworzyć skrypt który konfiguruje timer. 

**Skrypt musi zawierać:**
- unit - opis dla timera
- timer - kiedy go wystartować i aktywować
- install - gdzie zainstalować

---
### Mytimer.timer
```
`
[Unit]
Description=My Timer

[Timer]
OnBootSec=3min
OnUnitActiveSec=1hour

[Install]
WantedBy=timers.target
`
```

Tutaj ustawiamy jak chcemy z niego korzystać.
Przykładowo jeżeli chcemy żeby system go odpalał na bootcie powinniśmy użyć `OnBootSec` w `Timer`
Jeżeli natomiast chcemy żeby się uruchamiał reguralnie używamy wtedy `OnUnitActiveSec`

---
### Utwórz Usługę
![[Pasted image 20250728191322.png]]
`sudo vim /etc/systemd/system/mytimer.service`

Tutaj ustawiamy nazwę i ustalamy pełną ścieżke do skryptu. 

`multi-user.target` jest systemem który się aktywuje gdy startujemy normalny **multi-user mode**. Definiuje on usługi które powinny startować podczas normalnego startu systemu.

```

[Unit]
Description=My Service

[Service]
ExecStart=/full/path/to/my/script.sh

[Install]
WantedBy=multi-user.target
```

Po tym dajemy systemd odczytać foldery żeby zaimplementował zmiany.

---
## Reload System

![[Pasted image 20250728191925.png]]
`sudo systemctl daemon-reload`
Po tym możemy użyć systemctl żeby wystartować usługe manualnie i ustawić autostart

---

### Start the Timer & Service
![[Pasted image 20250728192332.png]]
```
sudo systemctl start mytimer.timer
sudo systemctl enable mytimer.timer
```

Dzięki temu `mytimer.service` będzie odpalał się automatycznie w zależności od przedziałów czasowych które ustawie w `mytimer.timer`

---
## Cron
Cron to następne narzędzie które pozwala na automatyzacje procesów. żeby ustawić cron daemona musimy przechować zadanie w pliku `crontab` i potem dać znać deamonowi kiedy odpalić taska. Potem możemy go zautomatyzować poprzez konfiguracje cron daemona. 
### 📆 Składnia Crontaba – ramy czasowe

| **Zakres czasu** | **Opis (PL)**                            | **Zakres wartości**                         |
| ---------------- | ---------------------------------------- | ------------------------------------------- |
| **Minuty**       | W której minucie zadanie ma się wykonać  | `0–59`                                      |
| **Godziny**      | W której godzinie zadanie ma się wykonać | `0–23`                                      |
| **Dni miesiąca** | Którego dnia miesiąca ma się wykonać     | `1–31`                                      |
| **Miesiące**     | W którym miesiącu ma się wykonać         | `1–12` lub `jan-dec`                        |
| **Dni tygodnia** | W który dzień tygodnia ma się wykonać    | `0–7` (`0` i `7` = niedziela) lub `mon-sun` |
|                  |                                          |                                             |
### 🔢 Symbole w cronie
- `*` – dowolna wartość (np. każda minuta/godzina/miesiąc)
- `,` – lista wartości (np. `1,15`)
- `-` – zakres wartości (np. `1-5`)
- `/` – krok (np. `*/10` co 10 jednostek)

### Przykładowy skrypt
![[Pasted image 20250728194017.png]]
```

# System Update
0 */6 * * * /path/to/update_software.sh

# Execute Scripts
0 0 1 * * /path/to/scripts/run_scripts.sh

# Cleanup DB
0 0 * * 0 /path/to/scripts/clean_database.sh

# Backups
0 0 * * 7 /path/to/scripts/backup.sh
```
### 🕒 Zadania cron – przykłady

| **Opis zadania**      | **Czas wykonania**                          | **Cron składnia**       | **Skrypt**                  |
|------------------------|---------------------------------------------|--------------------------|-----------------------------|
| **Aktualizacja systemu** | Co 6 godzin                                | `0 */6 * * *`            | `/ścieżka/update_software.sh` |
| **Wykonanie skryptów**  | 1. dnia miesiąca o północy                 | `0 0 1 * *`              | `/ścieżka/run_scripts.sh`      |
| **Czyszczenie bazy**    | W każdą niedzielę o północy (`0 = niedz.`) | `0 0 * * 0`              | `/ścieżka/clean_database.sh`   |
| **Kopie zapasowe**      | W każdą niedzielę o północy (`7 = niedz.`) | `0 0 * * 7`              | `/ścieżka/backup.sh`           |

---

### 📬 Dodatki
- Możesz skonfigurować **powiadomienia e-mail** po sukcesie/błędzie zadania.
- Warto logować wynik działania za pomocą przekierowań:
  ```bash
  /skrypt.sh >> /var/log/skrypt.log 2>&1
```

---

## Systemd vs Cron
Robią to samo ale inaczej
**Systemd** - tworzę timer i skrypt usługi którty mówi systemowi kiedy odpalać zadanie
**Cron** - tworzę plik `crontab` który mówi cron daemonowi kiedy odpalać taska

---
## Zadanka
- What is the Type of the service of the "dconf.service"?
![[Pasted image 20250728195214.png]]
- `find /etc/systemd /lib/systemd /usr/lib/systemd -type f -name "*dconf*.service"`
- `cat /lib/systemd/user/dconf.service`
Type = dbus
## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)