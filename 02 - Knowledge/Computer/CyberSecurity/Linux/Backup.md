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

## Ogólnie
Backup to backup większej filozofii nie ma backapujesz coś jakby się miało rozjebać

---

## Narzędzia
### ### **Rsync**

- Tworzy szybkie, przyrostowe kopie (przesyła tylko zmienione dane)
    
- Działa lokalnie i zdalnie (np. przez `ssh`)
    
```
- Przykłady:
  # Instalacja
sudo apt install rsync -y

# Pełna kopia zapasowa na zdalny serwer
rsync -av /ścieżka/źródło user@backup:/ścieżka/docelowa

# Kompresja, backup przyrostowy, katalog zmian, synchronizacja usuniętych plików
rsync -avz \
  --backup \
  --backup-dir=/ścieżka/przyrosty \
  --delete \
  /ścieżka/źródło \
  user@backup:/ścieżka/docelowa

# Przywracanie z backupu zdalnego
rsync -av user@backup:/ścieżka/docelowa /ścieżka/źródło
  
```
### **Duplicity**

- Rozszerza `rsync` o szyfrowanie GPG
    
- Obsługuje zdalne lokalizacje (FTP, SFTP, Amazon S3 itp.)
    
- Zapisuje przyrostowe snapshoty (czyli historię zmian)
### **Deja Dup**

- Graficzny interfejs do `duplicity`
    
- Umożliwia automatyczne, szyfrowane kopie
    
- Idealne do prostych backupów na desktopie
  
## 🧠 Wskazówki i Koncepcje

- **Dane = skarb**, narzędzia backupu = sejf
    
    - `rsync` = szybki kurier (kopiuje tylko różnice)
        
    - `duplicity` = sejf z zamkiem GPG
        
    - `deja-dup` = wygodny sejf z przyciskiem (GUI)
        
- **Zawsze szyfruj backupy**:
    
    - `rsync` + `ssh` = bezpieczne połączenie
        
    - `duplicity` = automatyczne szyfrowanie GPG
        
    - Możesz też używać np. `eCryptfs`, `LUKS`, VeraCrypt

---
## Automatyzacja (cron)
### 1. Klucze SSH
`ssh-keygen`
`ssh-copy-id user@backup`
## 2. Skrypt backupu (`backup.sh`)
`#!/bin/bash`
`rsync -avz -e ssh /ścieżka/źródło user@backup:/ścieżka/docelowa`
`chmod +x backup.sh`
## 3.Cron (co godzinę)
`crontab -e`
`0 * * * * /ścieżka/backup.sh
`
`mkdir ~/do_backupu ~/kopia_zapasowa`
`rsync -av ~/do_backupu/ ~/kopia_zapasowa/`


## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)