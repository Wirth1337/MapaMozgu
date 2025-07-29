---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - status/in-progress
  - review/pending
  - theme/cybersecurity
  - theme/os
date: 2025-07-28
last_updated: 2025-07-28
---

# [[Linux Main|Study - {{Linux}}]]

## Objective
What is the goal of this study?

## Content
## Co to jest?
Za pomocą RegEx mogę tworzyć dokładnie blueprinty żeby szukać patternów w tekscie lub pliku.
### Zasada 3Z (Nie Zakuj Zdaj Zapomnij)
- Znajdz
- Zamień
- Zmień
Dziękli nim mogę analizować dane, sprawdzić input i przeprowadzać zawansowane operacje szukania.

---
### Operatory 

| Operators | Description                                                                                                                                   |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `(a)`     | Nawiasy okrągłe służą do grupowania części wyrażenia regularnego. W ich obrębie można zdefiniować wzorce, które mają być przetwarzane razem.  |
| `[a-z]`   | Nawiasy kwadratowe służą do definiowania klas znaków. W ich obrębie można wskazać listę znaków, które mają zostać dopasowane.                 |
| `{1,10}`  | Nawiasy klamrowe służą do określenia kwantyfikatorów – wskazują, ile razy dany wzorzec powinien zostać powtórzony (np. od 1 do 10 razy).      |
| `\|`      | Operator OR – dopasowuje, gdy przynajmniej jeden z dwóch wzorców pasuje.                                                                      |
| `.*`      | Oznacza dowolny ciąg znaków – działa jak AND, gdyż dopasowanie następuje tylko wtedy, gdy oba elementy są obecne i w odpowiedniej kolejności. |
## OR
![[Pasted image 20250728132612.png]]
Jeden z dwóch parametrów zawsze pojawia się w którejś z 3 lini więc wszystkie są wyświetlane.
## AND
![[Pasted image 20250728132721.png]]
Tutaj wyświetlamy te linie które spełniają warunek pojawienia się obu parametrów. 
![[Pasted image 20250728132847.png]]
Można też to zrobić tak.

---
## CheatSheet Regex dla Grep

