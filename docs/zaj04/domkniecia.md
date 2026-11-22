## Zagnieżdżone funkcje i domknięcia (closures)

Funkcja może być zdefiniowana wewnątrz innej funkcji. Taka **funkcja zagnieżdżona** ma dostęp do zmiennych z zakresu funkcji zewnętrznej - nawet po zakończeniu jej wykonania. Ten mechanizm nazywamy **domknięciem** (ang. *closure*).

To właśnie czwarty punkt z listy "co można zrobić z funkcją": **zwrócić funkcję z funkcji**.

```python
def stworz_mnoznik(n):
    def mnoz(x):
        return x * n  # n pochodzi z zewnętrznej funkcji i jest "zapamiętane"
    return mnoz

razy3 = stworz_mnoznik(3)
razy5 = stworz_mnoznik(5)

print(razy3(10))  # 30
print(razy5(10))  # 50
```

`razy3` i `razy5` to dwa osobne domknięcia - każde pamięta swoją wartość `n`. Można to potwierdzić:

```python
print(razy3.__closure__[0].cell_contents)  # 3
print(razy5.__closure__[0].cell_contents)  # 5
```

### Praktyczne zastosowanie: fabryki funkcji

Domknięcia pozwalają tworzyć **sparametryzowane funkcje** bez powtarzania kodu:

```python
def stworz_walidator(min_val, max_val):
    def waliduj(x):
        return min_val <= x <= max_val
    return waliduj

ocena_szkolna = stworz_walidator(1, 6)
temperatura = stworz_walidator(-50, 60)

print(ocena_szkolna(5))   # True
print(ocena_szkolna(7))   # False
print(temperatura(100))   # False
```

---

## Słowo kluczowe `nonlocal`

Na [zajęciach 3](../zaj03/zakres_zmiennych.md) widzieliśmy, że przypisanie wewnątrz funkcji tworzy zmienną lokalną, a do modyfikacji zmiennej globalnej potrzeba `global`.

`nonlocal` działa analogicznie, ale dotyczy zmiennej z **najbliższej otaczającej funkcji** (nie globalnej). Jest niezbędne, gdy funkcja zagnieżdżona ma **zmienić** zmienną z funkcji zewnętrznej, a nie tylko ją odczytać.

```python
def zewnetrzna():
    licznik = 0

    def wewnetrzna():
        nonlocal licznik
        licznik += 1

    wewnetrzna()
    wewnetrzna()
    print(licznik)  # 2

zewnetrzna()
```

Bez `nonlocal` przypisanie `licznik += 1` wewnątrz `wewnetrzna` rzuciłoby `UnboundLocalError`, bo Python potraktowałby `licznik` jako zmienną lokalną.

Odczyt bez przypisania działa i bez `nonlocal` - tak jak w `stworz_mnoznik`, gdzie `mnoz` tylko **czyta** `n`. `nonlocal` jest potrzebne dopiero wtedy, gdy zagnieżdżona funkcja robi przypisanie (`=`, `+=` itd.).

```python
def stworz_licznik():
    stan = 0

    def zwieksz():
        nonlocal stan
        stan += 1
        return stan

    return zwieksz

licznik = stworz_licznik()
print(licznik())  # 1
print(licznik())  # 2
```

Każde wywołanie `stworz_licznik()` tworzy **osobne** domknięcie z własnym `stan`:

```python
a = stworz_licznik()
b = stworz_licznik()
print(a())  # 1
print(a())  # 2
print(b())  # 1 - b ma własny stan
```

---

## 📝 Zadania

Stwórz plik `python1course/zaj04/domkniecia.py` i wykonaj w nim poniższe zadania.

1. Napisz funkcję `stworz_potege(n)`, która zwraca funkcję podnoszącą liczbę do potęgi `n`. Stwórz na jej podstawie `kwadrat` i `szescian`, a następnie przetestuj je.

    ```python
    kwadrat = stworz_potege(2)
    szescian = stworz_potege(3)
    print(kwadrat(4))   # 16
    print(szescian(3))  # 27
    ```

2. Napisz funkcję `stworz_licznik()`, która używa `nonlocal` do zarządzania wewnętrznym stanem. Funkcja powinna zwracać dwie inne funkcje: `zwieksz` i `pobierz`, które odpowiednio inkrementują i odczytują wewnętrzny licznik.

    ```python
    zwieksz, pobierz = stworz_licznik()
    zwieksz()
    zwieksz()
    zwieksz()
    print(pobierz())  # 3
    ```

    Stwórz dwa niezależne liczniki i upewnij się, że zwiększanie jednego nie zmienia drugiego.

3. **(Dodatkowe)** Napisz funkcję `stworz_konto(saldo_poczatkowe)`, która zwraca trzy funkcje: `wplac(kwota)`, `wyplac(kwota)` i `sprawdz()`. Każda z nich operuje na wewnętrznym saldzie konta poprzez `nonlocal`. Funkcja `wyplac` powinna wypisać ostrzeżenie i nie zmieniać salda, jeśli po wypłacie byłoby ono ujemne.

    ```python
    wplac, wyplac, sprawdz = stworz_konto(100)
    wplac(50)
    wyplac(30)
    print(sprawdz())  # 120
    wyplac(200)       # Ostrzeżenie: niewystarczające środki
    print(sprawdz())  # 120
    ```
