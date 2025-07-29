---
aliases:
  - Study - {{Linux}}
  - Linux
  - General
tags:
  - type/study
  - "#theme/cybersecurity"
  - "#theme/os"
  - context/studies
date: 2025-07-27
last_updated: 2025-07-27
---
#linux [[Deskryptory]] [[Linux Main|Study - {{Linux}}]]
# Study: {{Linux}}

## Objective
Dowiedzieć się więcej na temat systemu operacyjnego Linux
## Content
### Linux
Linux został napisany w języku C w oparciu o system Unix przez Linusa Torvaldsa. 
Istnieje ponad 600 dystrybucji Linuxa:
- Ubuntu
- Devian
- Fedora
- Gentoo Linux
- Red Hat
- Mint
- ArchLinux
- Kali Linux

| Komponent                                                                  | Opis                                               |
| -------------------------------------------------------------------------- | -------------------------------------------------- |
| [Bootloader](https://pl.wikipedia.org/wiki/Program_rozruchowy)             | Kawałek kodu który bootuje system                  |
| [OS Kernel](https://pl.wikipedia.org/wiki/J%C4%85dro_systemu_operacyjnego) | Główny składnik, zarządza zasobami                 |
| [Deamons](https://pl.wikipedia.org/wiki/Demon_(informatyka))               | Niezależny od użytkownika skrypt, działający w tle |
| [OS Sheel](https://pl.wikipedia.org/wiki/Pow%C5%82oka_systemowa)           | Bash                                               |
| GUI                                                                        | Nakładeczka żeby nie pisać jak 100 lat temu        |
## Architektura Linuxa
![[Pasted image 20250727181113.png]]

## System Hierarchi Plików
![[Pasted image 20250727181228.png]]

| /      | Główny Katalog (Root) zawiera wszystkie pliki niezbędne do uruchomienia systemu. Pliki i systemy plików montowane są jako podkatalogi katalogu root. |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| /bin   | Niezbędne Programy dla użytkowników                                                                                                                  |
| /boot  | Bootloader, Pliki potrzebne do uruchomienia systemu                                                                                                  |
| /dev   | Pliki Urządzeń które umożliwiają dostęp do wszystkich podłączonych urządzeń sprzętowych                                                              |
| /etc   | Pliki konfiguracyjne lokalnego systemu i pliki aplikacji                                                                                             |
| /home  | Każdy użytkownik ma tutaj swój podkatalog                                                                                                            |
| /lib   | Współdzielone biblioteki niezbędne do startu systemu                                                                                                 |
| /media | Punkt montowania nośników wymiennych (pendrive)                                                                                                      |
| /mnt   | Tymczasowy punkt montowania dla zwykłych systemów plików                                                                                             |
| /opt   | Dodatkowe pliki np. narzędia od firm zewnętrznych                                                                                                    |
| /root  | Katalog domowy admina                                                                                                                                |
| /sbin  | Pliki Wykonalne używane do administracji systemu                                                                                                     |
| /tmp   | Pliki Tymczasowe                                                                                                                                     |
| /usr   | Programy Biblioteki itp                                                                                                                              |
| /var   | Zmienne dane takie jak logi, maile         

-------------------------------------------------------------------------------
### Bash

| $   | User |
| --- | ---- |
| #   | Root |

|**Special Character**|**Description**|
|---|---|
|`\d`|Date (Mon Feb 6)|
|`\D{%Y-%m-%d}`|Date (YYYY-MM-DD)|
|`\H`|Full hostname|
|`\j`|Number of jobs managed by the shell|
|`\n`|Newline|
|`\r`|Carriage return|
|`\s`|Name of the shell|
|`\t`|Current time 24-hour (HH:MM:SS)|
|`\T`|Current time 12-hour (HH:MM:SS)|
|`\@`|Current time|
|`\u`|Current username|
|`\w`|Full path of the current working directory|
# Komendy
#komendylinux
https://www.gnu.org/software/coreutils/manual/coreutils.html
https://www.fosslinux.com/45587/linux-command-cheat-sheet.htm

| **Command** | **Description**                                                                                                                    |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `whoami`    | Displays current username.                                                                                                         |
| `id`        | Returns users identity                                                                                                             |
| `hostname`  | Sets or prints the name of current host system.                                                                                    |
| `uname`     | Prints basic information about the operating system name and system hardware.                                                      |
| `pwd`       | Returns working directory name.                                                                                                    |
| `ifconfig`  | The ifconfig utility is used to assign or to view an address to a network interface and/or configure network interface parameters. |
| `ip`        | Ip is a utility to show or manipulate routing, network devices, interfaces and tunnels.                                            |
| `netstat`   | Shows network status.                                                                                                              |
| `ss`        | Another utility to investigate sockets.                                                                                            |
| `ps`        | Shows process status.                                                                                                              |
| `who`       | Displays who is logged in.                                                                                                         |
| `env`       | Prints environment or sets and executes command.                                                                                   |
| `lsblk`     | Lists block devices.                                                                                                               |
| `lsusb`     | Lists USB devices                                                                                                                  |
| `lsof`      | Lists opened files.                                                                                                                |
| `lspci`     | Lists PCI devices.                                                                                                                 |
|             | ![[Pasted image 20250727193108.png]]                                                                                               |
|             |                                                                                                                                    |
```shell-session
find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null
```
| **Option**            | **Description**                                                                                                                                                                                                                               |     |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| `-type f`             | Typ danych f = file                                                                                                                                                                                                                           |     |
| `-name *.conf`        | -name * = wszystkie z rozszerzeniem .conf                                                                                                                                                                                                     |     |
| `-user root`          | pliki których właścicielem jest root                                                                                                                                                                                                          |     |
| `-size +20k`          | Większe niż 20KiB                                                                                                                                                                                                                             |     |
| `-newermt 2020-03-03` | Pokazuje wszystkie nowsze niż podana data                                                                                                                                                                                                     |     |
| `-exec ls -al {} \;`  | Wykonuje komende - `ls -al` <br>    <br>`{}` wstawia w to miejsce **aktualną ścieżkę** znalezionego pliku/katalogu.<br>    <br>`\;` kończy listę argumentów dla `-exec`.                                                                      |     |
| `2>/dev/null`         | This is a `STDERR` redirection to the '`null device`', which we will come back to in the next section. This redirection ensures that no errors are displayed in the terminal. This redirection must `not` be an option of the 'find' command. |     |
      

-------------------------------------------------------------------------------


## Review Questions
- Jakbym chciał znaleźć za pomoca komendy locate wszystko zwiazane z netcatem i nc to jakiego rozszerzenia powinienem używać?
  locate '*netcat*'
- Chce wiedziec ile plików w systemie ma rozszerzenie .log , za pomoca locate *.log mogę je wypisać ale czy istnieje sposób żebym wiedział od razu ile ich jest a nie liczyć je po kolei?
	find / -type f -iname '*.log' 2>/dev/null | wc -l
	/ – przeszukaj cały system (możesz podać inny katalog, np. .)
	-type f – tylko pliki
	-iname '*.log' – bez uwzględniania wielkości liter (.log, .LOG, itd.)
	2>/dev/null` – ukryj komunikaty o odmowie dostępu
	| wc -l – zlicz linie, czyli liczbę plików

- Jeśli chcesz sprawdzić, **jakie usługi nasłuchują na jakich interfejsach i portach**, użyj: `ss -tuln`
	- - `-t` — TCP
    
	- `-u` — UDP
    
	- `-l` — tylko nasłuchujące
    
	- `-n` — nie rozwiązywać nazw hostów ani portów (pokazuje numeryczne adresy)
	
	Przykładowa linia:
	`tcp   LISTEN  0      128    [::]:22      [::]:*`    
	- kolumna 1: protokół (`tcp`)
	- kolumna 2: stan (`LISTEN`)
    - kolumna 5: lokalny adres i port (`[::]:22`)
    ### Dlaczego `[::]:22` się liczy, a `0.0.0.0:22` nie?
    - Pytanie brzmi: **“listening on _all interfaces_ (not just localhost and IPv4)”**
    - Usługa na `0.0.0.0:22` jest _tylko_ IPv4‑all.
    - Usługa na `[::]:22` (dual‑stack) obsłuży zarówno ruch IPv6 _i_ IPv4, więc spełnia kryterium “nie tylko IPv4”.
    ### Filtracja wyjścia za pomocą `awk` ![[Pasted image 20250728112946.png]]

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)