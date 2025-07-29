---
aliases:
  - Study - {{title}}
tags:
  - type/study
  - context/studies
  - status/in-progress
  - review/pending
  - theme/os
  - theme/cybersecurity
date: 2025-07-27
last_updated: 2025-07-27
linux: "#linux"
---
#linux [[Linux Main|Study - {{Linux}}]]
# Study: {{title}}

## 1. Wprowadzenie

- **Tryby**:
    
    - **Normal** (ESC) – nawigacja i komendy.
        
    - **Insert** (i, a, o) – wstawianie tekstu.
        
    - **Visual** (v, V, ) – zaznaczanie.
        
    - **Command-line** (:) – polecenia ex (`:w`, `:q`).
        
- **Podstawowe uruchomienie**:
    
    ```
    vim <plik>
    vim +<linia> <plik>  " od razu przejdź do linii
    vim -u NONE <plik>    " bez konfiguracji użytkownika
    ```
    

---

## 2. Nawigacja

- Poruszanie po znakach: `h` `j` `k` `l`
    
- Słówka: `w` (next), `b` (back), `e` (end)
    
- Linie: `0`, `^`, `$`, `G`, `gg`, `:<numer>`
    
- Ekran: `Ctrl-f`, `Ctrl-b`, `Ctrl-d`, `Ctrl-u`
    
- Kluczowe: `zz` (centruj), `zt`, `zb`
    

---

## 3. Edycja

- Wchodzenie:
    
    - `i` (insert before cursor)
        
    - `a` (append after cursor)
        
    - `o` (nowa linia poniżej)
        
- Usuwanie:
    
    - `x` (znak), `dd` (linia), `dw` (słowo), `d$`, `d^`
        
- Kopiowanie/Wklejanie:
    
    - `yy` (yank linia), `yw`, `y$`, `p`, `P`
        

---

## 4. Przeszukiwanie i zamiana

- Wyszukiwanie: `/pattern` (next: `n`, prev: `N`)
    
- Zamiana (globalnie / w linii):
    
    ```
    :%s/old/new/gc  " confirm each
    :5,10s/foo/bar/g
    ```
    
- Wyrażenia regularne: `\v` – very magic
    

---

## 5. Praca z plikami

- Bufory i pliki:
    
    - `:e <plik>`, `:bn`, `:bp`, `:bd`
        
- Okna (splity):
    
    - `:split`, `:vsplit`, `Ctrl-w h/j/k/l`, `:resize`, `:vertical resize`
        
- Zakładki (tabs):
    
    - `:tabnew`, `:tabnext`, `:tabclose`
        

---

## 6. Makra i skróty

- Nagrywanie makra:
    
    - `q<register>` – start, `q` – stop, `@<register>` – odtwórz
        
- Powtarzanie poleceń: `.` (kropka)
    
- Skróty mapowania:
    
    ```
    nnoremap <leader>w :w<CR>
    inoremap jk <ESC>
    ```
    

---

## 7. Konfiguracja

- Plik: `~/.vimrc` lub `~/.config/nvim/init.vim`
    
- Przykład:
    
    ```
    set number            " numeruj linie
    set relativenumber    " numeracja względna
    set tabstop=4         " tab = 4 spacje
    set expandtab         " zamieniaj tab na spacje
    set shiftwidth=4      " szer. wcięcia
    set clipboard=unnamedplus  " schowek systemowy
    ```
    
- Pluginy (vim-plug):
    
    ```
    call plug#begin('~/.vim/plugged')
    Plug 'tpope/vim-sensible'
    Plug 'junegunn/fzf', {'do': { -> fzf#install() }}
    call plug#end()
    ```
    

---

## 8. Zaawansowane

- **LSP** (Language Server Protocol): coc.nvim, nvim-lspconfig
    
- **Fuzzy finder**: fzf.vim, Telescope
    
- **Drzewo plików**: NERDTree, nvim-tree.lua
    
- **Statusline**: lightline, airline
    
- **Motywy**: gruvbox, onedark, nord
    
- **Debugowanie**: vim-test, vimspector
## Summary
Write a summary of what you've learned.

## References
- [Reference 1](link)
- [Reference 2](link)