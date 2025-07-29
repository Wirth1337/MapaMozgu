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

# [[Linux Main|Linux]]

# Zapory sieciowe w Linux (Firewall)

Zapory sieciowe (firewalle) kontrolują i monitorują ruch między segmentami sieci (wewnętrznym, zewnętrznym, DMZ itp.), chroniąc przed nieautoryzowanym dostępem, atakami DDoS, skanami portów i innymi zagrożeniami. W Linuxie podstawą jest framework **Netfilter**, a narzędziami konfiguracyjnymi – historyczne **iptables** oraz nowocześniejsze **nftables**, **firewalld** i **ufw**.

---

## 1. Netfilter i iptables

- **Netfilter** – moduły jądra Linux, które oferują haki do przechwytywania i przetwarzania pakietów.
- **iptables** – użytkowe narzędzie CLI do definiowania reguł Netfilter.
- **Historia**: ipfwadm → ipchains → iptables (od kernela 2.4, 2000 r.) → **nftables** (kernel 3.13, 2014 r.)  

### 1.1 Instalacja iptables  
```bash
# Ubuntu/Debian
sudo apt update && sudo apt install iptables -y

# RHEL/CentOS
sudo yum install iptables-services -y
sudo systemctl enable --now iptables
```

---
## 2. Tabele (Tables)

Tabele grupują różne rodzaje przetwarzania pakietów:

| Nazwa    | Przeznaczenie                                | Łańcuchy wbudowane                              |
| -------- | -------------------------------------------- | ----------------------------------------------- |
| filter   | Filtrowanie ruchu (blokowanie/zezwalanie)    | INPUT, FORWARD, OUTPUT                          |
| nat      | Modyfikacja adresów (SNAT, DNAT, MASQUERADE) | PREROUTING, POSTROUTING, OUTPUT                 |
| mangle   | Modyfikacja nagłówków (markowanie, TTL, TOS) | PREROUTING, INPUT, FORWARD, OUTPUT, POSTROUTING |
| raw      | Wykluczanie pakietów z connection tracking   | PREROUTING, OUTPUT                              |
| security | (SELinux) dodatkowe reguły bezpieczeństwa    | INPUT, FORWARD, OUTPUT                          |

---

## 3. Łańcuchy (Chains)

- **Wbudowane** – istnieją w każdej tabeli, obsługują podstawowy przepływ pakietów.
    
- **Użytkownika** – definiowane przez administratora dla uproszczenia i ponownego wykorzystania reguł.
    

### 3.1 Wbudowane przykłady

- **filter**:
    
    - `INPUT` – pakiety przychodzące do lokalnego hosta
        
    - `FORWARD` – pakiety przechodzące przez host (routing)
        
    - `OUTPUT` – pakiety wychodzące z lokalnego hosta
        
- **nat**:
    
    - `PREROUTING` – zmiana docelowego adresu przed routingiem
        
    - `POSTROUTING` – zmiana źródłowego adresu po routingiem
        

---

## 4. Reguły i cele (Rules & Targets)

Reguła składa się z **match** (kryteriów) oraz **target** (działania).

### 4.1 Przykład dodania reguły

Zezwól na przychodzący ruch SSH (TCP/22) do tabeli `filter`, łańcuchu `INPUT`:
`sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT`

### 4.2 Popularne cele (targets)

- `ACCEPT` – przepuść pakiet
    
- `DROP` – porzuć pakiet (bez powiadomienia)
    
- `REJECT` – porzuć i odeslij ICMP “port unreachable”
    
- `LOG` – zaloguj pakiet do syslog
    
- `SNAT` / `MASQUERADE` – zmień źródłowy adres (NAT wyjściowy)
    
- `DNAT` / `REDIRECT` – zmień docelowy adres/port (NAT przychodzący)
    
- `MARK` – oznacz pakiet (dla zaawansowanego routingu)
    

---

## 5. Matches (dopasowania)

Kryteria określają, które pakiety pasują do reguły:

|Opcja|Opis|
|---|---|
|`-p`|protokół (tcp, udp, icmp)|
|`-s` / `--source`|adres źródłowy|
|`-d` / `--destination`|adres docelowy|
|`--sport`|port źródłowy|
|`--dport`|port docelowy|
|`-m state --state`|stan połączenia (NEW, ESTABLISHED)|
|`-m multiport`|wiele portów lub zakres portów|
|`-m conntrack`|dopasowanie na podstawie śledzenia połączeń|
|`-m limit`|ograniczenie szybkości matchowania|
|`-m mac`|adres MAC|

**Przykład**: Zezwól HTTP i HTTPS:
`sudo iptables -A INPUT -p tcp -m multiport --dports 80,443 -j ACCEPT`

---

## 6. Nowoczesne alternatywy

### 6.1 nftables

- Bardziej wydajne, zunifikowane reguły
    
- Składnia inna niż iptables, ale migracja przy pomocy `iptables-translate`
```
sudo apt install nftables
sudo nft add table inet filter
sudo nft 'add chain inet filter input { type filter hook input priority 0; policy drop; }'
sudo nft 'add rule inet filter input ct state established,related accept'
sudo nft 'add rule inet filter input proto tcp tcp dport ssh accept'
```
### 6.2 firewalld

- Nakładka na nftables/iptables z koncepcją stref (zones) i serwisów
    
- Dynamiczne przeładowanie bez przerywania ruchu
```
sudo dnf install firewalld
sudo systemctl enable --now firewalld
sudo firewall-cmd --permanent --zone=public --add-service=ssh
sudo firewall-cmd --reload
```

### 6.3 UFW (Uncomplicated Firewall)

- Prosty frontend do iptables
```
sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw enable
```

## 7. Przykładowe zadania praktyczne

1. Uruchom serwer WWW na porcie 8080 i zablokuj do niego ruch:
2. Zmień reguły, aby zezwalać na ruch do portu 8080.
3. Zablokuj ruch z wybranego adresu IP:





















## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)