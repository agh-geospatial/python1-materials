## Wyjątki

Są to błędy, które występują podczas wykonywania programu i mogą powodować jego nagłe zakończenie, jeśli nie są odpowiednio obsłużone.

Zarządzanie wyjątkami, czyli kontrola nad tym, jak program zachowuje się w sytuacji, gdy wystąpi błąd, obejmuje wykorzystanie słów kluczowych `try`, `except`, `else` i `finally` oraz ręczne wywoływanie wyjątków za pomocą `raise`.

### Jak czytać komunikat o błędzie?

Nieobsłużony wyjątek kończy program i wypisuje tzw. **traceback**:

```
Traceback (most recent call last):
  File "skrypt.py", line 5, in <module>
    print(podziel(10, 0))
  File "skrypt.py", line 2, in podziel
    return a / b
ZeroDivisionError: division by zero
```

Czytamy go **od dołu**: ostatnia linia to typ wyjątku (`ZeroDivisionError`) i jego opis, a linie powyżej pokazują, w którym pliku, linii i funkcji wystąpił błąd oraz skąd ta funkcja została wywołana.

---

## Podstawowa obsługa wyjątków

Podstawowy blok obsługi wyjątków składa się z `try` i `except`.

- W bloku `try` umieszczamy kod, który może wywołać wyjątek,
- a blok `except` przechwytuje ten wyjątek i wykonuje odpowiednie działania.

```python
try:
    liczba = int(input("Podaj liczbę: "))
    wynik = 10 / liczba
    print(f"Wynik: {wynik}")
except ZeroDivisionError:
    print("Nie można dzielić przez zero!")
except ValueError:
    print("To nie jest poprawna liczba!")
```

Gdy w bloku `try` wystąpi wyjątek, reszta tego bloku jest pomijana, a Python wykonuje pierwszy pasujący blok `except`. Kilka typów wyjątków można też obsłużyć wspólnie, podając je w krotce:

```python
try:
    liczba = int(input("Podaj liczbę: "))
    wynik = 10 / liczba
except (ValueError, ZeroDivisionError):
    print("Podaj liczbę różną od zera!")
```

Można też użyć klasy `Exception`, żeby złapać praktycznie każdy błąd, a zapis `as e` pozwala odczytać jego treść:

```python
try:
    liczba = int(input("Podaj liczbę: "))
    wynik = 10 / liczba
    print(f"Wynik: {wynik}")
except Exception as e:
    print(f"Wystąpił błąd: {e}")
```

!!! warning "Uważaj z `except Exception`"
    `except Exception` przechwytuje wszystkie "zwykłe" błędy programu, ale **nie** przechwytuje np. `KeyboardInterrupt` (przerwanie programu przez `Ctrl+C`) ani `SystemExit` - dziedziczą one bezpośrednio po `BaseException`, a nie po `Exception`. Mimo to łapanie wszystkiego naraz ukrywa błędy, których się nie spodziewamy - lepiej wymieniać konkretne typy wyjątków, które umiemy sensownie obsłużyć.

---

## Dodatkowe bloki `else` i `finally`

- Kod w bloku `else` wykona się tylko wtedy, gdy **żaden wyjątek nie wystąpił** w bloku `try`.
- Blok `finally` wykona się **zawsze**, niezależnie od tego, czy wyjątek wystąpił. Używany głównie do zwalniania zasobów (zamykanie połączeń, zapis logów itp.).

```python
try:
    liczba = int(input("Podaj liczbę: "))
    wynik = 10 / liczba
except ZeroDivisionError:
    print("Nie można dzielić przez zero!")
except ValueError:
    print("To nie jest poprawna liczba!")
else:
    print(f"Wynik: {wynik}")
finally:
    print("Blok finally wykonuje się zawsze.")
```

---

## Ręczne wywoływanie wyjątków - `raise`

Funkcje powinny aktywnie zgłaszać wyjątki, gdy otrzymują niepoprawne dane - zamiast cicho zwracać błędne wyniki. To właśnie widziałeś już w sekcji o docstringach (`raise ValueError`).

```python
def sprawdz_wiek(wiek):
    if wiek < 18:
        raise ValueError("Wiek musi być co najmniej 18 lat.")
    return "Dostęp dozwolony"

try:
    print(sprawdz_wiek(15))
except ValueError as e:
    print(f"Błąd: {e}")
```

