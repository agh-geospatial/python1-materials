## Wprowadzenie
W Pythonie istnieje kilka podstawowych typów wbudowanych, które pozwalają na przechowywanie i manipulowanie różnymi rodzajami danych. Do najważniejszych należą:

- **Liczby (int, float, complex)** - typy liczbowe,
- **Łańcuchy znaków (str)** - sekwencje znaków,
- **Listy (list)** - uporządkowane kolekcje elementów,
- **Krotki (tuple)** - niemutowalne kolekcje elementów,
- **Słowniki (dict)** - struktury danych przechowujące pary klucz-wartość,
- **Zbiory (set)** - nieuporządkowane kolekcje unikalnych elementów.

---

## Liczby
### Typy liczbowe w Pythonie
Python obsługuje kilka typów liczbowych:
```python
# Liczba całkowita
x = 10  # int

# Liczba zmiennoprzecinkowa
y = 3.14  # float

# Liczba zespolona
z = 1 + 2j  # complex
```

???- warning "Dokładność liczb zmiennoprzecinkowych"
    **Dokładność liczb zmiennoprzecinkowych**
    
    Warto pamiętać, że liczby zmiennoprzecinkowe (`float`) są reprezentowane w standardzie IEEE 754, co może prowadzić do błędów zaokrągleń:
    ```python
    print(0.1 + 0.2)  # Wynik może być 0.30000000000000004 zamiast 0.3
    ```
    Aby uniknąć problemów precyzji, można używać modułu `decimal`:
    ```python
    from decimal import Decimal

    x = Decimal('0.1')
    y = Decimal('0.2')
    print(x + y)  # Wynik: 0.3 (dokładnie)
    ```

    **Zakres i precyzja**
    
    Liczby całkowite (`int`) w Pythonie mają dowolną precyzję - są ograniczone jedynie dostępną pamięcią.
    
    Liczby zmiennoprzecinkowe (`float`) to zawsze liczby podwójnej precyzji (64 bity, IEEE 754), niezależnie od tego, czy system jest 32- czy 64-bitowy. Dają one około 15-17 cyfr znaczących, a największa możliwa wartość to około `1.8e308`. Szczegóły można sprawdzić przez `sys.float_info`:
    ```python
    import sys
    print(sys.float_info.max)  # 1.7976931348623157e+308
    print(sys.float_info.dig)  # 15
    ```

    Jeśli kluczowa jest precyzja obliczeń, warto rozważyć użycie `decimal.Decimal` lub `fractions.Fraction`.

### Operacje matematyczne
Python obsługuje podstawowe operacje matematyczne:
```python
a = 10
b = 3.5

suma = a + b
roznica = a - b
iloczyn = a * b
iloraz = a / b           # dzielenie zawsze zwraca float
dzielenie_calk = a // 3  # dzielenie całkowite (z zaokrągleniem w dół)
potega = a ** 2
modulo = a % 3           # reszta z dzielenia
```

### Moduły matematyczne
Bardziej zaawansowane funkcje znajdują się w modułach biblioteki standardowej, np. `math` i `random`:
```python
import math
import random

print(math.sqrt(16))                # Pierwiastek kwadratowy
print(math.e)                       # Stała e
print(random.randint(1, 10))        # Losowa liczba całkowita z zakresu 1-10
print(random.choice(["a", "b"]))    # Losowy element z listy
```

### Rzutowanie typów
Typ wartości sprawdzamy funkcją `type()`, a zamieniamy funkcjami o nazwach typów:
```python
x = 3.99
print(type(x))   # <class 'float'>
print(int(x))    # 3 (część ułamkowa jest odcinana)
print(str(x))    # '3.99'
print(float("2.5") + 1)  # 3.5
```

---

## Łańcuchy znaków
Łańcuchy znaków w Pythonie są niezmienne i można się do nich odwoływać jak do sekwencji:
```python
tekst = "Witaj, świecie!"
print(tekst[0])  # Pierwszy znak
print(tekst[-1])  # Ostatni znak
print(tekst[0:5])  # Fragment łańcucha
print(len(tekst))  # Długość łańcucha
```

