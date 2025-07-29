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
Usługi nazywane też `daemons` i nie, nie jest to Capra Demon i jego przydupasy. Są to komponenty które działają po cichu w tle bez "bezpośredniej ingerencji użytkownika".
Można je podzielić na dwa typy
### Usługi systemowe
- Usługi które są wymagane do odpalenia OS
- Coś jak silnik akumlator i inne takie bez nich auto nie pojedzie
### Usługi Zainstalowane Przez Usera
- Dodane przez użytkownika i inne które dostarczają określone funkcje i możliwości.
- Coś jak klimatyzacja i radio w samochodzie

---

## Treść
### Deamons
Daemons określa się jako `d` na końcu nazwy programu na przykład:
- **sshd (SSH Daemon)
- **systemd (System Daemon)
### Podział usług
1. Start/Restart usługi lub procesu
2. Zatrzymanie usługi lub procesu
3. Sprawdzenie co się dzieje/działo z usługą lub procesem
4. Włącz/Wyłącz usługę lub proces podczas boota
5. Znajdź usługę lub proces

**init system - initializaton system - systemd**

Kolejność wykonywania procesów podczas boota określa **Proces ID (PID)**. Każdy proces ma przypisany numer który można sprawdzić w foldrze `/proc/` który zawiera informacje o każdym procesie.

Procesy mogą mieć również **Parent Process ID (PPID)** który mówi czy są zaczynane przez inny proces które czyni je potomnym.

---
### Systemctl

**Startujemy proces następującą komendą:
![[Pasted image 20250728180159.png]]
Sprawdzamy czy działa bez błędów
![[Pasted image 20250728180211.png]]
Możemy go dodać do SysV żeby oznajmić systemowi że chcemy go mieć odpalonego przy bootcie:
![[Pasted image 20250728180346.png]]
Kiedy zrobimy reeboot proces będzie odpalał się automatycznie. Możemy to sprawdzić przy użyciu `ps`
![[Pasted image 20250728180458.png]]
Możemy też użyć `systemctl` żeby wylistować wszystkie usługi.
![[Pasted image 20250728180545.png]]
Jest całkiem możliwe że usługi nie startują przez error. Żeby to sprawdzić używamy journalctl, żeby sprawdzić logi**
![[Pasted image 20250728180647.png]]
### Kill the Process
Stany Procesu:
- Działa
- Czeka (na zdarzenie lub zasób systemowy)
- Zatrzymany
- Zombie (Zatrzymany ale dalej jest na liście procsów)

| Stan | Opis | Komenda |  
|----------|-----------------------------------|-------------------|  
| Running | Działa | `ps` |  
| Sleeping | Czeka na I/O | |  
| Stopped | Zatrzymany sygnałem | |  
| Zombie | Zakończony, ale nie zebrany przez PPID | |

Procesy można kontrolować za pomocą:
- kill
- pkill
- pgerp
- killall
Żeby dokonać egzekucji na procesie w imię rewolucji francuskiej wysyłamy do niego sygnał tak jakby lud wysyłał list do arystokracji z zaproszeniem na test gilotyny.
![[Pasted image 20250728181019.png]]

---
### **Najczęściej używane sygnały**
|**Nr**|**Sygnał**|**Opis**|
|--:|---|---|
|`1`|`SIGHUP`|Terminal sterujący został zamknięty – pozwól procesowi na odświeżenie konfiguracji.|
|`2`|`SIGINT`|[Ctrl]+C przerwie proces – wysyła żądanie przerwania.|
|`3`|`SIGQUIT`|[Ctrl]+\ przerwie i wygeneruje zrzut stanu (core dump).|
|`9`|`SIGKILL`|Natychmiastowe zabicie procesu – bez możliwości obsługi ani sprzątania.|
|`15`|`SIGTERM`|Łagodne zakończenie – pozwól procesowi zamknąć się poprawnie.|
|`19`|`SIGSTOP`|Zatrzymaj (suspend) proces – nie można go obsłużyć ani przechwycić.|
|`20`|`SIGTSTP`|[Ctrl]+Z zawiesza proces – użytkownik może potem `fg`/`bg` wznowić jego wykonanie.|
Przykładowo gdyby program się wywalił o własne nogi i leżał na ziemi to możemy go dobić za pomocą
`kill 9 <PID>`

Za pomocą CTRL + Z możemy wysłać SIGTSTP do kernela i zatrzymać proces w terminalu.

**Wszystkie procesy w tle można wyświetlić za pomocą komendy `jobs`
![[Pasted image 20250728181535.png]]
Za pomocą `bg` można wrzucić proces do tła.
![[Pasted image 20250728181617.png]]
Można też to zrobić za pomocą AND `&`
![[Pasted image 20250728181741.png]]
Gdy Proces się zakończy zobaczymy Rezultat**
![[Pasted image 20250728181804.png]]

---
### Jak wrzucić proces na pierwszy plan 

Jak Tarantino wrzuca stopy na pierwszy plan zrobimy to samo z procesem.

Używamy `jobs` żeby wyświetlić procesy w tle
Następnie używamy `fg <ID>` żeby go wrzucić na pierwszy plan.
![[Pasted image 20250728183323.png]]

---
## Wykonaj wiele komend

Są 3 możliwości żeby wykonać wiele komend jedna po drugiej.
Są one odzielone od siebie za pomocą:
- `;` - używamy by wykonać komende i zignorować wynik poprzedniej (**również błąd**)
![[Pasted image 20250728183624.png]]
- `&&` - Jeżeli jakaś komenda kończy się błędem kolejna się nie wykona
![[Pasted image 20250728183706.png]]
- `|` - tutaj zależy od poprawności i rezulatatu poprzedniego procesu

---

## Zadanka
- Use the "systemctl" command to list all units of services and submit the unit name with the description "Load AppArmor profiles managed internally by snapd" as the answer.
	-`systemctl > status.txt` wrzucamy logi do pliku
	- `grep -E '(AppArmor)' status.txt` sprawdzamy linie z AppArmor
	- Lub `grep "Load AppArmor profiles managed internally by snapd" status.txt`



## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)