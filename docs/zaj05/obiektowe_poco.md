Programowanie obiektowe (*OOP*, ang. *object-oriented programming*) to paradygmat, w którym dane i zachowania dotyczące jednego bytu trzymamy razem, w **obiekcie**. Obiekt reprezentuje konkretną rzecz (samochód, użytkownika, pomiar), a **klasa** jest przepisem, według którego takie obiekty powstają.

Dla kierunku analizy danych OOP nie jest jedynym dobrym narzędziem. Skrypt, który wczytuje CSV, liczy średnią i rysuje wykres, często lepiej zostawić proceduralnie. Klasy zaczynają się opłacać, gdy ten sam rodzaj danych pojawia się w wielu miejscach i ma własne reguły.

## Zalety

- **Grupowanie danych i logiki** - atrybuty i metody dotyczące jednego bytu są w jednym miejscu.
- **Ponowne użycie** - klasę można importować i tworzyć z niej wiele instancji.
- **Dziedziczenie** - nowa klasa może bazować na istniejącej, zamiast kopiować kod.
- **Polimorfizm** - różne klasy mogą mieć metodę o tej samej nazwie i różnym działaniu, a kod wywołujący nie musi znać konkretnego typu.

## Wady

- **Wymaga projektu** - przy małym skrypcie narzut klas bywa zbędny.
- **Złożoność** - hierarchie klas łatwo rozrosnąć ponad to, czego problem naprawdę potrzebuje.
- **Nie zawsze pasuje do danych** - przy przetwarzaniu tabel i wektorów często wygodniejsze są funkcje, listy i biblioteki w rodzaju pandas.

---

## Przykład: podejście proceduralne vs. obiektowe

Te same dane i logika na dwa sposoby. Wersja obiektowa zaczyna się opłacać, gdy rośnie liczba operacji na tym samym bycie.

```python
# -- Podejście proceduralne --
def oblicz_pole_prostokata(szerokosc, wysokosc):
    return szerokosc * wysokosc

def skaluj_prostokat(szerokosc, wysokosc, wspolczynnik):
    return szerokosc * wspolczynnik, wysokosc * wspolczynnik

s, h = 4, 3
print(oblicz_pole_prostokata(s, h))   # 12
s, h = skaluj_prostokat(s, h, 2)
print(oblicz_pole_prostokata(s, h))   # 48
```

```python
# -- Podejście obiektowe --
class Prostokat:
    def __init__(self, szerokosc, wysokosc):
        self.szerokosc = szerokosc
        self.wysokosc = wysokosc

    def pole(self):
        return self.szerokosc * self.wysokosc

    def skaluj(self, wspolczynnik):
        self.szerokosc *= wspolczynnik
        self.wysokosc *= wspolczynnik

r = Prostokat(4, 3)
print(r.pole())    # 12
r.skaluj(2)
print(r.pole())    # 48
```

W wersji obiektowej wymiary i operacje są zgrupowane. Kolejną metodę (np. `obwod`) dopisujesz do klasy, zamiast dodawać następną funkcję i pamiętać, które zmienne do niej podać.
