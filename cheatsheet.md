# Git Cheatsheet - Skondensowana Ściągawka

To jest szybka ściągawka z najważniejszych komend, flag i wzorców użycia Gita. Idealna do trzymania pod ręką podczas codziennej pracy.

---

### Konfiguracja i Inicjalizacja

- `git config --global user.name "Twoje Imię"`
  - Ustawia globalnie nazwę użytkownika dla wszystkich Twoich repozytoriów.
- `git config --global user.email "twoj.email@example.com"`
  - Ustawia globalnie e-mail.
- `git config --global alias.<skrót> <komenda>`
  - Tworzy skrót dla dłuższej komendy (np. `alias.lg "log --color --graph..."`).
- `git init`
  - Inicjalizuje nowe repozytorium w bieżącym folderze.
- `git clone <url_repozytorium>`
  - Klonuje zdalne repozytorium.
  - `--depth=1`: Klonuje tylko ostatni commit (shallow clone), idealne dla CI/CD.

---

### Podstawowy Cykl Pracy

- `git status`
  - Pokazuje status plików w katalogu roboczym i poczekalni.
  - `-s` lub `--short`: Wyświetla skrócony, zwięzły status.
- `git add <plik/katalog>`
  - Dodaje zmiany do poczekalni.
  - `.`: Dodaje wszystkie zmiany z bieżącego katalogu.
  - `-p` lub `--patch`: Pozwala interaktywnie wybrać fragmenty zmian (hunks), które chcesz dodać. Niezwykle przydatne!
- `git commit`
  - Zatwierdza zmiany z poczekalni.
  - `-m "Opis commita"`: Dodaje opis bezpośrednio w komendzie.
  - `-am "Opis"`: Dodaje wszystkie śledzone pliki i od razu tworzy commit.
  - `--amend`: Modyfikuje ostatni commit (zmienia jego zawartość lub opis). Tworzy nowy commit na bazie starego, możemy dołączyć nowe zmiany.
- `git restore`
  - Odrzuca zmiany w katalogu roboczym
  - `--staged`: Wycofuje zmiany z poczekalni do katalogu roboczego
  - `--source=<hash_commita/nazwa_gałęzi> <nazwa_pliku>`: Przywracanie pliku z konkretnego commita lub gałęzi

---

### Praca z Gałęziami (Branching)

- `git branch`
  - Wyświetla listę lokalnych gałęzi.
  - `-a`: Wyświetla wszystkie gałęzie (lokalne i zdalne).
  - `-v`: Pokazuje ostatni commit na każdej gałęzi.
  - `<nazwa_gałęzi>`: Tworzy nową gałąź.
  - `-d <nazwa_gałęzi>`: **Bezpiecznie** usuwa gałąź (tylko jeśli została scalona).
  - `-D <nazwa_gałęzi>`: **Wymusza** usunięcie gałęzi (nawet jeśli nie została scalona).
- `git checkout <nazwa_gałęzi>`
  - Przełącza na inną gałąź.
  - `-b <nazwa_nowej_gałęzi>`: Tworzy nową gałąź i od razu się na nią przełącza.
  - `<plik>`: Odrzuca zmiany w danym pliku w katalogu roboczym.
- `git switch`
  - `<nazwa_gałęzi>`: Przejście na istniejącą gałąź.
  - `-c <nazwa_gałęzi>`: Utworzenie i przejście na nową gałąź.
  - `-`: Powrót do poprzedniej gałęzi.
- `git merge <nazwa_gałęzi>`
  - Scala podaną gałąź z bieżącą.
  - `--no-ff`: Zawsze twórz merge commit, nawet jeśli możliwe jest przewinięcie (fast-forward). Zachowuje kontekst istnienia gałęzi.
  - `--squash`: Scala wszystkie commity z gałęzi w jeden, gotowy do zatwierdzenia. Nie tworzy merge commita. Usunąć branch po squash merge!
  - `--abort`: Przerywa proces scalania, jeśli wystąpiły konflikty.
- test

---

### Analiza Historii i Zmian

- `git log`
  - Wyświetla historię commitów.
  - `<gałąź>`: Pokazuje commity z danej gałęzi.
  - `--oneline`: Pokazuje każdy commit w jednej linii.
  - `--graph`: Rysuje graf historii gałęzi.
  - `--decorate`: Pokazuje, gdzie wskazują gałęzie i tagi.
  - `--all`: Pokazuje historię wszystkich gałęzi.
  - `-p <plik>`: Pokazuje historię zmian tylko dla danego pliku.
  - `-S"tekst"`: Wyszukuje commity, które wprowadziły lub usunęły wystąpienia danego tekstu.
  - `--grep="wzorzec"`: Filtruje commity po wzorcu w ich opisie.
- `git diff`
  - Pokazuje różnice między katalogiem roboczym a poczekalnią.
  - `--staged` lub `--cached`: Pokazuje różnice między poczekalnią a ostatnim commitem.
  - `<gałąź1>..<gałąź2>`: Pokazuje różnice między dwiema gałęziami.
- `git show <hash_commita>`
  - Pokazuje szczegóły i zmiany wprowadzone przez dany commit.

---

### Cofanie i Poprawianie Błędów

