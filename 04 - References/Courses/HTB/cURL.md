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
 [[Linux Main|Study - {{Linux}}]]
# Study: {{cURL}}

## Objective
Use cURL from your Pwnbox (not the target machine) to obtain the source code of the "https://www.inlanefreight.com" website and filter all unique paths (https://www.inlanefreight.com/directory" or "/another/directory") of that domain. Submit the number of these paths as the answer.

## Sposób rozwiązania
1. **Pobranie kodu strony**
	`curl -s https://www.inlanefreight.com > page.html`
	- `-s` – tryb cichy, nie wyświetla paska postępu.
    - Zapisujemy całą odpowiedź (HTML) do pliku `page.html`.
2. **Wyciągnięcie wszystkich linków (`href` i `src`)
	- `grep -Eio '(href|src)=["'\''][^"'\''<>]+' page.html > raw_links.txt`
	- `-E` – używamy rozszerzonych wyrażeń regularnych.
    - `-i` – ignorujemy wielkość liter.
	- `-o` – wypisujemy tylko samą część dopasowaną (np. `href="…"`, `src='…'`).
	- **`(href|src)`**
		- Grupa przechwytująca, dopasowuje dokładnie słowo **`href`** lub **`src`**.
	    - Dzięki temu bierzemy pod uwagę zarówno odnośniki (`href="…"`) jak i zasoby osadzone (`src="…"`).
	- ` = `
		- Dosłowny znak równości – oddziela atrybut od wartości.
	- `["'\'']`
		- Zestaw znaków (ang. character class), dopasowuje **pojedynczy cudzysłów** `"` **lub** **pojedynczy apostrof** `'`.
		- Dzięki temu uwzględniamy oba sposoby zapisu atrybutu w HTML.
	- `[^"'\<>]+`
		- **`^`** na początku nawiasu kwadratowego oznacza negację zestawu.
		- Zestaw **`"'\<>`** zawiera cudzysłów (`"`), apostrof (`'`), znak mniejszości `<` i większości `>`.
		- Całość **`[^"'\<>]`** więc dopasowuje dowolny pojedynczy znak **oprócz** tych czterech.
		- **`+`** quantifier – co najmniej jeden taki znak.
		- W efekcie pobieramy ciąg znaków stanowiący wartość atrybutu aż do pierwszego napotkania kończącego cudzysłowu/apostrofu lub (ewentualnie) znaku `<` albo `>`.
		
	- Rezultat zapisujemy w `raw_links.txt`.
3. **Usunięcie prefiksu `href=` / `src=`
	- `sed -E 's/^(href|src)=["'\'']//I' raw_links.txt > links_only.txt`
	- `^` – początek linii, `(href|src)` – wybieramy oba atrybuty.
	- `["'\'']` – usuwamy cudzysłów lub apostrof.
	- `I` – ignorujemy wielkość liter w wzorcu.
4. **Filtracja linków do naszej domeny
	- `grep -E '^(https?://www\.inlanefreight\.com)?/' links_only.txt > filtered.txt`
	- Zachowujemy tylko te wiersze, które albo zaczynają się od `/…`, albo pełnego `https://www.inlanefreight.com/...`.
5. **Zamiana pełnych URL na ścieżki
	- ` `sed 's|https://www.inlanefreight.com||' filtered.txt > paths.txt`
	- Usuwamy tę część, zostawiając tylko “/ścieżka”.
6. **Usunięcie duplikatów i zliczenie unikalnych ścieżek**
	- sort -u paths.txt | wc -l
	- `sort -u` – sortuje alfabetycznie i usuwa duplikaty.
	- `wc -l` – liczy linie, czyli liczbę unikalnych ścieżek.
7. Wynik
	- 34




## Review Questions
- Co robi flaga `-o` w `grep` i dlaczego jest przydatna w tym zadaniu?
- W jakim kroku usuwamy prefiks domeny i dlaczego to jest ważne przed zliczaniem unikalnych ścieżek?
## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)