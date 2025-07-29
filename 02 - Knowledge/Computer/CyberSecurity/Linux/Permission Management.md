---
aliases:
  - "[[Linux Main|Study - {{Linux}}]]"
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

## Objective
Permission to klucze które kontrolują dostęp do plików i folderów
Można je przydzielać
- Użytkownikowi
- Grupom 
Każdy plik i i folder ma właściciela i jest przypisany do grupy. 
## **Te Uprawnienia obejmują
- Odczyt (r)
- Pisanie (w)
- Wykonanie (x)
**Ważne!! - uprawnienia execute są potrzebne do dostępu do katalogu
![[Pasted image 20250728141151.png]]

---
## Zawartość
### Zmiana Uprawnień
Możemy zmieniać uprawnienia używajac `chmod` dla referencji grup 
- `u` - Owner
- `g` - Group 
- `o` - Others
- `a` - All Users
Za pomocą operatorów
- `+` - add permisson
- `-` - delate permisson
Przykładowy chmod
	chmod +x plik.sh - Dodaje prawa do wykonania
	chmod u=rwx,g=rx,o=r  plik.txt - Precyzyjne ustawienie dla każdego
	chmod 755 folder/ - Oktalnie: pełne dla właściciela, odczyt/wykonanie dla reszty

---

## Umask – domyślne uprawnienia plików i katalogów

`umask` (user file-creation mode mask) **określa domyślne uprawnienia** dla nowo tworzonych plików i katalogów.

### 🛠️ Jak działa?

- Linux **najpierw przyznaje maksymalne uprawnienia**:
    
    - `666` dla plików (czyli `rw-rw-rw-`)
        
    - `777` dla katalogów (czyli `rwxrwxrwx`)
        
- Następnie **odejmuje maskę `umask`**.
    

### 📊 Przykładowa tabela

|umask|Pliki (`666 - umask`)|Foldery (`777 - umask`)|
|---|---|---|
|`022`|`644` → `rw-r--r--`|`755` → `rwxr-xr-x`|
|`002`|`664` → `rw-rw-r--`|`775` → `rwxrwxr-x`|
|`027`|`640` → `rw-r-----`|`750` → `rwxr-x---`|
|`077`|`600` → `rw-------`|`700` → `rwx------`|

---

### 📌 Jak sprawdzić?
`umask`
### 🧑‍🏫 Jak ustawić tymczasowo?

`umask 027`

### 🛠️ Stała zmiana (np. dla bash):

Dodaj do pliku `~/.bashrc` lub `~/.profile`:

`umask 027`

---

## 💡 Podsumowanie

- `umask` **nie ustawia** uprawnień – **usuwa je** z maksymalnych wartości.
    
- Domyślnie nie pozwala plikom mieć uprawnienia `x`, ponieważ pliki zwykle nie są wykonywalne.
    
- Ma **kluczowe znaczenie dla bezpieczeństwa** systemu i kontroli dostępu.

---
## Directory Permissions

| Uprawnienie | Symbol | Znaczenie                                                                   |
| ----------- | ------ | --------------------------------------------------------------------------- |
| read        | `r`    | Pozwala **zobaczyć listę plików** w katalogu (`ls`)                         |
| write       | `w`    | Pozwala **tworzyć, usuwać i zmieniać nazwy plików** w katalogu              |
| execute     | `x`    | Pozwala **wejść do katalogu** (`cd`) oraz uzyskać dostęp do jego zawartości |
## Przykładowe przypadki

|Uprawnienia katalogu|Można wejść (`cd`)|Można listować (`ls`)|Można tworzyć/usunąć|
|---|---|---|---|
|`--x`|✅ TAK|❌ NIE|❌ NIE|
|`r--`|❌ NIE|❌ NIE|❌ NIE|
|`r-x`|✅ TAK|✅ TAK|❌ NIE|
|`rw-`|❌ NIE|❌ NIE|❌ NIE|
|`rwx`|✅ TAK|✅ TAK|✅ TAK|

---

## 🧠 Kluczowe zasady

- 🔑 **Bez `x` – nie wejdziesz do katalogu** (`cd folder/` → Permission denied)
    
- 🔎 **Bez `r` – nawet mając `x`, nie zobaczysz listy plików (`ls`)**
    
- ✏️ **Bez `w` – nie stworzysz, nie usuniesz ani nie zmienisz nazwy pliku/katalogu**
### Dodanie Uprawnień do odczytu dla wszystkich Userów
![[Pasted image 20250728141531.png]]
### Można też to robić Oktalnie
|Binary Notation|Binary Representation|Octal Value|Permission Representation|Znaczenie|
|---|---|---|---|---|
|4 2 1|1 1 1|`7`|`rwx`|odczyt, zapis i wykonanie|
|4 2 1|1 1 0|`6`|`rw-`|odczyt i zapis|
|4 2 1|1 0 1|`5`|`r-x`|odczyt i wykonanie|
|4 2 1|1 0 0|`4`|`r--`|tylko odczyt|
|4 2 1|0 1 1|`3`|`-wx`|zapis i wykonanie|
|4 2 1|0 1 0|`2`|`-w-`|tylko zapis|
|4 2 1|0 0 1|`1`|`--x`|tylko wykonanie|
|4 2 1|0 0 0|`0`|`---`|brak uprawnień|
## Dla przypomnienia

- **Kolejność uprawnień**: `Owner | Group | Others`
    
- Przykład `chmod 754` oznacza:
    
    - Owner: `7` → `rwx`
        
    - Group: `5` → `r-x`
        
    - Others: `4` → `r--`
        
- Czyli: właściciel ma pełne prawa, grupa tylko odczyt i wykonanie, reszta tylko odczyt
## Szybki sposób liczenia

|Litera|Bit|Wartość|
|---|---|---|
|`r`|1|4|
|`w`|1|2|
|`x`|1|1|

Dodajesz wartości dla każdego zestawu (`rwx`, `rw-`, itp.) → to daje Ci wartość oktalną (`chmod`).
### Zmiana Właściciela
Do zmiany właściciela plików lub folderów (grup) używamy komendy
`chown`
### Syntax
`chown <user>:<group> <file/directory>`

---

## SUID & SGID
Po za standardowymi uprawnieniami mamy również specialne uprawnienia
- **Set User Id (SUID)** 
- **Set Group ID (SGID)**
Dzięki nim można dodać uprawnienia wyszczególnionemu użytkownikowi bądź grupie. Te uprawnienia oznacza się za pomocą `s` w miejscu `x` w zasadach dostępu pliku. Wtedy Program uruchamia się z uprawnieniami właściciela.
**Uwaga Security Risk!!**
Przykład: https://gtfobins.github.io/gtfobins/journalctl/#sudo

## Sticky Bit
Wyobraź sobie warsztat gdzie każdy może wejść i korzystać z tych samych narzędzi ale każda osoba ma swoją własną szafkę którą tylko oni (lub szef) mogą otworzyć. Sticky Bits działają jak zamek na tych szafkach. W współdzielonych folderach oznacza to że tylko właściciel albo root user mogą usunąć lub zmienić nazwę pliku. Reszta użytkowników dalej może zajrzeć do folderu ale nie mogą modifikować plików które do nich nie należą.
![[Pasted image 20250728143513.png]]
Sticki bit jest tutaj oznaczony przez `T` to oznacza że użytkownicy nie mają (x) uprawnień wiec nie mogą zobaczyć zawartości ani jej wykonać.
Sticki bit `t` oznacza że (x) uprawnienia zostały przyznane
## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)