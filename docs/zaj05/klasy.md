## Klasa

**Szablon** (przepis), który definiuje strukturę i zachowanie swoich instancji. Klasa określa, jakie **atrybuty** (cechy) i **metody** (działania) będą miały obiekty do niej należące. Przykładowo klasa `Samochod` może mieć atrybuty `marka`, `model`, `rok` oraz metody `przyspiesz()` i `hamuj()`.

```python
class Samochod:
    def __init__(self, marka, model, rok):
        self.marka = marka
        self.model = model
        self.rok = rok
```

Słowo `class` rozpoczyna definicję. `__init__` to metoda wywoływana automatycznie przy tworzeniu instancji - ustawia początkowy stan obiektu. Nie zwraca wartości (a raczej zwraca `None`); nowy obiekt tworzy Python.

Pierwszy parametr metod instancji nazywa się zwyczajowo `self` i oznacza **ten konkretny obiekt**. Przy wywołaniu `moj_samochod.przyspiesz(20)` Python sam przekazuje instancję jako `self`, a `20` trafia do `wartosc`.

## Instancja

Konkretny egzemplarz klasy, stworzony na podstawie jej definicji. Instancje tej samej klasy mają ten sam zestaw atrybutów, ale własne wartości.

```python
moj_samochod = Samochod("Toyota", "Corolla", 2020)
print(type(moj_samochod))  # <class '__main__.Samochod'>
```

Wywołanie klasy jak funkcji (`Samochod(...)`) tworzy instancję i uruchamia `__init__`.

## Atrybuty

Przechowują stan obiektu. W Pythonie wyróżniamy dwa rodzaje.

### Atrybuty instancji

Definiowane w `__init__` przez `self`. Każda instancja ma własne, niezależne wartości.

```python
class Samochod:
    def __init__(self, marka, model, rok):
        self.marka = marka
        self.model = model
        self.rok = rok

moj_samochod = Samochod("Toyota", "Corolla", 2020)
inny_samochod = Samochod("Ford", "Focus", 2018)

print(moj_samochod.marka)   # Toyota
print(inny_samochod.marka)  # Ford
```

### Atrybuty klasy

Definiowane bezpośrednio w ciele klasy, poza `__init__`. Są **współdzielone** przez wszystkie instancje.

```python
class Samochod:
    liczba_kol = 4  # atrybut klasy - wspólny dla wszystkich

    def __init__(self, marka):
        self.marka = marka  # atrybut instancji

auto1 = Samochod("Toyota")
auto2 = Samochod("Ford")

print(auto1.liczba_kol)  # 4
print(auto2.liczba_kol)  # 4

Samochod.liczba_kol = 6  # zmiana przez klasę - widać u wszystkich instancji
print(auto1.liczba_kol)  # 6
print(auto2.liczba_kol)  # 6
```

!!! warning "Pułapka"
    Przypisanie przez instancję (`auto1.liczba_kol = 3`) **nie zmienia** atrybutu klasy. Tworzy nowy atrybut instancji, który przesłania atrybut klasy tylko dla `auto1`. `auto2.liczba_kol` nadal bierze wartość z klasy.

## Metody

Funkcje zdefiniowane wewnątrz klasy. Operują na instancji przez `self` i mogą odczytywać albo zmieniać jej stan.

```python
class Samochod:
    def __init__(self, marka, model, rok):
        self.marka = marka
        self.model = model
        self.rok = rok

    def przyspiesz(self, wartosc=10):
        print(f"{self.marka} {self.model} przyspiesza o {wartosc}!")

moj_samochod = Samochod("Toyota", "Corolla", 2020)
moj_samochod.przyspiesz(20)
# Toyota Corolla przyspiesza o 20!
```

Instancję trzeba utworzyć **z tej definicji klasy**, która już ma metodę `przyspiesz`. Ponowne napisanie `class Samochod` w tym samym pliku zastępuje poprzednią klasę, ale stare instancje zostają przy starej definicji i nowej metody nie będą miały.

Więcej rodzajów metod (`@property`, a na kolejnych zajęciach `@classmethod` i `@staticmethod`) jest na stronie [Metody i właściwości](metody.md).
