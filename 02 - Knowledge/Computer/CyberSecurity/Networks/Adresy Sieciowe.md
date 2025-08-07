---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - theme/default
  - status/in-progress
  - review/pending
date: 2025-08-07
last_updated: 2025-08-07
---

# Study: [[Network Main]]

## 🌐 Adresacja w Sieciach: MAC, IPv4 i Subnety

---

### 🔎 Zrozumienie adresacji: systemowa metafora

| Rodzaj adresu   | Co identyfikuje?                 | Systemowa analogia                  |
| --------------- | -------------------------------- | ----------------------------------- |
| **MAC Address** | Urządzenie w **lokalnej sieci**  | Numer mieszkania i piętra           |
| **IPv4/IPv6**   | Urządzenie **w sieci globalnej** | Adres pocztowy + dzielnica          |
| **Subnet**      | Część większej sieci IP          | Osiedle, ulica                      |
| **Broadcast**   | Wszystkie urządzenia w sieci     | Ogłoszenie megafonem na całej ulicy |

---

### 🧩 MAC Address (Media Access Control)

- Unikalny dla każdej **karty sieciowej**
    
- Stały (wypalony w urządzeniu), zapisany jako 6 bajtów (48 bitów), np.: `00:1A:2B:3C:4D:5E`
    
- Służy do komunikacji **w ramach jednej sieci lokalnej (LAN)**
    
- Przypisywanie dynamiczne IP do MAC odbywa się przez **ARP (Address Resolution Protocol)**
    

---

### 📬 IPv4 – Adresacja logiczna

#### Struktura IPv4:

- **32 bity**, podzielone na **4 oktety po 8 bitów**
    
- Przykład adresu:
    
    - Binarnie: `11000000.10101000.00001010.00100111`
        
    - Dziesiętnie: `192.168.10.39`
        

#### Każdy adres IPv4 zawiera:

- **Część sieciową (network)** – określa do jakiej sieci należy host
    
- **Część hosta (host)** – identyfikuje konkretne urządzenie w tej sieci
    

---

### 🧠 Klasy adresów IPv4 (klasyczny model)

| Klasa | Zakres adresów                  | Maska domyślna  | CIDR  | Hostów (użytecznych) |
| ----- | ------------------------------- | --------------- | ----- | -------------------- |
| A     | `1.0.0.0` – `126.255.255.255`   | `255.0.0.0`     | `/8`  | ~16 milionów         |
| B     | `128.0.0.0` – `191.255.255.255` | `255.255.0.0`   | `/16` | ~65 tysięcy          |
| C     | `192.0.0.0` – `223.255.255.255` | `255.255.255.0` | `/24` | 254                  |
| D     | `224.0.0.0` – `239.255.255.255` | Multicast       | —     | —                    |
| E     | `240.0.0.0` – `255.255.255.255` | Zarezerwowane   | —     | —                    |
📌 **Uwaga**: Klasy nie są już używane w nowoczesnym routingu. Zastąpione przez **CIDR**.

---

### 📐 Subnetting i CIDR

#### CIDR (Classless Inter-Domain Routing)

- Umożliwia podział adresacji na mniejsze sieci **niezależnie od klasy**
    
- Notacja: `IP/CIDR`, np.: `192.168.10.39/24`
    
- `/24` = 24 bity zajęte przez część sieciową → maska: `255.255.255.0`
    

#### Subnet mask:

| CIDR | Maska           | Ilość hostów |
| ---- | --------------- | ------------ |
| /8   | 255.0.0.0       | 16 777 214   |
| /16  | 255.255.0.0     | 65 534       |
| /24  | 255.255.255.0   | 254          |
| /30  | 255.255.255.252 | 2            |
📌 Dwa adresy są **zawsze zarezerwowane**:

- **Adres sieci (network)** – pierwszy w zakresie
    
- **Broadcast** – ostatni w zakresie

---

### 🌍 Adresy Specjalne

| Typ adresu          | Opis                                                        |
| ------------------- | ----------------------------------------------------------- |
| **Network address** | Identyfikuje daną podsieć – np. `192.168.10.0`              |
| **Broadcast**       | Wysyła dane do **wszystkich hostów** – np. `192.168.10.255` |
| **Default Gateway** | Adres routera – często `.1` lub `.254` w podsieci           |
| **Loopback**        | `127.0.0.1` – lokalny testowy adres własnego hosta          |

---

### 🧮 Konwersja: binarny ↔ dziesiętny

#### Przykład:
```
IP: 192.168.10.39
1st octet = 192 → 11000000
2nd = 168 → 10101000
3rd = 10  → 00001010
4th = 39  → 0010011
```

Maska `/24`:
```
Binary:    11111111.11111111.11111111.00000000
Decimal:   255     .255     .255     .0
```

---

### 🧠 Systemowe podejście: zależności i konteksty

|Element|Rola systemowa|
|---|---|
|**MAC**|Trwała tożsamość lokalna (hardware ID)|
|**IP**|Ruchomy identyfikator logiczny w sieci|
|**ARP**|Pośrednik między warstwą 2 i 3 OSI|
|**CIDR/subnet**|Segmentacja logiczna, kontrola przepływu|
|**Broadcast**|Narzędzie masowej komunikacji|
|**Gateway**|Węzeł pośredniczący między podsieciami|

---

### 🔐 Znaczenie dla pentestera

|Kontekst|Znaczenie praktyczne|
|---|---|
|**Enumeracja sieci**|Znalezienie zakresu IP w danej podsieci (np. `192.168.1.0/24`)|
|**ARP Spoofing**|Fałszowanie odpowiedzi ARP w celu podsłuchu|
|**Scanowanie IP**|Nmap, Masscan – znajdowanie hostów w podsieci|
|**Eksploatacja broadcastu**|Wysyłanie exploitów do wielu maszyn równocześnie|
|**Maskowanie**|Ustawienie IP/maski by ukryć aktywność w ruchu sieciowym|

---

### 📁 Przykład notacji CIDR i konfiguracji

```
# Konfiguracja interfejsu sieciowego
interface: eth0
ip_address: 192.168.10.39
subnet: 255.255.255.0
cidr: /24
gateway: 192.168.10.1
```

### 🧭 Podsumowanie

- MAC = **adres sprzętowy**, IP = **adres logiczny**
    
- Adres IP składa się z części **sieciowej** i **hosta**
    
- **CIDR** umożliwia elastyczne dzielenie adresacji
    
- Broadcast i gateway mają **specjalne role** w komunikacji
    
- Zrozumienie binarnej reprezentacji IP = **klucz do subnetingu**
    
- Dla pentestera znajomość adresacji = **podstawa skutecznej enumeracji i pivotingu**
## References
- [Reference 1](link)
- [Reference 2](link)