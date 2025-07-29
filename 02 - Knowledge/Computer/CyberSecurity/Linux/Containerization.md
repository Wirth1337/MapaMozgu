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

# Study: [[Linux Main|Linux]]

## 🧱**Containerization** 

to technika uruchamiania aplikacji w _izolowanych, lekkich środowiskach_, zwanych **kontenerami**, które współdzielą kernel systemu hosta. Umożliwia to spójne działanie aplikacji niezależnie od środowiska, w którym są uruchamiane (dev/test/prod).

Kontenery:

- Są **lżejsze** niż VM — nie wymagają pełnego systemu operacyjnego.
    
- Zapewniają **izolację** między aplikacjami.
    
- Ułatwiają **skalowanie, wdrażanie i zarządzanie** aplikacjami.
    

> 🧠 **Metafora:** Kontener to jak „scena koncertowa w pudełku” – każda kapela ma swoje światła, instrumenty i głośniki, ale wszystkie mogą grać na tej samej scenie (kernelu) bez wchodzenia sobie w drogę.

---

# 🐳 Docker — kontenery dla ludzi
## 🔧 Instalacja Docker Engine (Ubuntu)
```

#!/bin/bash
sudo apt update -y
sudo apt install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo usermod -aG docker $USER
echo '[!] Wyloguj się i zaloguj ponownie, by aktywować grupę docker.'
docker run hello-world
```

📌 Docker jest **stateless** – dane tracisz po zatrzymaniu kontenera. Używaj **woluminów** lub **Dockerfile**, by utrwalić zmiany.

---

## 📦 Dockerfile – tworzenie własnego obrazu
```
FROM ubuntu:22.04

RUN apt-get update && apt-get install -y apache2 openssh-server && \
    rm -rf /var/lib/apt/lists/*

RUN useradd -m docker-user && echo "docker-user:password" | chpasswd && \
    chown -R docker-user:docker-user /var/www/html /var/run/apache2 /var/log/apache2 /var/lock/apache2 && \
    usermod -aG sudo docker-user && echo "docker-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

EXPOSE 22 80
CMD service ssh start && apache2ctl -D FOREGROUND

```

---
## 🛠️ Komendy Docker

```
# Budowanie obrazu
docker build -t my_image .

# Uruchomienie kontenera
docker run -p 8022:22 -p 8080:80 -d my_image

# Zarządzanie
docker ps               # Lista działających kontenerów
docker stop <id/name>   # Zatrzymaj kontener
docker start <id/name>  # Start kontenera
docker restart <id/name>
docker rm <id/name>     # Usuń kontener
docker rmi <image>      # Usuń obraz
docker logs <id/name>   # Logi kontenera

```

---

## 📦 LXC — kontenery low-level (systemowe)

### ✍️ Instalacja
`sudo apt-get install lxc lxc-utils -y
`
## ➕ Tworzenie i zarządzanie kontenerem
```
# Utwórz nowy kontener
sudo lxc-create -n linuxcontainer -t ubuntu

# Start/stop
sudo lxc-start -n linuxcontainer
sudo lxc-stop -n linuxcontainer
sudo lxc-attach -n linuxcontainer  # Wejście do środka

```


📘 [[Linux - Containers|LXC - Basic CLI Reference]]
```
# 🧱 LXC - Basic Commands

## 📦 lxc-core commands (now part of `lxc` package)

- `lxc-ls`  
  ➤ List all existing containers.

- `lxc-start -n <container>`  
  ➤ Start a stopped container.

- `lxc-stop -n <container>`  
  ➤ Stop a running container.

- `lxc-restart -n <container>`  
  ➤ Restart a container (shortcut for stop + start).

- `lxc-attach -n <container>`  
  ➤ Attach to container’s shell (like `docker exec -it`).

- `lxc-attach -n <container> -- /bin/bash`  
  ➤ Attach with specific shell.

- `lxc-attach -n <container> -f /path/to/share`  
  ➤ Attach and share a specific directory or file (⛔ często błędna składnia — `-f` nie służy do bind-mounta! → zamiast tego użyj wpisu w `config` lub `lxc.mount.entry`).

---

## ⚙️ lxc-config

- `lxc-config -n <container> -s storage`  
  ➤ Manage container storage settings.

- `lxc-config -n <container> -s network`  
  ➤ Configure networking (bridge, macvlan, NAT).

- `lxc-config -n <container> -s security`  
  ➤ Adjust apparmor/seccomp profiles.

> 🧠 *Uwaga:* `lxc-config` to **rzadziej używany wrapper**, większość konfiguracji obecnie odbywa się przez plik `config` w `/var/lib/lxc/<container>/config`.

---

## 📁 Gdzie trzyma dane LXC:
- kontenery: `/var/lib/lxc/<name>/`
- konfiguracja: `config` wewnątrz katalogu kontenera
- logi: `/var/log/lxc/<name>.log`

---

## 🧠 Porównania (dla myślących abstrakcyjnie)
- `lxc-start` ~ `systemctl start <unit>`  
- `lxc-attach` ~ `ssh localhost` bez SSH  
- `lxc-config` ~ `sysctl` dla kontenera  
- `lxc` = docker prequel; działa na poziomie *systemowym*, nie *demonowym*


```
---