- `git checkout <hash_commita>`: Możliwość przełączenia na commit/HEAD bez gałęzi (detached HEAD). Przywraca projekt do stanu podanego commita. Aby wrócić do aktualnego stanu użyj `git checkout <nazwa_gałęzi>`
- `git reset`
  - `HEAD^` lub `HEAD~1`: Cofa ostatni commit, ale zachowuje zmiany w katalogu roboczym (domyślnie `--mixed`).
  - `--soft <hash>`: Cofa commity, ale zostawia wszystkie zmiany w poczekalni.
  - `--hard <hash>`: **NIEODWRACALNIE** cofa commity i usuwa wszystkie zmiany.
- `git revert <hash>`
  - Tworzy nowy commit, który jest odwrotnością podanego commita. Bezpieczne dla publicznej historii.
- `git clean`
  - Usuwa nieśledzone pliki z katalogu roboczego.
  - `-n`: "Próba na sucho" - pokazuje, co zostanie usunięte.
  - `-dfx`: Usuwa wszystkie nieśledzone pliki, katalogi oraz pliki ignorowane. Używaj z rozwagą!
- `git reflog`
  - Wyświetla dziennik wszystkich operacji na `HEAD`. Twoja ostateczna siatka bezpieczeństwa do odzyskiwania "utraconych" commitów.

---

### Praca Zdalna (Remote)

- `git remote`
  - `-v`: Pokazuje wszystkie skonfigurowane zdalne repozytoria z adresami URL.
  - `add <nazwa> <url>`: Dodaje nowe zdalne repozytorium.
- `git fetch <nazwa_zdalnego>`
  - Pobiera wszystkie nowe dane (gałęzie, tagi) ze zdalnego repozytorium, ale nie scala ich z Twoją pracą.
- `git pull`
  - To skrót dla `git fetch` + `git merge`.
  - `--rebase`: Używa `rebase` zamiast `merge` do integracji zmian. Utrzymuje liniową historię.
- `git push`
  - `-u <nazwa> <gałąź>` lub `--set-upstream`: Ustawia domyślną zdalną gałąź dla bieżącej gałęzi lokalnej.
  - `--tags`: Wypycha wszystkie lokalne tagi.
  - `--force`: **Wymusza** nadpisanie zdalnej gałęzi. Niebezpieczne, jeśli ktoś inny pracuje na tej gałęzi.
  - `--force-with-lease`: Bezpieczniejsza wersja `force`. Nie nadpisze zdalnej gałęzi, jeśli ktoś inny wypchnął na nią zmiany, odkąd ostatnio pobrałeś.

---

### Zaawansowane Narzędzia

- `git rebase -i <baza>` - przykład `git rebase -i HEAD~4`
  - Otwiera interaktywną sesję do edycji, łączenia, usuwania i zmiany kolejności commitów. Po zakończeniu `rebase` należy użyć `git push --force-with-lease`, aby zaktualizować historię na zdalnym repozytorium.
- `git cherry-pick <hash>`
  - Aplikuje jeden konkretny commit z innej gałęzi na bieżącą.
- `git stash`
  - Chowa bieżące, niezatwierdzone zmiany. Indeks 0 wskazuje najświeższe zmiany w schowku. **Pozwala przenosić niezatwierdzone zmiany pomiędzy gałęziami.**
  - `-u`: Pozwala zapisać w schowku nieśledzone pliki.
  - `-a`: Pozwala zapisać w schowku wszystkie pliki (również te ignorowane przez gita).
  - `save "Opis schowka"`: Zapisuje zmiany w schowku z własnym opisem.
  - `list`: Pokazuje listę schowków.
  - `branch <nazwa_gałęzi> <nazwa_schowka>`: Utworzy nową gałąź, zastosuje na niej zmiany z wybranego schowka oraz usunie ten schowek.
  - `show <nazwa_schowka> -p`: Pokazuje zmiany, które znajdują się w schowku.
  - `pop`: Przywraca ostatni schowek i usuwa go z listy.
    - `<nazwa_schowka>`: Przywraca konkretny schowek i usuwa
  - `apply`: Przywraca ostatni schowek, ale go nie usuwa.
    - `<nazwa_schowka>`: Przywraca konkretny schowek bez usuwania
  - `drop`: Usuwa ostatni schowek.
    - `<nazwa_schowka>`: Usuwa konkretny schowek
  - `clear`: Czyści wszystkie schowki
- `git bisect`
  - `start`: Rozpoczyna proces szukania błędu.
  - `good <hash>`: Oznacza commit jako "dobry" (bez błędu).
  - `bad <hash>`: Oznacza commit jako "zły" (z błędem).
  - `reset`: Kończy sesję `bisect`.

---

### Zarządzanie Repozytorium

- `git tag`
  - `-a <nazwa>`: Tworzy tag adnotowany (zalecany).
  - `-m "opis"`: Dodaje opis do tagu.
  - `-d <nazwa>`: Usuwa tag.
- `git gc`
  - Uruchamia "garbage collector" - czyści i optymalizuje repozytorium.
- `git lfs`
  - `install`: Inicjalizuje Git LFS.
  - `track "*.psd"`: Rozpoczyna śledzenie plików o danym rozszerzeniu przez LFS.
