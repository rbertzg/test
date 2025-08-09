# Kompletny Przewodnik po Git: Od Podstaw do Poziomu Eksperta

Ten przewodnik gromadzi wiedzę na temat Gita, zaczynając od absolutnych podstaw, a kończąc na strategiach i narzędziach używanych przez ekspertów.

## Spis Treści

- [Część 1: Fundamenty (Dla Początkujących)](#część-1-fundamenty-dla-początkujących)
  - [Kluczowe Koncepcje](#kluczowe-koncepcje)
  - [Najważniejsze Komendy](#najważniejsze-komendy)
  - [Praca z Gałęziami (Podstawy)](#praca-z-gałęziami-podstawy)
  - [Dobre Praktyki na Start](#dobre-praktyki-na-start)
- [Część 2: Poziom Średniozaawansowany](#część-2-poziom-średniozaawansowany)
  - [Ratowanie Sytuacji i Poprawianie Błędów](#ratowanie-sytuacji-i-poprawianie-błędów)
  - [Utrzymywanie Czystej Historii (`rebase`)](#utrzymywanie-czystej-historii-rebase)
  - [Zaawansowana Praca ze Zmianami](#zaawansowana-praca-ze-zmianami)
  - [Tagowanie i Wersjonowanie](#tagowanie-i-wersjonowanie)
- [Część 3: Poziom Eksperta](#część-3-poziom-eksperta)
  - [Automatyzacja (`git hooks`)](#automatyzacja-git-hooks)
  - [Zaawansowana Diagnostyka Kodu](#zaawansowana-diagnostyka-kodu)
  - [Optymalizacja Pracy z Dużymi Repozytoriami](#optymalizacja-pracy-z-dużymi-repozytoriami)
  - [Zarządzanie Złożonymi Zależnościami](#zarządzanie-złożonymi-zależnościami)
- [Część 4: Poziom Mistrzowski](#część-4-poziom-mistrzowski)
  - [Filozofia i Model Myślowy Gita](#filozofia-i-model-myślowy-gita)
  - [Zaawansowane Strategie Przepływu Pracy](#zaawansowane-strategie-przepływu-pracy)
  - [Higiena i Utrzymanie Repozytorium](#higiena-i-utrzymanie-repozytorium)
  - [Bezpieczeństwo i Wiarygodność Historii](#bezpieczeństwo-i-wiarygodność-historii)
- [Część 5: Strategie Przepływu Pracy (Workflows) w 2025](#część-5-strategie-przepływu-pracy-workflows-w-2025)
  - [Trunk-Based Development (TBD)](#trunk-based-development-tbd)
  - [GitHub Flow](#github-flow)
  - [GitFlow](#gitflow)
  - [Tabela Porównawcza](#tabela-porównawcza)
- [Dodatek: Przydatne Aliasy](#dodatek-przydatne-aliasy)

---

## Część 1: Fundamenty (Dla Początkujących)

### Kluczowe Koncepcje

- **System Kontroli Wersji (VCS):** Narzędzie do śledzenia zmian w plikach. Umożliwia powrót do poprzednich wersji i współpracę.
- **Rozproszony VCS:** Każdy programista ma na swoim komputerze pełną kopię historii projektu. Pozwala na pracę offline.
- **Repozytorium (Repo):** Folder projektu z całą historią zmian.
  - **Lokalne:** Na Twoim komputerze.
  - **Zdalne:** W chmurze (np. GitHub, GitLab), służy do synchronizacji pracy.
- **Trzy Stany Gita:**
  1.  **Katalog roboczy (Working Directory):** Pliki, które aktualnie edytujesz.
  2.  **Poczekalnia (Staging Area / Index):** Miejsce, gdzie dodajesz zmiany, które chcesz zapisać w następnym commicie.
  3.  **Lokalne repozytorium (.git):** "Zatwierdzone" zmiany, które tworzą historię projektu.
- **Commit:** "Migawka" stanu Twoich plików w danym momencie. Ma unikalny identyfikator (hash) i opis.
- **Gałąź (Branch):** Niezależna linia rozwoju. Pozwala na pracę nad nowymi funkcjami bez psucia stabilnej, głównej wersji (`main`).
- **HEAD:** Wskaźnik, który pokazuje, na której gałęzi i na którym commicie się aktualnie znajdujesz.

### Najważniejsze Komendy

Początkowa, jednorazowa konfiguracja:

```bash
# Ustaw swoją nazwę użytkownika
git config --global user.name "Twoje Imię"

# Ustaw swój adres e-mail
git config --global user.email "twoj.email@example.com"
```

Tworzenie nowego repozytorium lub klonowanie istniejącego:

```bash
# Tworzy nowe repozytorium w bieżącym folderze
git init

# Tworzy lokalną kopię repozytorium zdalnego
git clone <adres URL repozytorium>
```

Podstawowy cykl pracy (dodaj, zatwierdź, wyślij):

```bash
# 1. Sprawdź status plików
git status

# 2. Dodaj zmiany do poczekalni (wszystkie lub konkretny plik)
git add .
git add <nazwa_pliku>

# 3. Zatwierdź zmiany (stwórz commit)
git commit -m "Krótki, zrozumiały opis zmian"

# 4. Wyślij zmiany na serwer zdalny
git push
```

Synchronizacja ze zdalnym repozytorium:

```bash
# Pobiera zmiany ze zdalnego repozytorium i łączy je z Twoją lokalną wersją
git pull

# Pobiera informacje o zmianach ze zdalnego repozytorium, ale ich nie łączy
git fetch
```

### Praca z Gałęziami (Podstawy)

```bash
# Wyświetla listę wszystkich lokalnych gałęzi
git branch

# Tworzy nową gałąź
git branch <nazwa_nowej_gałęzi>

# Przełącza na istniejącą gałąź
git checkout <nazwa_gałęzi>

# Komenda-skrót: tworzy nową gałąź i od razu się na nią przełącza
git checkout -b <nazwa_nowej_gałęzi>

# Łączy zmiany z innej gałęzi do tej, na której aktualnie jesteś
git merge <nazwa_gałęzi_do_połączenia>

# Usuwa gałąź (tylko jeśli jej zmiany zostały już połączone)
git branch -d <nazwa_gałęzi>
```

### Dobre Praktyki na Start

- **Częste, małe commity:** Łatwiej zrozumieć historię i cofnąć małą zmianę.
- **Dobre opisy commitów:** Pisz w trybie rozkazującym (np. "Dodaj formularz logowania").
- **Pracuj na gałęziach:** Nigdy nie pracuj bezpośrednio na gałęzi `main`.

---

## Część 2: Poziom Średniozaawansowany

### Ratowanie Sytuacji i Poprawianie Błędów

- **`git commit --amend`**: Poprawia **ostatni commit** (zmienia jego opis lub dodaje zapomniane pliki).
  > **Uwaga:** Używaj tylko na commitach, których jeszcze nie wypchnąłeś do zdalnego repozytorium!
- **`git revert <hash_commita>`**: **Bezpiecznie cofa zmiany** z commita, który jest już w zdalnym repozytorium. Tworzy _nowy_ commit, który jest odwrotnością tego wycofywanego.
- **`git reset <hash_commita>`**: "Przesuwa" gałąź do wcześniejszego punktu w historii, odrzucając commity. **Używaj głównie lokalnie!**
  - `--soft`: Cofa commity, ale zmiany zostawia w poczekalni (staging area).
  - `--mixed` (domyślny): Cofa commity, a zmiany przenosi do katalogu roboczego.
  - `--hard`: **NIEODWRACALNIE usuwa** commity i wszystkie zmiany w plikach. Bardzo ostrożnie!
- **`git reflog`**: Twoja siatka bezpieczeństwa. To lokalny dziennik wszystkich operacji. Jeśli "zgubiłeś" commity (np. po złym `reset --hard`), `reflog` pozwoli Ci je odnaleźć i do nich wrócić.

### Utrzymywanie Czystej Historii (`rebase`)

`Rebase` "przesuwa" Twoje commity tak, aby zaczynały się na końcu innej gałęzi, tworząc liniową, czystą historię. Jest alternatywą dla `merge`.

- **`git rebase <nazwa_gałęzi>`**: Aktualizuje Twoją gałąź o najnowsze zmiany z innej gałęzi (np. `main`).
  > **Złota zasada:** Nigdy nie wykonuj `rebase` na gałęzi, którą już wypchnąłeś i na której pracują inni!
- **`git rebase -i HEAD~N` (Interaktywny rebase)**: Twoje narzędzie do "sprzątania" lokalnej historii _przed_ wysłaniem. Pozwala na:
  - `pick`: użyj commita.
  - `reword`: zmień opis commita.
  - `edit`: zatrzymaj się, by edytować commit.
  - `squash`: połącz commit z poprzednim.
  - `fixup`: jak `squash`, ale odrzuć opis.
  - `drop`: usuń commit.

### Zaawansowana Praca ze Zmianami

- **`git stash`**: Chwilowo odkłada Twoje niezapisane zmiany na bok, pozwalając na przełączenie gałęzi.
  - `git stash pop`: Przywraca ostatnio schowane zmiany i usuwa je ze schowka.
  - `git stash apply`: Przywraca zmiany, ale zostawia je w schowku.
- **`git cherry-pick <hash_commita>`**: Wybiera i kopiuje jeden konkretny commit z jednej gałęzi na drugą.

### Tagowanie i Wersjonowanie

Służy do oznaczania ważnych punktów w historii, np. wydań.

```bash
# Tworzy tag adnotowany (zalecane dla wydań)
git tag -a v1.0 -m "Wersja 1.0 - pierwsze stabilne wydanie"

# Wysyła tagi do zdalnego repozytorium (nie są wysyłane domyślnie!)
git push --tags
```

---

## Część 3: Poziom Eksperta

### Automatyzacja (`git hooks`)

Skrypty w `.git/hooks/` uruchamiane automatycznie w odpowiedzi na zdarzenia.

- **Client-Side (po stronie klienta):**
  - `pre-commit`: Uruchamiany przed utworzeniem commita (np. do uruchomienia lintera, formatera).
  - `commit-msg`: Do weryfikacji opisu commita.
  - `pre-push`: Uruchamiany przed `push` (np. do uruchomienia kluczowych testów).
- **Server-Side (po stronie serwera):**
  - `pre-receive`: Do weryfikacji polityk na serwerze przed przyjęciem zmian.
  - `post-receive`: Do zadań CI/CD (np. deployment, wysyłanie notyfikacji).

### Zaawansowana Diagnostyka Kodu

- **`git bisect`**: Automatyzuje poszukiwanie commita, który wprowadził błąd, używając wyszukiwania binarnego.
- **`git log` z opcjami**:
  - `-S "fragment_kodu"`: Wyszukuje commity, które wprowadziły lub usunęły dany ciąg znaków.
  - `--grep="JIRA-123"`: Przeszukuje opisy commitów.
  - `sciezka/do/pliku`: Pokazuje historię tylko dla danego pliku.

### Optymalizacja Pracy z Dużymi Repozytoriami

- **`git lfs` (Large File Storage)**: Zarządza dużymi plikami binarnymi (grafiki, wideo), przechowując w repozytorium tylko wskaźniki do nich.
- **`git clone --depth 1 <url>` (Shallow Clone)**: Pobiera tylko ostatni commit, drastycznie przyspieszając klonowanie.
- **`git sparse-checkout`**: Pozwala pobrać do katalogu roboczego tylko wybrane podkatalogi. Kluczowe w monorepach.

### Zarządzanie Złożonymi Zależnościami

- **`git submodule`**: Zagnieżdża inne repozytorium w Twoim, "zamrażając" je na konkretnym commicie.
- **`git subtree`**: Kopiuje kod i historię z innego repozytorium do Twojego, jako integralną część kodu.

---

## Część 4: Poziom Mistrzowski

### Filozofia i Model Myślowy Gita

- **Git to graf migawek (snapshots), a nie zbiór różnic (delt).** Każdy commit to pełny obraz projektu w danym momencie. To sprawia, że operacje są błyskawiczne.
- **Niezmienność (Immutability) jest kluczowa.** Operacje takie jak `rebase` czy `amend` **nie edytują** commitów. Tworzą **nowe commity** i przesuwają wskaźnik gałęzi. Zrozumienie tego wyjaśnia, dlaczego zmiana publicznej historii jest niebezpieczna.

### Zaawansowane Strategie Przepływu Pracy

- **Stacked Diffs / Stacked Pull Requests**: Zamiast jednego wielkiego PR, tworzysz łańcuch małych, zależnych od siebie PR-ów, co ułatwia i przyspiesza code review.
- **Strategie Łączenia**:
  - `--no-ff` (no fast-forward): Zawsze twórz merge commit, nawet jeśli to niekonieczne. Zachowuje kontekst istnienia gałęzi w historii.
  - `--squash`: Zbiera wszystkie commity z gałęzi w jeden, czysty commit na gałęzi docelowej. Idealne do ukrycia chaotycznej historii gałęzi `feature`.

### Higiena i Utrzymanie Repozytorium

- **`git gc` (Garbage Collector)**: Usuwa niedostępne obiekty i kompresuje repozytorium.
- **`git-filter-repo`**: Nowoczesne i bezpieczne narzędzie do całkowitego przepisywania historii (np. w celu usunięcia wrażliwych danych). Operacja ekstremalnie destrukcyjna.

### Bezpieczeństwo i Wiarygodność Historii

- **Podpisywanie commitów i tagów (GPG/SSH)**: Zapewnia, że commit rzeczywiście pochodzi od zaufanego autora. Platformy takie jak GitHub oznaczają takie commity jako "Verified".