## ⚙️ Ograniczenia zasobów (CPU, RAM)

`sudo vim /usr/share/lxc/config/linuxcontainer.conf
# Konfiguracja sieci w systemie Linux

Konfiguracja sieci w systemach Linux to kluczowa umiejętność – zwłaszcza dla pentesterów – umożliwiająca przygotowanie środowisk testowych, manipulację ruchem i analizę potencjalnych luk w zabezpieczeniach. Ważne jest zrozumienie sposobu zarządzania interfejsami sieciowymi (np. przewodowymi i bezprzewodowymi) oraz protokołów sieciowych (TCP/IP, DNS, DHCP, FTP itp.). Znajomość tych mechanizmów pozwala na precyzyjne dostosowywanie konfiguracji sieciowej w celu zoptymalizowania testów i wyników.

---

## Konfigurowanie interfejsów sieciowych

W nowoczesnych dystrybucjach Linux (np. Ubuntu, Debian) do zarządzania interfejsami sieciowymi zamiast przestarzałych narzędzi `ifconfig` i `route` stosuje się pakiet iproute2. Polecenia te są szybsze i bardziej przejrzyste[askubuntu.com](https://askubuntu.com/questions/1025568/has-netstat-been-replaced-with-a-new-tool#:~:text=52)[redhat.com](https://www.redhat.com/en/blog/route-ip-route#:~:text=%5Broot%40rhel%20~%5D,1). Przykładowe komendy:

Wyświetlanie interfejsów i adresów:
`lxc.cgroup.cpu.shares = 512
`lxc.cgroup.memory.limit_in_bytes = 512M

`sudo systemctl restart lxc.service
`
`cpu.shares` = udział względny (512 oznacza 50% przy domyślnym 1024), `memory.limit_in_bytes` = twardy limit RAM.

---
## 🛡️ Bezpieczeństwo kontenerów
- Izolacja przez **namespaces** (PID, net, mnt, etc.)
    
- **cgroups** – limity zasobów
    
- **AppArmor/SELinux** (Docker)
    
- Używaj tylko zaufanych obrazów
    
- W LXC – sam skonfiguruj zabezpieczenia (ACL, sieć, dostęp)
    

> 🚨 Obie technologie mogą być podatne na _container escape_ lub _privilege escalation_, jeśli źle skonfigurowane.

---

## ⚔️ LXC vs Docker

|Cecha|Docker|LXC|
|---|---|---|
|Poziom|Aplikacyjny|Systemowy|
|Portowalność|Wysoka (Docker Hub, CI/CD)|Niska (zależny od hosta)|
|Izolacja|Domyślnie silna (AppArmor, rootless)|Wymaga ręcznej konfiguracji|
|Użyteczność|Prosty CLI, duża społeczność|Bardziej złożony, mniej user-friendly|
|Obrazowanie|Dockerfile + cache|Ręczne rootfs / szablony|

## Review Questions
- Zainstaluj LXC i utwórz pierwszy kontener
	    `sudo apt install lxc` - instalacja
	    
- Skonfiguruj sieć w kontenerze
	    jako root `cd /var/lib/lxc/linuxcontainer`
	    tam znajdujemy config i edytujemy sekcję:
	    
	    lxc.net.0.type = veth
		lxc.net.0.link = lxcbr0
		lxc.net.0.flags = up
		lxc.net.0.hwaddr = 00:16:3e:xx:xx:xx

- Stwórz własny rootfs i uruchom z niego kontener
	- `mkdir -p ~/lxc_rootfs/debian-minimal`
		- sudo apt install debootstrap
		- sudo debootstrap stable ~/lxc_rootfs/debian-minimal http://deb.debian.org/debian
		  sudo mkdir -p /var/lib/lxc/deb1/rootfs
		- sudo cp -a ~/lxc_rootfs/debian-minimal/. /var/lib/lxc/deb1/rootfs/
		- sudo nano /var/lib/lxc/deb1/config
		- lxc.include = /usr/share/lxc/config/common.conf
		- lxc.arch = linux64
		- lxc.rootfs.path = dir:/var/lib/lxc/deb1/rootfs
		- lxc.uts.name = deb1
		- lxc.net.0.type = veth
		- lxc.net.0.link = lxcbr0
		- lxc.net.0.flags = up
		- lxc.net.0.hwaddr = 00:16:3e:xx:xx:xx
    
- Nałóż limity CPU/RAM
    
- Przećwicz `lxc-*` komendy
    
- Uruchom serwer WWW w kontenerze
    
- Skonfiguruj dostęp przez SSH
    
- Zadbaj o persystencję danych
    
- Użyj kontenera do testowania exploita lub malware

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)