| Case                                 | Pattern           | Description                                                                                    |                                 |      |
| ------------------------------------ | ----------------- | ---------------------------------------------------------------------------------------------- | ------------------------------- | ---- |
| **Zawiera**                          | `foo`             | Dopasuje każdą linię zawierającą ciąg `foo`.                                                   |                                 |      |
| **Zaczyna się od**                   | `^foo`            | Dopasuje linię, która zaczyna się od `foo`.                                                    |                                 |      |
| **Kończy się na**                    | `foo$`            | Dopasuje linię, która kończy się na `foo`.                                                     |                                 |      |
| **Dokładnie równa**                  | `^foo$`           | Dopasuje linię, która składa się **tylko** z `foo`.                                            |                                 |      |
| **Słowo równe**                      | `\bfoo\b`         | Dopasuje całe słowo `foo` (granice słów). Wymaga `grep -P` lub symulacji `(^                   | [^[:alnum:]_])foo([^[:alnum:]_] | $)`. |
| **Słowo zaczyna się od**             | `\bfoo`           | Dopasuje słowo zaczynające się od `foo`.                                                       |                                 |      |
| **Słowo kończy się na**              | `foo\b`           | Dopasuje słowo kończące się na `foo`.                                                          |                                 |      |
| **Dowolny znak**                     | `.`               | Jeden dowolny znak (bez końca linii).                                                          |                                 |      |
| **Dowolny ciąg znaków**              | `.*`              | Zero lub więcej dowolnych znaków.                                                              |                                 |      |
| **Co najmniej 1 znak**               | `.+`              | Jeden lub więcej dowolnych znaków.                                                             |                                 |      |
| **Opcjonalnie**                      | `?`               | Zero lub jeden wystąpienie poprzedniego elementu.                                              |                                 |      |
| **Dokładnie n powtórzeń**            | `{n}`             | Poprzedni element występuje dokładnie `n` razy.                                                |                                 |      |
| **Zakres powtórzeń od n do m**       | `{n,m}`           | Poprzedni element powtarza się od `n` do `m` razy.                                             |                                 |      |
| **Klasa znaków**                     | `[abc]`           | Jeden znak: `a`, `b` lub `c`.                                                                  |                                 |      |
| **Negowana klasa znaków**            | `[^abc]`          | Jeden znak **inny niż** `a`, `b` lub `c`.                                                      |                                 |      |
| **Cyfra**                            | `[0-9]`           | Jeden znak numeryczny.                                                                         |                                 |      |
| **Nie-cyfra**                        | `\D` lub `[^0-9]` | Jeden znak nienumeryczny.                                                                      |                                 |      |
| **Biała spacja**                     | `\s`              | Dowolny biały znak (spacja, tabulacja, nowa linia…).                                           |                                 |      |
| **Nie-biała spacja**                 | `\S`              | Dowolny znak niebędący białą spacją.                                                           |                                 |      |
| **Znak słowa**                       | `\w`              | Jeden znak alfanumeryczny lub podkreślenie.                                                    |                                 |      |
| **Nie-znak słowa**                   | `\W`              | Jeden znak niebędący alfanumerycznym ani podkreśleniem.                                        |                                 |      |
| **OR (alternatywa)**                 | `foo              | bar`                                                                                           | Dopasuje `foo` lub `bar`.       |      |
| **Grupowanie**                       | `(pattern)`       | Zgrupuje `pattern` jako jedną jednostkę.                                                       |                                 |      |
| **Poztywne lookahead**               | `(?=pattern)`     | Sprawdza, czy **zaraz za** bieżącym miejscem pojawia się `pattern` (wymaga `grep -P`).         |                                 |      |
| **Negatywne lookahead**              | `(?!pattern)`     | Sprawdza, czy **zaraz za** bieżącym miejscem **nie** pojawia się `pattern` (wymaga `grep -P`). |                                 |      |
| **Poztywne lookbehind**              | `(?<=pattern)`    | Sprawdza, czy **przed** bieżącym miejscem był `pattern` (wymaga `grep -P`).                    |                                 |      |
| **Negatywne lookbehind**             | `(?<!pattern)`    | Sprawdza, czy **przed** bieżącym miejscem **nie** było `pattern` (wymaga `grep -P`).           |                                 |      |
| **Linie **nie** zawierające**        | `-v 'foo'`        | W `grep`: odrzuca wszystkie linie zawierające `foo`.                                           |                                 |      |
| **Linie **nie** zaczynające się od** | `-Ev '^foo'`      | W `grep -E`: odrzuca linie zaczynające się od `foo`.                                           |                                 |      |
| **Linie **nie** kończące się na**    | `-Ev 'foo$'`      | W `grep -E`: odrzuca linie kończące się na `foo`.                                              |                                 |      |

---

### Zadanka
1. Show all lines that do not contain the `#` character.
	1. `grep -v '#' /etc/passwd`
		1. `-v` odwraca dopasowanie (zwraca linie **niezawierające** wzorca)
2. Search for all lines that contain a word that starts with `Permit`.
	1. `grep -E '^Permit' filename`
	2. `\b` To tzw. **granica słowa** – dopasowanie następuje **tylko na początku słowa
	3. `-E` Włącza **rozszerzone wyrażenia regularne (ERE)**
3. Search for all lines that contain a word ending with `Authentication`.
	1. `grep -E 'Auth$' filename` Gdy Szukamy słowa **na samym końcu linii**
	2. `grep -E 'Auth\b' filename` słowo kończy się na `Auth`, ale może mieć coś po nim w linii (np. spację, kropkę, itp.)
4. Search for all lines containing the word `Key`.
	1. `grep -E 'key' filename`
5. Search for all lines beginning with `Password` and containing `yes`.
6. `grep -E '^Password.*yes' filename`
7. Search for all lines that end with `yes`.
	1. `grep -E 'yes$' filename`


## Review Questions
- Jak dopasować całe słowo `root`?
- Co robi `grep -Ev '^#' filename`?
- Jak znaleźć linie, które NIE zawierają `yes`?


## Summary
- Wiem, jak używać `grep -E` z wyrażeniami regularnymi.
- Umiem używać `^`, `$`, `\b` do precyzyjnych dopasowań.
- Poznałem różnice między `grep`, `grep -E`, `grep -P`.


## References
- [Reference 1](link)
- [Reference 2](link)