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
#apache #networking 
#  [[Linux Main|Study - {{Linux}}]] 

## Ogólnie
Kluczowym elementem w web development jest komunikacja między przeglądarką i serwerem. 
Apache jest silnikiem dla strony.

Albo fundamentami pod dom. Tak jak można dodawać inne pokoje tak do apache można dodawać moduły każdy przystosowany do konkretnego użycia.

---
## Content
## Apache 

Instalujemy Apache `sudo apt install apache2 -y`
![[Pasted image 20250729135634.png]]
**Następnie możemy wystartować server za pomocą:
- `apache2ctl
- `systemctl
- `service
Można też użyć apache2 binary ale sie nie zaleca (zmienne środowiskowe w defaultowej konfiguracji)
`sudo systemctl start apache2`
![[Pasted image 20250729135825.png]]
**Gdy Apache się ruchomi możemy użyć przeglądarki do `http://localhost` Domyślnie na porcie 80
![[apache-default.webp]]
Podczas ćwiczenia na virtualce wyskakuje błąd.
Prawdopodobnie błąd portu, żeby go zmienić
`sudo nano /etc/apache2/port.conf`
Zmieniamy port z 80 na 8080
![[Pasted image 20250729141244.png]]
Następnie `sudo systemctl restart apache2`**
Restartujemy Apache i idziemy do `http://localhost:8080` albo używamy curla.
`curl -I [http://localhost:8080](http://localhost:8080/)`
![[Pasted image 20250729194252.png]]
Korzystanie z narzędzi CLI, takich jak `curl` i `wget`, pozwala komunikować się z serwerami WWW bez potrzeby użycia przeglądarki. To jak terminalowy odpowiednik przeglądarki – umożliwia pobieranie i analizę zawartości stron internetowych.

Na przykład, prosty skrypt bashowy może pobrać stronę i wyciągnąć z niej wszystkie URL-e. Takie podejście przydaje się m.in. w web scrapingu, testach automatycznych i monitorowaniu zmian na stronach.

--- 

### 
Main content here...

## Review Questions
- Question 1?
- Question 2?

## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)