Możemy również manipulować łańcuchami. Metody nie zmieniają oryginalnego łańcucha, tylko zwracają nowy - aby zachować wynik, trzeba go przypisać do zmiennej:
```python
print(tekst.upper())  # Wielkie litery
print(tekst.lower())  # Małe litery
print(tekst.replace("Witaj", "Hello"))  # Zamiana fragmentu

tekst = tekst.upper()  # Nadpisanie zmiennej nowym łańcuchem
```

Próba zmiany pojedynczego znaku kończy się błędem, ponieważ łańcuchy są niemutowalne:
```python
# tekst[0] = "w"  # TypeError: 'str' object does not support item assignment
```

### f-stringi
Poprzedzenie łańcucha literą `f` pozwala wstawiać do niego wartości zmiennych i wyrażeń w nawiasach klamrowych:
```python
imie = "Ala"
wiek = 21
print(f"{imie} ma {wiek} lat, za rok będzie miała {wiek + 1}")
```

!!! tip "Jakie metody ma dany obiekt?"
    Funkcja `dir(obiekt)` zwraca listę wszystkich atrybutów i metod obiektu, a `help(obiekt.metoda)` wyświetla jej opis.

---

## Listy
Listy są dynamicznymi kolekcjami elementów różnych typów:
```python
lista = [1, 2, 3, "Python", 3.14]
print(lista[2])  # Dostęp do elementu

lista.append("Nowy element")  # Dodanie elementu
lista.remove(3)  # Usunięcie elementu
print(lista)
```

### Operacje na listach
#### Wycinki (slicing)
Listy obsługują operacje wycinania (slicing):
```python
lista = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(lista[2:5])  # Elementy od indeksu 2 do 4
print(lista[:4])   # Pierwsze 4 elementy
print(lista[5:])   # Wszystkie elementy od indeksu 5
print(lista[-3:])  # Ostatnie 3 elementy
print(lista[::2])  # Co drugi element
```

#### Odwracanie listy
```python
print(lista[::-1])  # Odwrócenie kolejności elementów
```

#### Sprawdzanie obecności elementu
Operator `in` zwraca wartość typu `bool` (`True` lub `False`):
```python
lista = [1, 2, "Python"]
print("Python" in lista)  # True
print(5 in lista)         # False
```

---

## Słowniki
Słowniki przechowują dane w postaci par klucz-wartość:
```python
osoba = {
    "imie": "Jan",
    "nazwisko": "Kowalski",
    "wiek": 30
}
print(osoba["imie"])  # Dostęp do wartości

osoba["miasto"] = "Warszawa"  # Dodanie nowej pary
print(osoba)

pusty = {}                    # Pusty słownik
print(osoba.keys())           # Wszystkie klucze
print(osoba.values())         # Wszystkie wartości
print("wiek" in osoba)        # Sprawdzenie, czy klucz istnieje: True
print(osoba.get("adres"))     # Bezpieczny dostęp - None zamiast błędu
```

Wartościami w słowniku mogą być dowolne obiekty, także listy i inne słowniki. Do zagnieżdżonych elementów odwołujemy się, łącząc kolejne indeksy i klucze:
```python
student = {
    "imie": "Ola",
    "kursy": [
        {"nazwa": "Python", "ocena": 5},
        {"nazwa": "GIS", "ocena": 4},
    ],
}
print(student["kursy"][1]["nazwa"])  # GIS
```

---

## Krotki
Krotki działają podobnie do list, ale są niemutowalne:
```python
krotka = (1, 2, "Python", 3.14, 2)
print(krotka[1])
print(len(krotka))       # Liczba elementów: 5
print(krotka.count(2))   # Liczba wystąpień wartości 2: 2
print(krotka.index("Python"))  # Indeks pierwszego wystąpienia: 2
```
Próba modyfikacji krotki spowoduje błąd:
```python
# krotka[1] = 5  # TypeError: 'tuple' object does not support item assignment
```

---

## Zbiory
Zbiory przechowują unikalne wartości i pozwalają na operacje matematyczne:
```python
zbior1 = {1, 2, 3, 4}
zbior2 = {3, 4, 5, 6}

print(zbior1 & zbior2)  # Część wspólna
print(zbior1 | zbior2)  # Suma zbiorów
print(zbior1 - zbior2)  # Różnica zbiorów
```

Zbiór można utworzyć z dowolnej sekwencji funkcją `set()` - duplikaty zostaną usunięte:
```python
print(set([1, 1, 2, 3, 3]))  # {1, 2, 3}
print(set("banan"))          # {'b', 'a', 'n'} - kolejność może być inna
pusty = set()                # Uwaga: {} tworzy pusty słownik, a nie zbiór
```

