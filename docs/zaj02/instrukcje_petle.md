# Instrukcje warunkowe i pętle w Pythonie

## Wprowadzenie
W Pythonie istnieje kilka podstawowych mechanizmów sterowania przepływem programu. Najważniejsze z nich to:

- **Instrukcje warunkowe (`if`, `elif`, `else`)** - pozwalają na podejmowanie decyzji w kodzie,
- **Pętle (`for`, `while`)** - umożliwiają wielokrotne wykonanie kodu,
- **Instrukcje sterujące (`break`, `continue`, `pass`)** - dają większą kontrolę nad działaniem pętli.

!!! note "Wcięcia"
    W Pythonie bloki kodu (np. wnętrze `if` czy pętli) wyznacza **wcięcie**, a nie nawiasy klamrowe. Przyjęło się stosować 4 spacje. Linia rozpoczynająca blok kończy się dwukropkiem `:`.

---

## Instrukcje warunkowe (`if`, `elif`, `else`)
Instrukcje warunkowe pozwalają na wykonanie określonego kodu tylko wtedy, gdy spełniony jest dany warunek.

### Składnia `if`
```python
x = 10
if x > 5:
    print("x jest większe niż 5")
```
Jeśli warunek jest prawdziwy (`True`), wykonywany jest kod znajdujący się wewnątrz bloku `if`.

### Składnia `if-else`
```python
x = 3
if x > 5:
    print("x jest większe niż 5")
else:
    print("x jest mniejsze lub równe 5")
```
Blok `else` wykona się, gdy warunek w `if` nie zostanie spełniony.

### Składnia `if-elif-else`
Jeśli mamy więcej niż dwa przypadki, możemy użyć `elif`:
```python
x = 7
if x > 10:
    print("x jest większe niż 10")
elif x > 5:
    print("x jest większe niż 5, ale nie większe niż 10")
else:
    print("x jest mniejsze lub równe 5")
```

---

## Operatory porównania
Wynikiem porównania jest wartość typu `bool` (`True` lub `False`):

| Operator | Znaczenie |
|---|---|
| `==` | równe |
| `!=` | różne |
| `<`, `>` | mniejsze, większe |
| `<=`, `>=` | mniejsze lub równe, większe lub równe |
| `in`, `not in` | czy element występuje w sekwencji (lub kluczach słownika) |

!!! warning "`=` a `==`"
    Pojedynczy znak `=` to przypisanie, a podwójny `==` to porównanie. `if x = 5:` jest błędem składni.

## Operatory logiczne w warunkach
Warunki mogą być łączone przy użyciu operatorów logicznych:

- `and` - oba warunki muszą być prawdziwe,
- `or` - przynajmniej jeden warunek musi być prawdziwy,
- `not` - negacja warunku.

Przykład:
```python
x = 8
y = 15
if x > 5 and y < 20:
    print("Oba warunki są spełnione")

owoce = ["jabłko", "banan"]
if "kiwi" not in owoce:
    print("Brak kiwi")
```

---

## Wczytywanie danych od użytkownika
Funkcja `input()` wyświetla komunikat i czeka, aż użytkownik wpisze tekst i naciśnie Enter. Zawsze zwraca **łańcuch znaków** - aby wykonywać obliczenia, trzeba go zamienić na liczbę:
```python
imie = input("Jak masz na imię? ")
wiek = int(input("Ile masz lat? "))  # bez int() wiek byłby tekstem, np. "21"
print(f"{imie}, za rok będziesz mieć {wiek + 1} lat")
```

---

## Pętle w Pythonie
Pętle pozwalają na wielokrotne wykonanie danego fragmentu kodu.

### Pętla `for`
Pętla `for` przechodzi po kolei przez elementy sekwencji (listy, krotki, łańcucha znaków) lub innego obiektu iterowalnego:
```python
imiona = ["Jan", "Maria", "Piotr"]
for imie in imiona:
    print(imie)

for litera in "Python":
    print(litera)
```

### Pętle z licznikiem - `range()`
`range()` tworzy sekwencję liczb całkowitych, najczęściej używaną do kontrolowania liczby powtórzeń:

- `range(n)` - od `0` do `n-1`,
- `range(start, stop)` - od `start` do `stop-1`,
- `range(start, stop, krok)` - co `krok` elementów.

```python
for i in range(5):
    print(f"Iteracja numer {i}")  # 0, 1, 2, 3, 4

for i in range(2, 11, 2):
    print(i)  # 2, 4, 6, 8, 10
```

### Indeks i wartość - `enumerate()`
Funkcja `enumerate()` zwraca w każdym kroku parę (indeks, wartość), którą od razu rozpakowujemy do dwóch zmiennych:
```python
owoce = ["jabłko", "banan", "wiśnia"]
for idx, owoc in enumerate(owoce):
    print(f"Indeks: {idx}, Owoc: {owoc}")

for nr, owoc in enumerate(owoce, start=1):  # numerowanie od 1
    print(f"{nr}. {owoc}")
```

### Modyfikowanie elementów listy
Zmienna pętli `for` jest tylko kopią referencji do elementu - przypisanie do niej nowej wartości nie zmienia listy. Aby zmodyfikować elementy, iterujemy po indeksach `range(len(x))`:
```python
owoce = ["jabłko", "banan", "pomarańcza"]
for i in range(len(owoce)):
    owoce[i] = owoce[i].upper()
print(owoce)  # ['JABŁKO', 'BANAN', 'POMARAŃCZA']
```

