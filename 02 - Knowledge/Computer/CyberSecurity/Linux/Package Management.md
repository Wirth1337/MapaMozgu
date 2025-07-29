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
**Pakiet**: archiwum zawierające binarki, pliki konfiguracyjne, metadane: zależności, skrypty (`pre/post install`), informacje o aktualizacjach.

Funkcje Menadżera Pakietów:
- Pobieranie pakietów
- Rozwiązywanie zależności
- Format binarny paczki
- Lokalizacja instalacji i konfiguracji
- Dodatkowe systemowe konfiguracje i funkcje
- Kontrola Jakości
- 
Działa trochę jak Winrar, który dodatkowo by instalował archiwum a jakby pojawiła się jego nowa wersja to by ja aktualizował.

--- 
## Zawartość
### Zarządzanie pakietami i repo
| **Polecenie** | **Opis**                                                            | **Najważniejsze opcje / przykłady**                                                                                                       |
| ------------- | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `dpkg`        | Niskopoziomowe zarządzanie pakietami `.deb`                         | `-i` (instaluj), `-r` (usuń), `-l` (lista), `-s` (status), `-L` (lista plików), `-S` (plik → pakiet)                                      |
| `apt`         | Wysokopoziomowy interfejs APT                                       | `update` (odśwież listy), `upgrade` (zaktualizuj), `install <pakiet>`, `remove <pakiet>`, `search <fraza>`, `show <pakiet>`, `autoremove` |
| `aptitude`    | Interaktywny menedżer pakietów z TUI i zaawansowanym rozwiązywaniem | `aptitude` (uruchom TUI), `aptitude install <pakiet>`, `aptitude why <pakiet>`                                                            |
| `snap`        | Kontenerowe paczki od Canonicala                                    | `install <pakiet>`, `remove <pakiet>`, `refresh` (aktualizuj), `list`, `info <pakiet>`                                                    |
| `gem`         | Menedżer pakietów Ruby                                              | `install <pakiet>`, `uninstall <pakiet>`, `update`, `list`, `search <nazwa>`                                                              |
| `pip`         | Menedżer pakietów Python (wspiera VCS)                              | `install <pakiet>`, `uninstall <pakiet>`, `list`, `show <pakiet>`, `freeze > requirements.txt`                                            |
| `git`         | System kontroli wersji                                              | `clone <url>`, `pull`, `push`, `status`, `log`, `branch`, `checkout <gałąź>`, `merge`, `rebase`, `stash`, `reset --hard`                  |

--- 
### 🧙 Mistrzowskie Tipy

- 🔍 **`dpkg -S /ścieżka/do/pliku`** – znajdź, z którego pakietu pochodzi plik.
- 🧼 **`apt autoremove`** – regularnie czyść zależności, których już nie używasz.
- 💥 **`apt install -f`** – napraw złamane zależności, np. po błędnym dpkg.
- 🪄 **`aptitude why`** – jedno z najpotężniejszych narzędzi do debugowania zależności.
- 🔐 **`snap run --shell <pakiet>`** – wejście do środowiska danego snapa.
- 🐍 **`python3 -m pip` zamiast `pip`** – zawsze wiesz, z którą wersją Pythona pracujesz.
- 📦 **`pip install .`** – instalacja pakietu z katalogu roboczego (np. własny projekt).
- 💾 **`git stash && git pull && git stash pop`** – bezpieczna aktualizacja kodu z zachowaniem lokalnych zmian.
- 🛑 **`git reset --hard HEAD`** – wraca do czystego stanu ostatniego commita. **Nieodwracalne!**

---
## APT
Dla distro opartych na Debianie. Zawiera rozszerzenie `.deb`. `dpkg` służy do instalacji programów z tym rozszerzoniem. APT ułatwia instalowanie wszystkich zależności gdy instalujesz program. Repo można sprawdzić w `/etc/apt/sources.list`
![[Pasted image 20250728152807.png]]
**APT używa lokalnego cache. To zapewnia informacje na temat zainstalowanych pakietów na naszym systemie nawet gdy jestesmy offline.
Przykład wszystkich pakietów `Impacket`
![[Pasted image 20250728152920.png]]
Możemy też sprawdzić dodatkowe informacje o pakiecie
![[Pasted image 20250728152944.png]]
Możemy sprawdzić wszystkie zainstalowane pakiety
![[Pasted image 20250728153101.png]]
Jeżeli jakichś nam brakuje możemy ich poszukać i zainstalować używając komendy:**
![[Pasted image 20250728153140.png]]

---
## Git
**Kopiowanie z gita i tworzenie folderu**
![[Pasted image 20250728153327.png]]

---
## DPKG
Można również pobierać programy i narzędzia z repozytorium odzielnie. Tutaj pobierajmy `strace` dla Ubuntu 18.04
`wget http://archive.ubuntu.com/ubuntu/pool/main/s/strace/strace_4.21-1ubuntu1_amd64.deb`
Dodatkowo można użyć obu apt i dpkg żeby zainstalować pakiety.
![[Pasted image 20250728153706.png]]
I teraz testujemy
![[Pasted image 20250728153720.png]]

---
## Zadanka
Search for "**evil-winrm**" tool on Github and install it on our interactive instances. Try all the different installation methods.
- GIT: `mkdir ~/evil-win/ && git clone https://github.com/Hackplayers/evil-winrm.git ~/evil-win`
- `sudo gem install evil-winrm` - Potrzeba Gem (RubyGems)
- `docker pull rastasheep/evil-winrm` - Docker



## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)