`raise` natychmiast przerywa działanie funkcji (podobnie jak `return`), a wyjątek "wędruje" w górę do miejsca wywołania - aż trafi na pasujący blok `except` lub zakończy program.

Większość wyjątków to klasy wbudowane dostępne w standardowej bibliotece - pełna lista w [dokumentacji](https://docs.python.org/3/library/exceptions.html#).

??? - tip "Najczęściej wykorzystywane wyjątki"
    - `ValueError` - argument ma poprawny typ, ale niepoprawną wartość.
    - `TypeError` - argument jest niepoprawnego typu.
    - `IndexError` - indeks jest poza zakresem listy, krotki itp.
    - `KeyError` - klucz nie istnieje w słowniku.
    - `AttributeError` - obiekt nie ma określonego atrybutu.
    - `NameError` - użyto nazwy, która nie została zdefiniowana.
    - `FileNotFoundError` - określony plik nie istnieje.
    - `ZeroDivisionError` - próba dzielenia przez zero.
    - `PermissionError` - brak uprawnień do wykonania operacji.
    - `ConnectionError` - błąd połączenia sieciowego.
    - `RuntimeError` - wyjątek ogólny, gdy żaden inny nie pasuje.
    - `NotImplementedError` - metoda zamierzona do implementacji w przyszłości.
    - `OverflowError` - wynik operacji matematycznej zbyt duży do przetworzenia.
    - `ImportError` - nie udało się zaimportować modułu lub elementu.
    - `AssertionError` - sprawdzenie `assert` zakończyło się niepowodzeniem.

!!! note "Własne wyjątki"
    Python pozwala też tworzyć własne typy wyjątków, dopasowane do konkretnej aplikacji. Wymaga to znajomości klas, dlatego wrócimy do tego na zajęciach z programowania obiektowego.

---

## 📝 Zadania

Stwórz plik `python1course/zaj03/wyjatki.py` i wykonaj w nim poniższe zadania.

1. Napisz funkcję `konwertuj_na_int(tekst)`, która próbuje zamienić podany ciąg znaków na liczbę całkowitą. Jeśli się nie uda, powinna wypisać czytelny komunikat o błędzie i zwrócić `None`. Przetestuj ją na kilku przypadkach: `"42"`, `"abc"`, `"3.14"`, `""`.

2. Napisz funkcję `oblicz_bmi(masa, wzrost)`, która:
    - Zgłasza `TypeError`, jeśli któryś z argumentów nie jest liczbą,
    - Zgłasza `ValueError`, jeśli masa ≤ 0 lub wzrost ≤ 0,
    - W pozostałych przypadkach zwraca wynik (`masa / wzrost ** 2`).

    Napisz kod wywołujący, który testuje wszystkie przypadki błędów z użyciem `try/except`.

    !!! tip "Wskazówka"
        Czy wartość jest liczbą, sprawdzisz przez `isinstance(masa, (int, float))` - drugi argument może być krotką typów.

3. Napisz funkcję `wczytaj_plik(sciezka)`, która otwiera plik tekstowy i zwraca jego zawartość jako string. Użyj bloku `with open()`. Obsłuż wyjątki `FileNotFoundError` i `PermissionError` - w obu przypadkach wypisz czytelny komunikat i zwróć `None`.

4. **(Dodatkowe)** Napisz funkcję `polacz_z_baza(host)`, która symuluje nawiązywanie połączenia z bazą danych. Użyj bloku `finally`, żeby zawsze wypisać komunikat o zakończeniu próby połączenia - niezależnie od tego, czy się powiodła. Zasymuluj błąd dla wybranych hostów (np. `raise ConnectionError("Nie można połączyć")` gdy `host == "zly-host"`).

    !!! tip "Kiedy `finally` ma sens?"
        `with` rozwiązuje problem zamykania plików i innych zasobów obsługujących protokół kontekstu. `finally` przydaje się wszędzie tam, gdzie tego protokołu nie ma - np. połączenia sieciowe, logi, liczniki prób, zwalnianie blokad.

5. **(Dodatkowe)** Napisz funkcję `pobierz_wartosc(slownik, klucz)`, która zwraca wartość ze słownika dla podanego klucza. Jeśli klucz nie istnieje, nie powinna rzucić `KeyError` - zamiast tego niech zgłosi `ValueError` z komunikatem wskazującym, jakiego klucza brakuje i jakie klucze są dostępne.
