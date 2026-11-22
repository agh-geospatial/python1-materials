## Funkcje jako obiekty pierwszej klasy

W Pythonie funkcje są **pełnoprawnymi obiektami** - tak samo jak liczby, listy czy słowniki. Oznacza to, że można je:

- przypisywać do zmiennych,
- przechowywać w kolekcjach (listach, słownikach),
- przekazywać jako argumenty do innych funkcji,
- zwracać z innych funkcji.

Nawiasy `()` **wywołują** funkcję. Bez nawiasów operujesz na samym obiekcie funkcji.

### Przypisanie funkcji do zmiennej

```python
def powitaj(imie):
    return f"Cześć, {imie}!"

przywitaj = powitaj  # przypisanie referencji - bez nawiasów!
print(przywitaj("Ania"))     # Cześć, Ania!
print(powitaj is przywitaj)  # True - obie zmienne wskazują na ten sam obiekt
```

### Funkcja jako argument

Możemy przekazać funkcję do innej funkcji, tak jak każdą inną wartość:

```python
def zastosuj(funkcja, wartosc):
    return funkcja(wartosc)

def podwoj(x):
    return x * 2

def kwadrat(x):
    return x ** 2

print(zastosuj(podwoj, 5))   # 10
print(zastosuj(kwadrat, 5))  # 25
print(zastosuj(str.upper, "python"))  # PYTHON
```

To wzorzec powszechnie spotykany w bibliotekach - np. `sorted(lista, key=...)` czy `map(funkcja, iterable)`. Argument `key` to właśnie funkcja, którą Python wywołuje dla każdego elementu.

### Funkcje w kolekcjach

Słownik funkcji pozwala wybrać operację po nazwie, zamiast pisać długi łańcuch `if/elif`:

```python
def dodaj(a, b):
    return a + b

def odejmij(a, b):
    return a - b

def pomnoz(a, b):
    return a * b

operacje = {
    "dodaj": dodaj,
    "odejmij": odejmij,
    "pomnoz": pomnoz,
}

nazwa = "dodaj"
print(operacje[nazwa](3, 4))  # 7
```

### Atrybuty funkcji

Funkcja, jak każdy obiekt, może mieć własne atrybuty. Odwołujemy się do nich notacją kropkową:

```python
def sample_function():
    return "Hello, world!"

sample_function.description = "To jest przykładowa funkcja."
print(sample_function.description)
print(sample_function.__name__)  # sample_function - nazwa nadana przy def
```

---

## 📝 Zadania

Stwórz plik `python1course/zaj04/funkcje_jako_obiekty.py` i wykonaj w nim poniższe zadania.

1. Napisz funkcję `zastosuj_do_listy(funkcja, lista)`, która zwraca nową listę z wynikami zastosowania `funkcja` do każdego elementu. Nie używaj `map()` - zaimplementuj to ręcznie pętlą. Przetestuj z co najmniej trzema różnymi funkcjami, np. `str.upper`, `len` i własną funkcją.

2. Napisz funkcję `zastosuj_kolejno(wartosc, funkcje)`, która przyjmuje wartość początkową i **listę funkcji**, a następnie aplikuje je po kolei (wynik jednej jest argumentem następnej). Przetestuj na przykład:

    ```python
    def dodaj_vat(cena):
        return cena * 1.23

    def zaokraglij(cena):
        return round(cena, 2)

    print(zastosuj_kolejno(100, [dodaj_vat, zaokraglij]))  # 123.0
    ```
