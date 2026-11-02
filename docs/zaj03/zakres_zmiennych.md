## Zakres zmiennych (scope)

Każda zmienna w Pythonie istnieje w określonym **zakresie** (ang. *scope*) - obszarze kodu, w którym jest dostępna. Python wyszukuje nazwy zmiennych według reguły **LEGB**:

1. **L**ocal - wnętrze bieżącej funkcji,
2. **E**nclosing - funkcja zewnętrzna (dla funkcji zagnieżdżonych),
3. **G**lobal - poziom modułu (skryptu),
4. **B**uilt-in - wbudowane nazwy Pythona, np. `print`, `len`.

```python
x = "globalny"

def zewnetrzna():
    x = "zewnętrzny"

    def wewnetrzna():
        x = "lokalny"
        print(x)  # lokalny

    wewnetrzna()
    print(x)  # zewnętrzny

zewnetrzna()
print(x)  # globalny
```

Python szuka `x` zawsze od najwęższego zakresu ku najszerszemu i zatrzymuje się przy pierwszym trafieniu.

Zmienne lokalne istnieją tylko w trakcie wykonywania funkcji - po jej zakończeniu nie da się do nich odwołać z zewnątrz:

```python
def oblicz():
    wynik = 42

oblicz()
# print(wynik)  # NameError: name 'wynik' is not defined
```

---

## Słowo kluczowe `global`

**Odczyt** zmiennej globalnej wewnątrz funkcji działa bez żadnych dodatkowych zabiegów:

```python
stawka_vat = 0.23

def cena_brutto(netto):
    return netto * (1 + stawka_vat)  # odczyt zmiennej globalnej - OK

print(cena_brutto(100))
```

Inaczej jest z **przypisaniem**. Wewnątrz funkcji przypisanie do nazwy (w tym `+=`) sprawia, że nazwa ta jest traktowana jako lokalna w całej funkcji, nawet jeśli istnieje zmienna globalna o tej samej nazwie. Żeby zmodyfikować zmienną globalną, trzeba ją jawnie zadeklarować słowem `global`.

```python
licznik = 0

def zwieksz():
    licznik += 1  # UnboundLocalError! Python widzi przypisanie i traktuje licznik jako lokalny

zwieksz()
```

Poprawne rozwiązanie:

```python
licznik = 0

def zwieksz():
    global licznik
    licznik += 1

zwieksz()
zwieksz()
print(licznik)  # 2
```

!!! warning "Ogranicz użycie `global`"
    Zmienne globalne utrudniają testowanie i śledzenie przepływu danych. Zamiast nich lepiej przekazywać wartości przez argumenty i zwracać przez `return`. `global` może być uzasadniony np. dla liczników lub flag w małych skryptach.

!!! note "A co z zakresem otaczającym (Enclosing)?"
    Modyfikowanie zmiennych funkcji zewnętrznej z wnętrza funkcji zagnieżdżonej (słowo kluczowe `nonlocal`) omówimy razem z domknięciami na kolejnych zajęciach.

---

## 📝 Zadania

Stwórz plik `python1course/zaj03/zakres_zmiennych.py` i wykonaj w nim poniższe zadania.

1. Uruchom poniższy kod i wyjaśnij wynik. Następnie popraw go tak, żeby funkcja `resetuj` rzeczywiście zerowała licznik globalny.

    ```python
    licznik = 10

    def resetuj():
        licznik = 0

    resetuj()
    print(licznik)
    ```

2. Poniższy kod działa, ale opiera się na zmiennej globalnej. Przepisz funkcje `wplac` i `wyplac` tak, żeby nie używały `global` - niech przyjmują aktualne saldo jako argument i zwracają nowe saldo. Funkcja `wyplac` powinna wypisać ostrzeżenie i zwrócić niezmienione saldo, jeśli po wypłacie byłoby ono ujemne.

    ```python
    saldo = 100

    def wplac(kwota):
        global saldo
        saldo += kwota

    def wyplac(kwota):
        global saldo
        saldo -= kwota

    wplac(50)
    wyplac(30)
    print(saldo)  # 120
    ```

    Po zmianach wywołanie powinno wyglądać tak:
    ```python
    saldo = 100
    saldo = wplac(saldo, 50)
    saldo = wyplac(saldo, 30)
    print(saldo)  # 120
    saldo = wyplac(saldo, 200)  # Ostrzeżenie: niewystarczające środki
    print(saldo)  # 120
    ```