---

## 📝 Zadania
1. W katalogu `./python1course/zaj01` stwórz nowy plik o nazwie `skrypt2.py`. Wszystkie kolejne instrukcje z tego rozdziału wykonuj właśnie w tym pliku.

!!! tip "Pamiętaj, że dobrą praktyką jest importowanie modułów i pakietów na samym początku pliku"

!!! tip "Na różnych etapach tych zadań wykorzystaj funkcję `type()`, która pozwala zauważyć, jak działa **dynamiczne typowanie** w Pythonie"

**Działania matematyczne**

1. Stwórz zmienną `wartosc` i przypisz jej liczbę 100
2. Do zmiennej `dodawanie` przypisz wartość dodania do zmiennej `wartosc` liczby `123.15`
3. Stwórz zmienną `potega` i przypisz do niej zmienną `dodawanie` podniesioną do potęgi `12345`

    ???- warning "Pojawia Ci się błąd?"
        Jest to spodziewane, zamień potęgę na `12`.

4. Do zmiennej `tekst` przypisz rzutowanie wartości zmiennej `potega` na typ `string` (funkcja `str()`) 
5. Do zmiennej `wartosc_pi` przypisz wartość liczby pi (skorzystaj z modułu `math`)
6. Do zmiennej `losowa` przypisz losową wartość z listy [1,2,3,4,5] (skorzystaj z modułu `random`)

**Łańcuchy znaków**

1. Nadpisz zmienną `tekst` następującym wyrażeniem: `tekst = f"Wartosc: {tekst}"`
2. Wyświetl długość tekstu w zmiennej `tekst` i później wykorzystując wycinki wyświetl część zmiennej `tekst` o wartości „art”
3. Wypisz wartość funkcji `dir(tekst)`
4. Zmień cały łańcuch znaków w zmiennej `tekst` na wielkie litery, przypisz wynik z powrotem do zmiennej `tekst` i wypisz
5. Spróbuj zamienić znak na pozycji 2 w łańcuchu w zmiennej `tekst` na znak p

    ???- warning "Pojawia Ci się błąd?"
        Jest to spodziewane, zapoznaj się z błędem i usuń lub zakomentuj tą linię kodu.

**Listy**

Działania na listach: 

1. Stwórz zmienną o nazwie `lista`, przypisz do niej rzutowanie na listę (funkcja `list()`) zmiennej `tekst`. 
2. Wykorzystując wycinki zrób tak, żeby lista zawierała jedynie litery słowa `WARTOSC` i później dwukropek.
3. Do listy dodaj kolejny wyraz, niech będzie to kolejna lista `[1,2,3,4,5]`. 
4. Z listy usuń wyraz, który jest dwukropkiem. 
5. Wypisz zmienną lista.

**Słowniki**

1. Stwórz pusty słownik o nazwie `ja`
2. Niech będzie to reprezentacja Twojej osoby, dodaj do niego klucze `imie`, `nazwisko`, `wiek`, `moje_hobby` (reprezentowane przez listę z 2 zagnieżdżonymi słownikami o 2 kluczach: `nazwa` i `dlaczego`)
3. Wypisz wartość klucza `moje_hobby`
4. Wypisz jedynie nazwę pierwszego hobby 
5. Wypisz wszystkie klucze naszego słownika
6. Sprawdź czy nasz słownik posiada klucz `adres`, wypisz zmienną typu `boolean`

**Krotki**

1. Do zmiennej `krotka1` przypisz wartość `(1,2,"3",4,2,5)`
2. Wypisz długość zmiennej i pierwszy wyraz
3. Sprawdź, ile razy występuje wartość 2 i wypisz
4. Spróbuj zmienić pierwszy wyraz na wartość 2

    ???- warning "Pojawia Ci się błąd?"
        Jest to spodziewane, zapoznaj się z błędem i usuń lub zakomentuj tą linię kodu.

**Zbiory**

1. Stwórz dwa zbiory o nazwach `X` i `Y`, nadaj im wartości odpowiednio: `set("kalarepa")` oraz `set("lepy")`
2. Wyświetl część wspólną obu zbiorów - można na nich wykonywać podobne operacje jak na zbiorach matematycznych