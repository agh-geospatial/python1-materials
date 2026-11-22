## Wstęp

Dziedziczenie pozwala tworzyć nową klasę **na bazie istniejącej**. Klasa pochodna przejmuje atrybuty i metody klasy bazowej, a potem może je uzupełnić albo nadpisać.

W nawiasie po nazwie klasy podajesz klasę bazową. `super()` odwołuje się do niej - najczęściej po to, żeby wywołać jej `__init__` i nie duplikować ustawiania wspólnych atrybutów.

```python
class Pojazd:
    def __init__(self, marka):
        self.marka = marka

    def uruchom(self):
        print(f"{self.marka} jest uruchamiany.")

class Samochod(Pojazd):
    def __init__(self, marka, model):
        super().__init__(marka)
        self.model = model

auto = Samochod("Toyota", "Corolla")
auto.uruchom()          # Toyota jest uruchamiany.
print(auto.model)       # Corolla
```

`isinstance(auto, Samochod)` i `isinstance(auto, Pojazd)` dadzą oba `True`: instancja klasy pochodnej jest też instancją klasy bazowej. `type(auto) == Pojazd` da `False`. Dlatego przy sprawdzaniu typu (np. w `__eq__` karetki) używaj `isinstance`.

Jeśli nazwa nie istnieje ani w klasie pochodnej, ani w bazowej, Python zgłasza `AttributeError`.

## Nadpisywanie (*overriding*)

Proces, w którym klasa pochodna definiuje własną wersję metody o tej samej nazwie. Może całkowicie zastąpić działanie z klasy bazowej albo je rozszerzyć, wołając `super()`.

```python
class Pojazd:
    def uruchom(self):
        print("Pojazd jest uruchamiany.")

class Samochod(Pojazd):
    def uruchom(self):
        print("Samochód jest uruchamiany szybciej!")
        super().uruchom()

moj_samochod = Samochod()
moj_samochod.uruchom()
# Samochód jest uruchamiany szybciej!
# Pojazd jest uruchamiany.
```

`Employee.__init__(self, ...)` też zadziała, ale `super()` jest zalecane.
