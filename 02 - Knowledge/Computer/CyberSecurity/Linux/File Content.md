---
aliases:
  - Study - {{File Content}}
tags:
  - type/study
  - context/studies
  - status/in-progress
  - review/pending
  - theme/cybersecurity
  - theme/os
date: 2025-07-27
last_updated: 2025-07-27
---

# [[Linux Main|Study - {{Linux}}]]

## Objective
Zrozumieć jak przeglądać pliki z poziomu basha

Plik to tylko strumień bajtów. Jednak mogę z nim pracować ja z tekstem. Wszystko jest plikiem.
Gdy czytam zawartość pliku:
- mogę ją wyświetlić (`cat`, `less`, `head`)
- przetwarzać (`grep`, `awk`, `cut`, `sed`, `tr`)
- liczyć (`wc`)
- przekształcać (`sort`, `uniq`)
- filtrować (`tail -f`, `grep -v`)
## Podstawowe czytanie plików

| Polecenie   | Opis                                                    |
| ----------- | ------------------------------------------------------- |
| `cat file`  | Wyświetla cały plik                                     |
| `tac file`  | Wyświetla plik od końca                                 |
| `less file` | Stronicowany podgląd (←↑↓→ / `q` – wyjście)             |
| `more file` | Prosty stronicowany podgląd (`space` – następna strona) |
## Pierwsze / ostatnie linie

| Polecenie         | Opis                                           |
| ----------------- | ---------------------------------------------- |
| `head file`       | Pierwsze 10 linii                              |
| `head -n 20 file` | Pierwsze 20 linii                              |
| `tail file`       | Ostatnie 10 linii                              |
| `tail -n 20 file` | Ostatnie 20 linii                              |
| `tail -f file`    | “Follow” — wyświetla dopisane linie na bieżąco |
| `tail -n +5 file` | Od linii 5 do końca                            |
## Wyszukiwanie tekstu
| Komenda                | Opis                                    |
| ---------------------- | --------------------------------------- |
| `grep 'tekst' file`    | znajdź linie zawierające „tekst”        |
| `grep -i 'tekst' file` | ignoruje wielkość liter                 |
| `grep -v 'tekst' file` | zwraca linie **nie** zawierające tekstu |
| `grep -r 'tekst' dir/` | przeszukiwanie rekurencyjne katalogu    |
| `grep -n 'tekst' file` | dodaje numer linii                      |
| `grep -e`              | pozwala szukać szczególnych słów        |

## Wycinanie i formatowanie

| Polecenie                           | Opis                                                          |
| ----------------------------------- | ------------------------------------------------------------- |
| `tr '[:lower:]' '[:upper:]' < file` | Lower→UPPER                                                   |
| `sort file`                         | Posortuje linie                                               |
| `uniq file`                         | Usuń duplikaty (po sortowaniu)                                |
| `nl file`                           | Numeruje linie                                                |
| `cut -d ':' -f2 file`               | wycina kolumnę nr 2 z pliku, gdzie `:` to separator           |
| `awk '{print $2}' file`             | wypisuje drugą kolumnę (domyślnie białe znaki jako separator) |
| `tr 'a-z' 'A-Z'`                    | zamiana małych liter na wielkie                               |
| `tr -d '\r'`                        | usuwa znaki CR (np. z Windowsa)                               |
### Sed & Awk 

#### `sed` – edytor strumieniowy

|Przykład|Opis|
|---|---|
|`sed 's/foo/bar/' file`|zamień pierwsze „foo” na „bar” w każdej linii|
|`sed -n '10,20p' file`|wypisz linie od 10 do 20|
|`sed '/pattern/d' file`|usuń linie zawierające wzorzec|
#### `awk` – język do analizy tekstu (nawet programowania)

