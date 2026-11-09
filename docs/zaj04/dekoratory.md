## Dekoratory

Dekoratory pozwalają **rozszerzyć działanie funkcji bez zmiany jej kodu** - np. dodać logowanie, pomiar czasu albo sprawdzanie uprawnień.

Dekorator to nic innego jak **funkcja przyjmująca funkcję jako argument i zwracająca nową funkcję**. To bezpośrednie zastosowanie [domknięć](domkniecia.md).

---

## Podstawowe działanie

```python
def przykladowy_dekorator(funkcja):
    def wrapper():
        print("Przed wywołaniem funkcji.")
        funkcja()
        print("Po wywołaniu funkcji.")
    return wrapper
```

Możemy zastosować dekorator ręcznie, przypisując wynik z powrotem do tej samej nazwy:

```python
def moja_funkcja():
    print("Oryginalna funkcja.")

moja_funkcja = przykladowy_dekorator(moja_funkcja)
moja_funkcja()
```

## Składnia z `@`

Znak `@` bezpośrednio nad definicją funkcji robi dokładnie to samo co ręczne przypisanie powyżej:

```python
@przykladowy_dekorator
def moja_funkcja():
    print("Oryginalna funkcja.")

moja_funkcja()
```

Zapis `@przykladowy_dekorator` jest równoważny `moja_funkcja = przykladowy_dekorator(moja_funkcja)`.

---

## Dekorator obsługujący argumenty dekorowanej funkcji

Żeby dekorator działał z dowolną funkcją, `wrapper` powinien przyjmować `*args` i `**kwargs`, przekazać je dalej i **zwrócić** wynik oryginalnej funkcji:

```python
def przykladowy_dekorator(funkcja):
    def wrapper(*args, **kwargs):
        print("Przed wywołaniem funkcji.")
        wynik = funkcja(*args, **kwargs)
        print("Po wywołaniu funkcji.")
        return wynik
    return wrapper

@przykladowy_dekorator
def dodaj(a, b):
    return a + b

print(dodaj(3, 5))  # 8 - bez return w wrapper wynik by zaginął
```

## Zachowanie nazwy i docstringa - `functools.wraps`

Bez dodatkowej pomocy udekorowana funkcja "udaje" `wrapper`: traci oryginalną nazwę i dokumentację.

```python
from functools import wraps

def przykladowy_dekorator(funkcja):
    @wraps(funkcja)
    def wrapper(*args, **kwargs):
        return funkcja(*args, **kwargs)
    return wrapper
```

`@wraps(funkcja)` kopiuje m.in. `__name__` i `__doc__` z oryginalnej funkcji na `wrapper`. Dzięki temu `help()` i komunikaty o błędach nadal pokazują prawdziwą nazwę.

## Dekorator z własnym argumentem

Jeśli dekorator ma przyjąć parametr (np. jednostkę czasu), pojawia się **trzeci** poziom zagnieżdżenia:

1. funkcja przyjmująca argumenty dekoratora (np. `unit`),
2. właściwy dekorator, przyjmujący funkcję,
3. `wrapper`, wywoływany zamiast oryginalnej funkcji.

Dlatego piszemy `@zmierz_czas(unit="seconds")` **z nawiasami** - najpierw wywołujemy `zmierz_czas(...)`, a to, co zwróci, dekoruje funkcję. Zwykły `@loguj` nawiasów nie potrzebuje, bo sam jest dekoratorem.

Do pomiaru krótkich odcinków czasu używamy `time.perf_counter()`, a nie `time.time()`. `perf_counter()` jest monotoniczny i ma wyższą rozdzielczość, więc nadaje się do benchmarków.

```python
import time
from functools import wraps

def zmierz_czas(unit="seconds"):
    def dekorator(funkcja):
        @wraps(funkcja)
        def wrapper(*args, **kwargs):
            start = time.perf_counter()
            wynik = funkcja(*args, **kwargs)
            elapsed = time.perf_counter() - start
            if unit == "microseconds":
                print(f"Czas wykonania {funkcja.__name__}: {elapsed * 1_000_000:.0f} µs")
            else:
                print(f"Czas wykonania {funkcja.__name__}: {elapsed:.4f} s")
            return wynik
        return wrapper
    return dekorator

@zmierz_czas(unit="microseconds")
def suma_kwadratow(n):
    return sum(i * i for i in range(n))

print(suma_kwadratow(10_000))
```

---

## Typowe zastosowania

??? - note "Logowanie"
    Automatyczne logowanie wywołań funkcji.

    ```python
    from functools import wraps

    def loguj(funkcja):
        @wraps(funkcja)
        def wrapper(*args, **kwargs):
            print(f"Wywołano: {funkcja.__name__} | args={args}, kwargs={kwargs}")
            return funkcja(*args, **kwargs)
        return wrapper
    ```

??? - note "Sprawdzanie warunku przed wywołaniem"
    Dekorator może przerwać wywołanie, zgłaszając wyjątek.

    ```python
    from functools import wraps

    def wymaga_dodatniej(funkcja):
        @wraps(funkcja)
        def wrapper(x):
            if x <= 0:
                raise ValueError("Argument musi być dodatni")
            return funkcja(x)
        return wrapper
    ```

!!! note "Dekorowanie klas"
    Dekorator można też zastosować do całej klasy. Wymaga to znajomości programowania obiektowego, dlatego wrócimy do tego później.

---

## 📝 Zadania

Stwórz plik `python1course/zaj04/dekoratory.py` i wykonaj w nim poniższe zadania.

1. Napisz dekorator `loguj`, który przed wywołaniem funkcji wypisuje jej nazwę oraz przekazane argumenty, a po wywołaniu zwraca jej wynik. Użyj `*args`, `**kwargs` i `functools.wraps`. Przetestuj go na funkcji przyjmującej argumenty, np. `dodaj(a, b)`.

2. **(Dodatkowe)** Napisz dekorator `zmierz_czas`, który mierzy i wypisuje czas wykonania dekorowanej funkcji.

    Wymagania:

    - przyjmuje argument `unit` o wartości `'seconds'` albo `'microseconds'`,
    - mierzy czas wykonania (`time.perf_counter()`),
    - wypisuje czas po zakończeniu wywołania,
    - działa z funkcjami o dowolnej liczbie argumentów (`*args`, `**kwargs`),
    - zachowuje nazwę i docstring dekorowanej funkcji (`functools.wraps`).

    ```python
    @zmierz_czas(unit="microseconds")
    def wolna_funkcja():
        import time
        time.sleep(0.1)

    wolna_funkcja()
    # Czas wykonania wolna_funkcja: 100523 µs
    ```
