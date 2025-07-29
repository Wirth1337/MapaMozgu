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

# [[Linux Main|Study - {{Linux}}]] 

## Objective
System plików w Linuxie to sposób organizowania i przechowywania danych na dyskach. Linux obsługuje różne typy systemów plików, z których każdy ma swoje zastosowanie

---
## Content
### 📁 Typy systemów plików:

- **ext2** – starszy system, brak journalingu (czyli mechanizmu „odzyskiwania” po awarii), mało używany, ale dobry np. na pendrive’y.
    
- **ext3/ext4** – z journalingiem, stabilne, szybkie. ext4 to domyślny wybór w większości nowoczesnych dystrybucji.
    
- **Btrfs** – zaawansowany system z funkcjami snapshotów i kontroli integralności danych.
    
- **XFS** – świetny do dużych plików i środowisk z dużym ruchem dyskowym.
    
- **NTFS** – system Windows, przydatny przy współdzieleniu danych między Linuxem a Windowsem.
    

### 🔎 Jak wybrać?

Wybór zależy od:

- Wydajności
    
- Integralności danych
    
- Kompatybilności
    
- Potrzeb aplikacji

---
## 🗂 Struktura systemu plików

- **Inody** – jak kartoteka w bibliotece – przechowują dane o pliku: prawa dostępu, właściciela, rozmiar, daty itp. Nie zawierają nazwy pliku ani treści – tylko „gdzie on leży”.
    
- **Tablica inodów** – katalog kart bibliotecznych dla całego systemu.
    

### 📄 Typy plików:

- **Pliki zwykłe** – tekst, programy, multimedia itd.
    
- **Katalogi** – foldery zawierające inne pliki/katalogi.
    
- **Linki symboliczne** – skróty do plików/katalogów w innych lokalizacjach.

---
## 🔐 Uprawnienia i użytkownicy:

Linux rozróżnia uprawnienia dla:

- Właściciela
    
- Grupy
    
- Innych
    

Przykład: `-rw-r--r-- 1 user grupa 1234 sty 1 12:34 plik.txt`

## 💿 Zarządzanie dyskami

Do pracy z partycjami: `sudo fdisk -l`

Przykład: `Device     Boot Start       End   Sectors Size Id Type
`/dev/vda1  *    2048 158`
`

---

# 📌 Montowanie systemów plików (ang. _mounting_)

### 🔗 Co to jest montowanie?

Montowanie to proces przypisania fizycznego lub logicznego dysku (np. partycji, pendrive’a) do konkretnego katalogu w strukturze systemu plików. Dzięki temu system widzi zawartość tego nośnika w określonym miejscu — tzw. **punkt montowania** (_mount point_), np. `/mnt/usb`.

To trochę jak przypięcie zewnętrznego magazynu do półki w bibliotece — po przypięciu możesz normalnie z niego korzystać, tak jak z innych półek.

---
### 🔧 Komendy montowania:

**Ręczne montowanie:**
`sudo mount /dev/sdb1 /mnt/usb`

**Sprawdzanie zamontowanych systemów:**
`mount`

**Automatyczne montowanie przy starcie (plik `/etc/fstab`):**
`cat /etc/fstab
`
**Przykładowy wpis:**
`UUID=xxxxx /mnt/usb ext4 rw,noauto,user 0 0`

- `noauto` – nie montuj automatycznie
- `user` – pozwala zwykłemu użytkownikowi na montowanie

---

## 📤 Odmontowywanie:

Aby bezpiecznie odpiąć nośnik:
`sudo umount /mnt/usb`
Jeśli nośnik jest w użyciu, trzeba zakończyć procesy:
`lsof | grep /mnt/usb`

---

## 🧠 **SWAP – pamięć wirtualna**
### 📌 Co to jest?

SWAP to przestrzeń na dysku używana jako zapasowa pamięć RAM. Gdy systemowi brakuje RAM-u, dane rzadziej używane trafiają do SWAP, co pozwala działać aplikacjom bez zawieszania systemu.

### 🔧 Komendy:
Tworzenie przestrzeni SWAP:
`sudo mkswap /dev/sdX
`
**Aktywacja SWAP:**
`sudo swapon /dev/sdX`

**Podgląd aktywnego SWAP:**
`free -h
`
### 💡 SWAP a hibernacja:

Podczas hibernacji cały stan systemu (RAM) jest zapisywany do przestrzeni SWAP, aby można było wrócić do działania po uruchomieniu systemu.

|Element|Opis|
|---|---|
|`mount`|Ręczne montowanie dysku/partycji|
|`/etc/fstab`|Plik konfiguracji automatycznego montowania|
|`umount`|Odmontowanie dysku|
|`lsof`|Sprawdzanie otwartych plików (czy system plików jest używany)|
|`mkswap`|Tworzenie przestrzeni SWAP|
|`swapon`|Aktywacja przestrzeni SWAP|
|`free -h`|Podgląd użycia pamięci i SWAP|

## Review Questions
- How many partitions exist in our Pwnbox? (Format: 0)
	- `sudo fdisk -l`
	- ![[Pasted image 20250729210341.png]]


## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)