### Przechodzenie co n-ty element
```python
liczby = [1, 2, 3, 4, 5, 6]
for i in range(0, len(liczby), 2):
    print(liczby[i])  # 1, 3, 5

# Alternatywnie, z użyciem wycinków
for liczba in liczby[::2]:
    print(liczba)
```

### Przechodzenie równoległe - `zip()`
`zip()` umożliwia jednoczesne iterowanie po kilku sekwencjach - w każdym kroku dostajemy krotkę z odpowiadającymi sobie elementami:
```python
imiona = ["Anna", "Jan", "Piotr"]
wieki = [25, 30, 35]
for imie, wiek in zip(imiona, wieki):
    print(f"{imie} ma {wiek} lat")
```

### Iterowanie po słowniku
Iterowanie po słowniku zwraca jego klucze. Metoda `.items()` zwraca pary (klucz, wartość):
```python
osoba = {"imie": "Jan", "wiek": 30}
for klucz in osoba:
    print(f"{klucz}: {osoba[klucz]}")

for klucz, wartosc in osoba.items():
    print(f"{klucz}: {wartosc}")
```

### Zbieranie wyników w pętli
Częsty wzorzec to zmienna (lub kolekcja) utworzona przed pętlą i uzupełniana w każdym kroku:
```python
liczby = [4, 8, 15, 16, 23, 42]
suma = 0
parzyste = []
for liczba in liczby:
    suma += liczba
    if liczba % 2 == 0:
        parzyste.append(liczba)
print(suma, parzyste)  # 108 [4, 8, 16, 42]
```

---

### Pętla `while`
Pętla `while` wykonuje kod tak długo, jak długo warunek jest prawdziwy:
```python
x = 0
while x < 5:
    print(f"x = {x}")
    x += 1
```

Pętle `while` mogą prowadzić do nieskończonego wykonania, jeśli warunek nigdy nie stanie się fałszywy. Celowo nieskończoną pętlę `while True` przerywa się instrukcją `break`:
```python
while True:
    odpowiedz = input("Czy chcesz zakończyć? (tak/nie): ")
    if odpowiedz == "tak":
        break  # Przerwanie pętli
```

!!! tip "Program się zawiesił?"
    Działający program (np. z nieskończoną pętlą) można przerwać w terminalu skrótem `Ctrl+C`.

---

## Instrukcje sterujące `break`, `continue`, `pass`
**`break` - przerywanie pętli**
```python
for liczba in range(10):
    if liczba == 5:
        break  # Przerywa pętlę, gdy liczba wynosi 5
    print(liczba)
```

**`continue` - pominięcie iteracji**
```python
for liczba in range(5):
    if liczba == 2:
        continue  # Pomija tylko tę iterację
    print(liczba)
```

**`pass` - pusta instrukcja (placeholder)**

Czasem musimy zostawić pusty blok kodu bez błędu składni:
```python
if True:
    pass  # Tymczasowy kod
```

---

## 📝 Zadania

Stwórz plik `python1course/zaj02/instrukcje_petle.py` i wykonaj w nim poniższe zadania.

1. Stwórz przykłady dla testów `if`:
    - Gdzie wystąpią dwa warunki - napisz program sprawdzający czy dana liczba jest dodatnia i parzysta. Jeśli tak, program powinien wydrukować `Liczba jest dodatnia i parzysta`
    - Gdzie wykorzystane zostanie zaprzeczenie `not` lub `!=` - napisz program, który sprawdza, czy wprowadzona przez użytkownika liczba nie jest równa zero. Jeśli nie jest, wydrukuj `Liczba jest różna od zera`
    - Gdzie wykorzystane będzie słowo `in` - napisz program, który sprawdza, czy wprowadzony przez użytkownika owoc znajduje się na liście dostępnych owoców (np. `['jabłko', 'banan', 'pomarańcza']`). Jeśli tak, program powinien wydrukować `Owoc jest dostępny`

2. Mając dwie listy, `imiona = ['Anna', 'Jan', 'Ewa']` i `oceny = [5, 4, 3]`, użyj `zip` do stworzenia pary każdego imienia z odpowiadającą mu oceną. Następnie, iteruj przez te pary, wyświetlając imię wraz z oceną. Co się stanie, jeśli listy będą miały różne długości?

3. Stwórz grę w zgadywanie liczby. Program losuje liczbę z zakresu 1-100, a użytkownik próbuje ją odgadnąć. Program powinien informować, czy liczba jest za duża czy za mała, aż użytkownik trafi. Zliczaj liczbę prób i wypisz ją na końcu.

    !!! tip "Wskazówka"
        Do losowania liczby użyj `random.randint(1, 100)` z modułu `random`. Pamiętaj, że `input()` zwraca tekst - przed porównaniem zamień go na liczbę funkcją `int()`.

4. Masz listę trójkątów reprezentowanych jako krotki: `[(3, 4, 5), (5, 12, 13), (7, 8, 9), (8, 15, 17)]`, gdzie każda krotka to (bok_a, bok_b, bok_c), a `bok_c` jest najdłuższy. Używając `enumerate`, dla każdego trójkąta:

    - sprawdź, czy jest prostokątny (a² + b² = c²),
    - policz jego pole ze wzoru Herona: `P = √(s·(s-a)·(s-b)·(s-c))`, gdzie `s = (a+b+c)/2`.

    Wypisz indeks, boki i pole tylko dla trójkątów prostokątnych.

    !!! tip "Wskazówka"
        Pierwiastek kwadratowy obliczysz funkcją `math.sqrt()` z modułu `math`. Krotkę z bokami możesz od razu rozpakować w pętli: `for i, (a, b, c) in enumerate(trojkaty):`.
