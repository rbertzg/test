![](https://media.discordapp.net/attachments/1202210478076461087/1403517823342481629/image.png?ex=689f1795&is=689dc615&hm=b0ebe464c3f6e7797b7f431a36cf4eafd6332bd3bd5a4191514889854d9ce693&=&format=webp&quality=lossless&width=902&height=733)

# Git Cheatsheet - Skondensowana Ściągawka

To jest szybka ściągawka z najważniejszych komend, flag i wzorców użycia Gita. Idealna do trzymania pod ręką podczas codziennej pracy.

1.  Pobierz aktualny main

- `git checkout main`
- `git pull --rebase`

2.  Nowy branch

- `git switch -c feature/<nazwa>`

3.  Pracuj

- `git add -A`
- `git commit -m "Opis"`

4.  Zaktualizuj branch

- `git fetch origin`
- `git rebase origin/main`

5.  Push + PR

- `git push -u origin feature/<nazwa>`

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

---

### Analiza Historii i Zmian

- `git shortlog`: Wyświetla podsumowanie historii commitów, grupując je według autora.
- `git log`
  - Wyświetla historię commitów.
  - `-3`: Pokazuje 3 ostatnie commity.
  - `--author=<username>`: Pokazuje commity danego autora.
  - `--before "rok-miesiąc-dzień"`: Pokazuje commity przed podaną datą.
  - `--after "rok-miesiąc-dzień"`: Pokazuje commity po podanej dacie.
  - `--before "rok-miesiąc-dzień" --after "rok-miesiąc-dzień"`: Pokazuje commity pomiędzy wybranymi datami.
  - `--stat --oneline`: Pokazuje informacje o zmianach (wyświetla ilości dodanych, usuniętych, zmienionych linii).
  - `--format=fuller`: Pokazuje commity z datą pierwotną i datą włączenia do gałęzi (AuthorDate i CommitDate).
  - `--pretty="<format>"`: Pokazuje logi we własnym zdefiniowanym formacie, np. `git log --pretty="%h | %an | %ar - %s"`
  - `<gałąź>`: Pokazuje commity z danej gałęzi.
  - `--oneline`: Pokazuje każdy commit w jednej linii.
  - `--graph`: Rysuje graf historii gałęzi.
  - `--decorate`: Pokazuje, gdzie wskazują gałęzie i tagi.
  - `--all`: Pokazuje historię wszystkich gałęzi.
  - `-p <ściezka_do_pliku>`: Pokazuje historię zmian tylko dla danego pliku.
  - `<ściezka_do_pliku>`: Pokazuje historię zmian tylko dla danego pliku.
  - `-3 -p`: Pokazuje historię zmian dla 3 ostatnich commitów.
  - `-S"tekst"`: Wyszukuje commity, które wprowadziły lub usunęły wystąpienia danego tekstu.
  - `--grep="tekst"`: Filtruje commity po tekście w ich opisie.
- `git diff`
  - Pokazuje różnice między katalogiem roboczym a poczekalnią.
  - `--staged` lub `--cached`: Pokazuje różnice między poczekalnią a ostatnim commitem.
  - `<gałąź1>..<gałąź2>`: Pokazuje różnice między dwiema gałęziami.
- `git show <hash_commita>`
  - Pokazuje szczegóły i zmiany wprowadzone przez dany commit.

---

### Cofanie i Poprawianie Błędów

## **UWAGA** Zmiana opisów commitów (`git commit --amend`) lub zmiana historii (`rebase`) najleiej gdyby odbywała się tylko na lokalnym repozytorium.

- `git checkout <hash_commita>`: Możliwość przełączenia na commit/HEAD bez gałęzi (detached HEAD). Przywraca projekt do stanu podanego commita. Aby wrócić do aktualnego stanu użyj `git checkout <nazwa_gałęzi>`
- `git reset`
  - `HEAD^` lub `HEAD~1`: Cofa ostatni commit, ale zachowuje zmiany w katalogu roboczym (domyślnie `--mixed`).
  - `--soft <hash/nazwa_gałęzi>`: Cofa commity, ale zostawia wszystkie zmiany w poczekalni.
  - `--hard <hash/nazwa_gałęzi>`: **NIEODWRACALNIE** cofa commity i usuwa wszystkie zmiany. Przykład: `git reset --hard origin/main`
- `git revert <hash>`
  - Tworzy nowy commit, który jest odwrotnością podanego commita. Bezpieczne dla publicznej historii.
- `git clean`
  - Usuwa nieśledzone pliki z katalogu roboczego.
  - `-n`: "Próba na sucho" - pokazuje, co zostanie usunięte.
  - `-dfx`: Usuwa wszystkie nieśledzone pliki, katalogi oraz pliki ignorowane. Używaj z rozwagą!
- `git reflog`
  - Wyświetla dziennik wszystkich operacji na `HEAD`. Twoja ostateczna siatka bezpieczeństwa do odzyskiwania "utraconych" commitów.
  - `-5`: Wyświetla 5 ostatnich operacji.

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

Jak radzić sobie z konfliktami przy `rebase`:

1. Rozwiąż konflikty w plikach.
2. Dodaj naprawione pliki: `git add`.
3. Kontynuuj proces: `git rebase --continue`.
4. Aby się poddać i wrócić do stanu początkowego: `git rebase --abort`.

---

- `git rebase -i <baza>` - przykład `git rebase -i HEAD~4` lub `git rebase origin/main` lub `git rebase -i <hash_commita>`
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

---

### Conventional Commits

Właściwe nazywanie commitów w Git jest kluczowe dla utrzymania czytelnej historii projektu. Standard Conventional Commits wprowadza ustrukturyzowany format opisów commitów, zrozumiały zarówno dla ludzi, jak i maszyn.

---

#### Złote zasady pisania dobrych commitów

- Tytuł i opis: Krótki, zwięzły tytuł + opcjonalny opis szczegółowy.
- Tryb rozkazujący: Pisanie w formie np. „Dodaj” zamiast „Dodano”.
- Limit znaków: 50–72 znaki w tytule.
- Wielka litera, brak kropki: Tytuł zaczynaj wielką literą, nie kończ kropką.
- Jedna zmiana na commit: Jeden commit = jedna logiczna zmiana.

---

#### Struktura commitów

```
<typ>[(opcjonalny zakres)]: <opis>

[opcjonalny, dłuższy opis]

[opcjonalna stopka]
```

---

#### Typy commitów

- `feat`: nowa funkcjonalność (MINOR w semantycznym wersjonowaniu)
- `fix`: naprawa błędu (PATCH)
- `docs`: zmiany w dokumentacji
- `style`: zmiany formatowania, które nie wpływają na kod
- `refactor`: zmiany w kodzie, bez naprawy błędu i nowych funkcji
- `perf`: poprawa wydajności
- `test`: dodawanie/poprawianie testów
- `build`: zmiany w procesie budowania lub zależnościach
- `ci`: zmiany w konfiguracji CI/CD
- `chore`: inne zmiany niekodowe (np. aktualizacja narzędzi)
- `revert`: wycofanie poprzedniego commita

---

#### Zakres (Scope)

Opcjonalny zakres pozwala określić część projektu, której dotyczy zmiana.
Przykłady:

- `feat(auth): dodaj logowanie przez Google`
- `fix(ui): popraw wyświetlanie przycisku`

---

#### Opis (Description)

Krótki opis zmian w commitcie, np.:

- `feat: dodaj możliwość resetowania hasła`

---

#### Dłuższy opis (Body)

Opcjonalny, bardziej szczegółowy opis zmian, motywacji i kontekstu:---

#### Typy commitów

- `feat`: nowa funkcjonalność (MINOR w semantycznym wersjonowaniu)
- `fix`: naprawa błędu (PATCH)
- `docs`: zmiany w dokumentacji
- `style`: zmiany formatowania, które nie wpływają na kod
- `refactor`: zmiany w kodzie, bez naprawy błędu i nowych funkcji
- `perf`: poprawa wydajności
- `test`: dodawanie/poprawianie testów
- `build`: zmiany w procesie budowania lub zależnościach
- `ci`: zmiany w konfiguracji CI/CD
- `chore`: inne zmiany niekodowe (np. aktualizacja narzędzi)
- `revert`: wycofanie poprzedniego commita

---

#### Zakres (Scope)

Opcjonalny zakres pozwala określić część projektu, której dotyczy zmiana.
Przykłady:

- `feat(auth): dodaj logowanie przez Google`
- `fix(ui): popraw wyświetlanie przycisku`

---

#### Opis (Description)

Krótki opis zmian w commitcie, np.:

- `feat: dodaj możliwość resetowania hasła`

---

#### Dłuższy opis (Body)

Opcjonalny, bardziej szczegółowy opis zmian, motywacji i kontekstu:

- `Dodano funkcjonalność wysyłania maila z linkiem resetującym hasło.
Ułatwia użytkownikom odzyskiwanie konta.`

---

#### Stopka (Footer)

Opcjonalna stopka może zawierać:

- odwołania do zadań w trackerze: `Fixes #123`
- informację o zmianach łamiących kompatybilność: `BREAKING CHANGE: zmieniono API logowania`

---

#### Przykłady

```
feat(uzytkownicy): Dodaj możliwość logowania przez media społecznościowe

Użytkownicy mogą teraz logować się za pomocą kont Google i Facebook.
```

---

```
fix(koszyk): Popraw obliczanie sumy zamówienia

Naprawiono błąd z naliczaniem zniżek przy określonej promocji.

Fixes: #432
```

---

```
docs(api): Zaktualizuj dokumentację endpointu /products

Dodano brakujące opisy parametrów sortowania i filtrowania.
```

---

```
feat(api)!: Zmień format odpowiedzi dla endpointu /users

BREAKING CHANGE: 'username' zmieniono na 'login' w /api/v1/users.
```

---

### Semantic Versioning (SemVer)
