## Funkcje anonimowe - lambda

Są to **krótkie, anonimowe funkcje**, które można definiować bez nadawania im nazwy. Służą głównie do wykonywania prostych operacji, zwłaszcza wtedy, gdy funkcja jest potrzebna tylko w jednym, konkretnym miejscu. Zamiast pełnej definicji z `def` używamy słowa kluczowego `lambda`.

Lambda to zwykły obiekt funkcji - można ją przekazać jako argument, schować w słowniku albo przypisać do zmiennej, tak samo jak funkcję z `def`.

### Podstawowa składnia

```python
# Tak wygląda standardowo zdefiniowana funkcja
def add(x, y):
    return x + y
print(add(3, 5))  # 8

# A tak lambda
add = lambda x, y: x + y
print(add(3, 5))  # 8
```

Ogólny zapis: `lambda argumenty: wyrażenie`. Ciało lambdy to **jedno wyrażenie** - nie wstawisz tam kilku instrukcji, pętli ani `return`.

Słownik operacji z poprzedniej strony można więc zapisać krócej:

```python
operacje = {
    "dodaj":   lambda a, b: a + b,
    "odejmij": lambda a, b: a - b,
    "pomnoz":  lambda a, b: a * b,
}

print(operacje["dodaj"](3, 4))  # 7
```

### Lambda z `sorted()`

```python
osoby = [
    {"imie": "Anna", "wiek": 30},
    {"imie": "Jan", "wiek": 25},
    {"imie": "Piotr", "wiek": 35},
]

posortowane = sorted(osoby, key=lambda x: x["wiek"])
print(posortowane)  # najpierw Jan (25), potem Anna (30), potem Piotr (35)
```

`sorted()` zwraca **nową** listę. Argument `key` dostaje funkcję, która dla każdego elementu zwraca wartość, według której sortujemy.

### Lambda z `filter()`

`filter(funkcja, sekwencja)` zostawia tylko te elementy, dla których `funkcja` zwraca wartość prawdziwą. Wynikiem jest iterator - żeby dostać listę, owijamy go w `list()`:

```python
liczby = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
parzyste = list(filter(lambda x: x % 2 == 0, liczby))
print(parzyste)  # [2, 4, 6, 8, 10]
```

To samo da się zapisać listą składaną: `[x for x in liczby if x % 2 == 0]`. Wybór jest kwestią czytelności.

### Lambda z `map()`

`map(funkcja, sekwencja)` aplikuje funkcję do każdego elementu. Też zwraca iterator:

```python
liczby = [1, 2, 3, 4, 5]
kwadraty = list(map(lambda x: x ** 2, liczby))
print(kwadraty)  # [1, 4, 9, 16, 25]
```

Odpowiednik listą składaną: `[x ** 2 for x in liczby]`.

### Lambda z `max()` / `min()`

```python
teksty = ["Python", "Java", "C++", "JavaScript"]
najdluzszy = max(teksty, key=lambda x: len(x))
print(najdluzszy)  # JavaScript

osoby = [{"imie": "Anna", "wiek": 30}, {"imie": "Jan", "wiek": 25}]
najstarszy = max(osoby, key=lambda x: x["wiek"])
print(najstarszy)  # {'imie': 'Anna', 'wiek': 30}
```

### Wielowierszowe lambdy? Nie.

Lambda powinna być prosta i jednolinijkowa:

```python
# Dobrze - prosta operacja
square = lambda x: x ** 2

# Nie rób tego - lambda nie powinna zawierać print ani sztuczek z or
warto_wykorzystac_def = lambda x: print(x) or x
```

!!! tip "Kiedy używać lambda?"
    - Do prostych transformacji w `map()`, `filter()`, `sorted()`.
    - Gdy funkcja jest potrzebna tylko raz.
    - Do sortowania według niestandardowego klucza.

!!! warning "Kiedy nie używać lambda?"
    - Jeśli logika jest skomplikowana - użyj `def`.
    - Gdy funkcja jest dłuższa niż jedno wyrażenie.
    - Gdy potrzebujesz dokumentacji (docstring).
    - Jeśli funkcja będzie używana wielokrotnie - nazwij ją.

## 📝 Zadania

Stwórz plik `python1course/zaj04/lambda.py` i wykonaj w nim poniższe zadania.

1. Masz słownik operacji matematycznych. Uzupełnij go lambdami, a następnie napisz funkcję `kalkulator(a, op, b)`, która pobiera dwie liczby i nazwę operacji (łańcuch znaków), wykonuje ją i zwraca wynik. Jeśli podana operacja nie istnieje, zgłoś `ValueError` z czytelnym komunikatem.

    ```python
    operacje = {"dodaj": ..., "odejmij": ..., "pomnoz": ..., "podziel": ...}
    ```

2. Masz daną listę słowników reprezentujących książki. Każdy słownik zawiera klucze: `tytul`, `autor` oraz `rok_wydania`.

    ```python
    ksiazki = [
        {"tytul": "Władca Pierścieni", "autor": "J.R.R. Tolkien", "rok_wydania": 1954},
        {"tytul": "Harry Potter i Kamień Filozoficzny", "autor": "J.K. Rowling", "rok_wydania": 1997},
        {"tytul": "Duma i uprzedzenie", "autor": "Jane Austen", "rok_wydania": 1813},
        {"tytul": "Rok 1984", "autor": "George Orwell", "rok_wydania": 1949},
        {"tytul": "Zbrodnia i kara", "autor": "Fiodor Dostojewski", "rok_wydania": 1866},
        {"tytul": "Mistrz i Małgorzata", "autor": "Michaił Bułhakow", "rok_wydania": 1967},
        {"tytul": "Hobbit", "autor": "J.R.R. Tolkien", "rok_wydania": 1937},
        {"tytul": "Sto lat samotności", "autor": "Gabriel García Márquez", "rok_wydania": 1967},
        {"tytul": "Imię róży", "autor": "Umberto Eco", "rok_wydania": 1980},
        {"tytul": "Solaris", "autor": "Stanisław Lem", "rok_wydania": 1961},
    ]
    ```

    Przy użyciu funkcji lambda wykonaj:

    - sortowanie książek rosnąco według roku wydania (`sorted`),
    - filtrację książek wydanych po roku **1950** (`filter`) - wynik zamień na listę,
    - transformację listy do listy samych tytułów (`map`) - wynik zamień na listę.

3. Masz listę uczniów: `["Anna Kowalska", "Jan Nowak", "Piotr Wiśniewski", "Ewa Zielińska"]`. Napisz kod, który:

    - uszereguje uczniów alfabetycznie według nazwiska (drugiego słowa),
    - stworzy listę samych inicjałów (np. `"AK"` dla `"Anna Kowalska"`).

    Użyj `sorted()` i `map()` z funkcjami lambda.

    !!! tip "Wskazówka"
        Metoda `.split()` dzieli łańcuch na listę wyrazów. Inicjały to pierwsze znaki kolejnych wyrazów, złożone w jeden łańcuch.
