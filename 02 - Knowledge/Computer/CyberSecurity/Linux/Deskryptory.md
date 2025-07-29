---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - status/in-progress
  - review/pending
  - theme/cybersecurity
  - linux
date: 2025-07-27
last_updated: 2025-07-27
---

# [[Linux Main|Study - {{Linux}}]]

## Objective
Zrozumieć Deskryptory i Przekierowania

# Deskryptory

Deskryptory są trochę jak zaklęcia z Harrego Pottera. 0 to wypowiedziane słowa (Protego Diabolica), wtedy 1 jest efektem niebieskim ognistym kręgiem. Natomiast 2 byłoby każdym płomieniem który pojawił się czerwonym.

| Deskryptor | Nazwa      | Domyślne źródło/cele             |
| ---------- | ---------- | -------------------------------- |
| `0`        | **stdin**  | standardowe wejście (klawiatura) |
| `1`        | **stdout** | standardowe wyjście (terminal)   |
| `2`        | **stderr** | standardowe wyjście błędów       |
## Przekierowania (redirections)

### 1. Standardowe wyjście `>` / `>>`

- `>` — nadpisuje plik
    
- `>>` — dopisuje na końcu
    
`echo "Hello" > out.txt ` zapisze "Hello" w out.txt (lub nadpisze jeśli istniał) `echo "Again" >> out.txt` dopisze "Again" w nowej linii`
### 2. Standardowe wejście `<`

`sort < lista.txt` posortuje zawartość lista.txt i wyniki wyświetli na ekran`

### 3. Błędy do pliku `2>`

`grep foo bigfile.txt 2> errors.log`    standardowe wyjście (wyniki grep) pójdzie na ekran, komunikaty o błędach (np. brak pliku) trafią do errors.log`

### 4. Przekierowanie stderr i stdout razem

- `&>` (Bash)
    
- `> file 2>&1` – klasyczne
    

`make &> build.log` zarówno normalne komunikaty, jak i błędy 
zapisz do build.log o samo w starszym stylu: `make > build.log 2>&1`

### 5. Przekierowanie wzajemne

- `/dev/null` — “czarna dziura” (ignoruj dane)
    
`cmd > /dev/null 2>&1`      nic się nie wyświetli ani nie zostanie zapisane 
`cat < /dev/null`           odczyta “pustkę”`

### 6. Potoki `|`
Łączą stdout jednego procesu z stdin drugiego:
`ps aux | grep ssh` output ps trafia na wejście grep`


## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)