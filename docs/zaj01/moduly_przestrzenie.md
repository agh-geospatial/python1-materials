## Wprowadzenie
W Pythonie moduły i przestrzenie nazw odgrywają kluczową rolę w organizacji kodu. Moduły pozwalają na podzielenie kodu na mniejsze, łatwiejsze w zarządzaniu części, a przestrzenie nazw pomagają uniknąć konfliktów pomiędzy nazwami zmiennych, funkcji czy klas.

---

## Moduły w Pythonie

### Czym jest moduł?
Moduł w Pythonie to plik `.py`, który zawiera zmienne, funkcje i klasy. Dzięki nim można podzielić kod na niezależne sekcje, co ułatwia jego ponowne użycie oraz utrzymanie.

#### Tworzenie i używanie modułu
Załóżmy, że chcemy stworzyć moduł do operacji matematycznych. Tworzymy plik `matematyka.py`:
```python
def dodaj(a, b):
    return a + b

def odejmij(a, b):
    return a - b
```

!!! note "Czym jest `def`?"
    Słowo kluczowe `def` definiuje **funkcję** - nazwany blok kodu, który można wielokrotnie wywoływać, przekazując mu argumenty (tutaj `a` i `b`). Instrukcja `return` zwraca wynik funkcji. Ciało funkcji wyróżnia się wcięciem. Funkcje omówimy szczegółowo na kolejnych zajęciach - na razie wystarczy umieć je wywołać, np. `dodaj(5, 3)`.

Teraz możemy zaimportować go w innym skrypcie:
```python
import matematyka

wynik = matematyka.dodaj(5, 3)
print(f'Wynik dodawania: {wynik}')
```

Możemy też importować tylko wybrane funkcje:
```python
from matematyka import odejmij

wynik = odejmij(10, 4)
print(f'Wynik odejmowania: {wynik}')
```

Moduł można też zaimportować pod krótszą nazwą (aliasem):
```python
import matematyka as m

print(m.dodaj(2, 2))
```

### Moduły i pakiety
**Pakiet** to katalog grupujący wiele modułów (np. `json`, `urllib` czy zewnętrzny `numpy`). Wiele narzędzi z biblioteki standardowej to jednak pojedyncze moduły, np. `os`, `time`, `math`, `random` czy `datetime`. Oba rodzaje importuje się w ten sam sposób.

### Ponowny import modułu
Kod modułu jest wykonywany przy jego **pierwszym** imporcie w danym uruchomieniu programu. Kolejne instrukcje `import` tego samego modułu nie wykonują jego kodu ponownie. Aby wymusić ponowne wykonanie kodu modułu, można go przeładować funkcją `reload()` z modułu `importlib`:
```python
import importlib
import matematyka

importlib.reload(matematyka)
```

!!! warning "Uwaga"
    `importlib.reload()` przyjmuje obiekt modułu, więc działa z `import matematyka`, a nie z `from matematyka import dodaj`.

---

## Przestrzenie nazw

Przestrzeń nazw to mechanizm pozwalający unikać konfliktów między zmiennymi i funkcjami. Szukając nazwy, Python przeszukuje przestrzenie nazw w ustalonej kolejności (reguła **LEGB**):

1. **Lokalna** (Local) - zmienne utworzone wewnątrz funkcji,
2. **Otaczająca** (Enclosing) - zmienne funkcji zewnętrznej, gdy jedna funkcja jest zdefiniowana w drugiej,
3. **Globalna** (Global) - zmienne utworzone na najwyższym poziomie modułu (pliku),
4. **Wbudowana** (Built-in) - standardowe funkcje Pythona jak `print()` czy `len()`.

Każdy moduł ma własną przestrzeń globalną - dlatego do funkcji z zaimportowanego modułu odwołujemy się przez jego nazwę, np. `matematyka.dodaj`.

#### Przykład użycia przestrzeni nazw
```python
x = 10  # Zmienna globalna

def funkcja():
    x = 5  # Zmienna lokalna
    print(f'Wartość x wewnątrz funkcji: {x}')

funkcja()
print(f'Wartość x na poziomie globalnym: {x}')
```

## Podsumowanie
1. **Moduły** pomagają organizować kod i unikać powielania funkcji.
2. **Przestrzenie nazw** zapobiegają konfliktom nazw i organizują zmienne w hierarchii.

Dzięki tym mechanizmom Python zapewnia elastyczność w organizacji kodu i jego efektywne zarządzanie!

## 📝 Zadania

Na potrzeby tego rozdziału pracuj w katalogu `./python1course/zaj01`. Utwórz w nim pliki `skrypt1.py` oraz `czas.py` zgodnie z poniższymi punktami.

  1. W pliku `skrypt1.py`, z modułu `os` zaimportuj funkcję `getcwd`
     ```python
     from os import getcwd
     ```
  2. Wywołaj funkcję i przypisz wynik do zmiennej `current_path`
  3. Wypisz wartość zmiennej `current_path`
  4. W pliku `czas.py` dodaj zmienną `aktualny_czas`:
     ```python
     from datetime import datetime
     aktualny_czas = datetime.now()
     ```
  5. Zaimportuj moduł `czas` w `skrypt1.py` (`import czas`)
  6. Wypisz wartość zmiennej `aktualny_czas` (`czas.aktualny_czas`)
  7. Zaimportuj moduł `time` i dodaj opóźnienie:
     ```python
     import time
     time.sleep(20)
     ```
  8. Ponownie wypisz wartość zmiennej `aktualny_czas`
  9. Przeładuj moduł `czas` (`importlib.reload(czas)`)
  10. Po raz trzeci wypisz wartość zmiennej `aktualny_czas`

Zwróć uwagę na to, kiedy zmieniła się wartość zmiennej. Zastanów się, kiedy inicjowana jest zmienna `aktualny_czas`. 