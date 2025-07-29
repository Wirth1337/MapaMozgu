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

# Zabezpieczanie systemu Linux

Wszystkie systemy komputerowe niosą ryzyko włamania. Serwery internetowe z exposowanymi aplikacjami są szczególnie narażone, zaś Linux – choć mniej podatny na wirusy Windows – również wymaga solidnych podstaw zabezpieczeń.

---

## 1. Aktualizacje i zarządzanie pakietami

- **Regularne aktualizacje** jądra i pakietów to fundament bezpieczeństwa:  
  ```
  sudo apt update && sudo apt full-upgrade -y
  # lub w RHEL/CentOS:
  sudo yum update -y
	```

**Automatyczne poprawki** (Unattended Upgrades):
```
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```
**Snapshoty VM** przed dużymi aktualizacjami – szybki rollback w razie awarii.

---
## 2. Zapora sieciowa (Firewall)

### 2.1. Nowoczesne narzędzia

- **nftables** – następca iptables.
    
- **firewalld** (z `nftables`) / **ufw** – prostsza warstwa zarządzania.
    

### 2.2. Przykład konfiguracji ufw

```
sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow http
sudo ufw enable
```

2.3. Przykład konfiguracji nftables
```
sudo apt install nftables
cat <<EOF | sudo tee /etc/nftables.conf
#!/usr/sbin/nft -f
table inet filter {
  chain input {
    type filter hook input priority 0;
    policy drop;
    ct state established,related accept
    iif "lo" accept
    tcp dport { ssh, http, https } ct state new accept
  }
  chain forward { policy drop }
  chain output { policy accept }
}
EOF
sudo systemctl enable --now nftables
```

---
## 3. Bezpieczny SSH

### 3.1. Wyłączanie haseł i logowania root

Edytuj `/etc/ssh/sshd_config`:
```
PasswordAuthentication no
PermitRootLogin no
ChallengeResponseAuthentication no
UsePAM yes
```
`sudo systemctl restart sshd`

### 3.2. Klucze SSH

- Generuj parę kluczy: `ssh-keygen -t ed25519`
    
- Autoryzuj: `ssh-copy-id user@host`

### 3.3. Fail2Ban
```
sudo apt install fail2ban
cat <<EOF | sudo tee /etc/fail2ban/jail.local
[sshd]
enabled = true
maxretry = 5
bantime = 3600
EOF
sudo systemctl enable --now fail2ban
```

---
## 4. Zasada najmniejszych uprawnień

- **Unikaj** stałego logowania jako root.
    
- **Sudoers** – pozwól na pojedyncze komendy:
```
sudo visudo
#dodaj linijkę:
janek ALL=(root) NOPASSWD: /usr/bin/systemctl restart apache2
```
- **Grupy** – organizuj dostęp według grup, nie indywidualnie.

---

## 5. Audyt systemu

- **Lynis** – skaner bezpieczeństwa:
`sudo apt install lynis
`sudo lynis audit system
- **rkhunter** / **chkrootkit** – wykrywanie rootkitów:
```
sudo apt install rkhunter chkrootkit
sudo rkhunter --check
sudo chkrootkit
```
- **auditd** – monitorowanie zdarzeń jądra:
```
sudo apt install auditd
sudo systemctl enable --now auditd
```
- **Sprawdzanie SUID/SGID**:
	`find / -perm /6000 -type f -exec ls -l {} \;`
- **Pliki world-writable**:
	`find / -xdev -type f -perm -0002 -print`

---

## 6. MAC: SELinux i AppArmor

### 6.1. SELinux

- Domyślnie w RHEL/Fedora/CentOS.
    
- Tryby: `enforcing`, `permissive`, `disabled`.
    
- Podstawowe komendy:
```
sudo getenforce
sudo setenforce enforcing
sudo ausearch -m avc -ts recent
```

- Tworzenie policy modułu:
```
sudo yum install selinux-policy-devel
sudo audit2allow -w -a
sudo audit2allow -a -M mypol
sudo semodule -i mypol.pp
```
### 6.2. AppArmor

- Domyślnie w Ubuntu.
    
- Profilowanie:
```
sudo apt install apparmor-utils
sudo aa-status
sudo aa-enforce /etc/apparmor.d/usr.bin.firefox
```

- Tworzenie profilu:
	`sudo aa-genprof /usr/bin/myapp`

---

## 7. Usuwanie zbędnych usług i twarde ustawienia

- **Wyłącz/usuwaj** niepotrzebne deamony:
	`sudo systemctl disable --now postfix`
- **SUID/SGID** – usuń bit, jeśli nie jest konieczny:
	`sudo chmod u-s /usr/bin/passwd1`
- **NTP / Chrony** – synchronizacja czasu:
	`sudo apt install chrony`
	`sudo systemctl enable --now chrony`
- **Syslog** / **rsyslog** – zbieranie logów:
	`sudo apt install rsyslog
	`sudo systemctl enable --now rsyslog
- **Silne hasła** i **password aging**:
```
sudo apt install libpam-pwquality
# edytuj /etc/pam.d/common-password
password requisite pam_pwquality.so retry=3 minlen=12
# edytuj /etc/login.defs
PASS_MAX_DAYS   90
PASS_MIN_DAYS   1
PASS_WARN_AGE   14
```

---
## 8. Dodatkowe narzędzia bezpieczeństwa

- **Snort** / **Suricata** – IDS/IPS.
    
- **OSSEC** – HIDS + SIEM.
    
- **Tripwire/AIDE** – wykrywanie zmian w plikach.
    
- **ClamAV** – antywirus (skanowanie plików).
    

---

## 9. TCP Wrappers (przestarzałe)

**TCP Wrappers** (`/etc/hosts.allow`, `/etc/hosts.deny`) kontrolują dostęp do usług po nazwie programu, nie na poziomie portu.
```
# /etc/hosts.allow
sshd: 10.0.0.0/24
# /etc/hosts.deny
ALL: ALL
```
**Uwaga:** TCP Wrappers nie zastąpią firewalla. Zalecamy używać `nftables`/`ufw` zamiast TCP Wrappers dla pełnej kontroli ruchu.


---

## 10. Proces, nie produkt

Bezpieczeństwo to ciągły proces:

1. **Monitoruj** (logi, IDS/IPS).
    
2. **Audytuj** (regularne skany Lynis, rkhunter).
    
3. **Aktualizuj** (OS, pakiety, kernel).
    
4. **Utrzymuj** polityki MAC (SELinux/AppArmor).
    
5. **Edukacja** – administratorzy muszą znać system “od środka”.

## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)