|Przykład|Opis|
|---|---|
|`awk '{print $1}' file`|wypisz pierwszą kolumnę|
|`awk '$3 > 100 {print $1, $3}'`|warunkowe wypisywanie (jeśli 3. kolumna > 100)|
|`awk -F ':' '{print $2}'`|zmiana separatora (np. na `:`)|
## Potoki i redirekcje

`grep ERROR log.txt | tail -n 50`  ostatnie 50 błędów 
`cat file | grep foo | wc -l ` liczba linijek z foo 
`sed '…' file > out.txt`zapis wyników do pliku 
`cmd > /dev/null 2>&1`wycisz stdout i stderr`

- `|` – potok stdout ▶ stdin kolejnej
    
- `>` – overwrite do pliku
    
- `>>` – append do pliku
    
- `<` – stdin z pliku
    
- `2>` – stderr do pliku
    
- `2>&1` – stderr do stdout

## Przykłady użycia
- Znajdź ostatnie błędy w systemie `grep -i error /var/log/syslog | tail -n 20` Wyświetl tylko unikalne komunikaty błędów `grep -i error /var/log/syslog | sort | uniq ` 
- Przeanalizuj tylko linie 100–200 w pliku `sed -n '100,200p' huge.log  
- Policz wystąpienia słowa TODO w projekcie `grep -R --include='*.py' -i TODO . | wc -l   
- Zmień wszystkie 'foo' na 'bar' na wejściu `sed 's/foo/bar/g' < input.txt | less  
- Śledź log podczas jednoczesnego filtrowania `tail -f access.log | grep --line-buffered ' 500 '`

## `cut`

Wyodrębnia kolumny lub zakresy znaków.

- **Po kolumnach (separator)**
    `cut -d':' -f1 /etc/passwd` - pierwsze pole (login) z pliku /etc/passwd, separator ':'`
- **Po znakach**
    `cut -c1-5 file.txt` 1–5 z każdej linii`
- **Kilka pól**
    `cut -d',' -f1,3 data.csv` pola 1 i 3 (oddzielone przecinkiem)`

---

## `tr`

Translacja lub usuwanie znaków.

- **Zmiana małych na duże**\ 
    `tr '[:lower:]' '[:upper:]' < file.txt`
- **Usuwanie znaków**
    `tr -d '\r' < dosfile.txt` usuń znaki CR (Windows → Unix)`
    
- **Zamiana tabulatorów na spacje**
    `tr '\t' ' ' < input.txt`
---

## `column`

Formatuje kolumny wyrównane do siatki.

- **Podział na kolumny według separatora**
    `cat data.txt | column -t -s',' ` wyrównaj tabele, separatorem ','
- **Lista w kolumny**
    `ls | column` rozmieść nazwy plików w kilka kolumn`

---

## `awk`

Potężny język do pola‑wierszowej manipulacji.

- **Wypisz pole 2**
    `awk '{print $2}' file.txt`
- **Filtruj i formatuj**    
    `awk -F':' '$3 > 1000 { print $1, $3 }' /etc/passwd # separator ':',` wyświetl login i UID tylko jeśli UID>1000`
- **Sumuj kolumnę**
    `awk '{sum += $5} END {print sum}' data.txt`

---
## `sed`

Strumieniowy edytor (do substytucji, wstawiania, usuwania linii).

- **Zamiana tekstu**    
    `sed 's/old/new/g' file.txt` zamień wszystkie 'old' na 'new'`
- **Usunięcie linii 10–20**
    `sed '10,20d' file.txt`
- **Wstawienie tekstu po linii 5**
	`sed '5a\Nowa linia tekstu' file.txt`
---

## `wc`

Zlicza linie, słowa i znaki.

- **Liczba linii**
    `wc -l file.txt` wypisze: "liczba file.txt"`
- **Liczba słów**
	`wc -w file.txt`
- **Liczba znaków**
    `wc -c file.txt`
- **Wszystko na raz**
    `wc file.txt` -l, -w i -c w jednym`
## Review Questions
- 



## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)