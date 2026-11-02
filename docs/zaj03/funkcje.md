## Wstęp

Funkcje pozwalają na organizowanie i strukturyzowanie kodu w logiczne bloki, które można wielokrotnie wywoływać. Dzięki funkcjom możemy **uprościć programy**, **zmniejszyć ilość powtarzającego się kodu**, a także sprawić, że nasze rozwiązania staną się bardziej **modularne** i **łatwiejsze do utrzymania**.

Zalety używania funkcji:

- **Modularność** - dzielisz duży problem na mniejsze części, które są łatwiejsze do zarządzania.

- **Ponowne wykorzystanie** - funkcję można wywoływać wielokrotnie w różnych miejscach programu.

- **Łatwiejsze utrzymanie** - zmiana logiki w jednym miejscu (w funkcji) automatycznie wprowadza zmiany w całym programie.

- **Czytelność** - funkcje pomagają tworzyć bardziej zrozumiały i uporządkowany kod.

```python
import random

def generuj_losowa(seed=None):
    # Ustawienie ziarna (seed) generatora liczb losowych
    if seed is not None:
        random.seed(seed)
    # Generowanie losowej liczby z zakresu od 0 do 100
    return random.randint(0, 100)

liczba = generuj_losowa(seed=42)
print(f"Wygenerowana liczba: {liczba}")
```

Trzy kluczowe elementy każdej funkcji:

1. Słowo kluczowe służące definiowaniu funkcji - `def`.

2. Argumenty: definiowane i podawane wewnątrz `()`.

3. Zwracane wartości - słowo kluczowe `return`. Funkcja bez `return` (lub z samym `return`) zwraca `None`.

## Docstringi

Docstring to ciąg znaków umieszczony bezpośrednio po nagłówku funkcji (lub klasy czy modułu) służący jako jej dokumentacja. Python udostępnia go przez atrybut `__doc__` i wyświetla w `help()`.

```python
def oblicz_bmi(masa: float, wzrost: float) -> float:
    """Oblicza wskaźnik BMI na podstawie masy i wzrostu.

    Args:
        masa: masa ciała w kilogramach.
        wzrost: wzrost w metrach.

    Returns:
        Wartość BMI zaokrąglona do 2 miejsc po przecinku.

    Raises:
        ValueError: jeśli wzrost jest równy zero.
    """
    if wzrost == 0:
        raise ValueError("Wzrost nie może być zerem.")
    return round(masa / wzrost ** 2, 2)

print(oblicz_bmi.__doc__)
help(oblicz_bmi)
```

Instrukcja `raise` celowo zgłasza błąd i przerywa działanie funkcji - wyjątki omówimy dokładnie na kolejnych zajęciach. Zapis `masa: float` i `-> float` to adnotacje typów, opisane na końcu tej strony.

!!! tip "Konwencja"
    Najczęściej stosowane style docstringów to **Google style** (jak powyżej) i **NumPy style**. Ważne, żeby w ramach jednego projektu trzymać się jednego stylu.

!!! warning "Docstring ≠ komentarz"
    Komentarz (`#`) jest pomijany przez interpreter i nie jest dostępny programistycznie. Docstring jest pełnoprawnym obiektem tekstowym przypisanym do funkcji - można go odczytać w czasie działania programu.

## 📝 Zadania

Stwórz plik `python1course/zaj03/funkcje.py` i wykonaj w nim poniższe zadania.

1. Napisz funkcję `zmien_wartosc(arg)`, która przyjmuje jeden argument i próbuje zmodyfikować ten argument w różny sposób w zależności od tego, czy jest on niemutowalny (w tym przypadku integerem) czy mutowalny (w tym przypadku listą).

    - Jeśli jest listą, wykonaj `arg[0] = 'kalafior'`.

    - Jeśli jest integerem, wykonaj `arg = 65482652`.

    Wypisz przykłady dla obu przypadków, wypisz wartości przed i po wykonaniu funkcji. Jak się zachowują te obiekty?

    !!! tip
        Typ argumentu sprawdzisz funkcją `isinstance()`, która zwraca `True` lub `False`, np. `isinstance(arg, list)` albo `isinstance(arg, int)`.

??? - note "Teoria: mutowalne i niemutowalne obiekty w funkcjach"
    Kiedy zmienne są przekazywane do funkcji jako argumenty, Python nie tworzy ich kopii, lecz przekazuje referencję do oryginalnego obiektu - parametr funkcji to po prostu nowa nazwa wskazująca na ten sam obiekt. To, co się stanie, zależy od tego, co z tą nazwą zrobimy:

    **Modyfikacja obiektu w miejscu** (możliwa tylko dla obiektów mutowalnych, np. list, słowników):

    Operacje takie jak `arg[0] = ...` czy `arg.append(...)` zmieniają sam obiekt, więc zmiana jest widoczna również poza funkcją.

    **Przypisanie do parametru** (`arg = ...`):

    Przypisanie tworzy jedynie nowe powiązanie lokalnej nazwy `arg` z innym obiektem. Oryginalny obiekt poza funkcją pozostaje nietknięty - niezależnie od tego, czy był mutowalny, czy nie. Obiektów niemutowalnych (np. liczb, napisów, krotek) nie da się zmienić w miejscu, więc funkcja nie ma jak zmienić wartości przekazanej zmiennej.

## Dopasowywanie argumentów

Funkcje mogą przyjmować argumenty na różne sposoby, co umożliwia elastyczne przekazywanie danych. Kluczowe elementy to: **argumenty pozycyjne**, **argumenty nazwane**, **wartości domyślne**, oraz specjalne operatory **`*args`** i **`**kwargs`**, które pozwalają na przekazywanie zmiennej liczby argumentów.

```python
# dodanie wartości domyślnych
def dodaj(a=0, b=0):
    return a + b

# argumenty pozycyjne przekazywane są w podanej kolejności
print(dodaj(3, 5))

# argumenty nazwane można podawać w dowolnej kolejności
print(dodaj(b=5, a=3))

# brakujące argumenty przyjmują wartości domyślne
print(dodaj(3))  # 3
```

**`*args` - zmienna liczba argumentów pozycyjnych**

Wszystkie dodatkowe argumenty są zbierane w **krotkę**, dzięki czemu możemy obsłużyć więcej argumentów, niż zdefiniowano w sygnaturze funkcji.

```python
def suma(*liczby):
    return sum(liczby)

print(suma(1, 2, 3))
print(suma(10, 20))
```

**`**kwargs` - zmienna liczba argumentów nazwanych**

Argumenty są zbierane w **słownik**, co umożliwia przekazanie większej liczby argumentów nazwanych, niż przewidziano w sygnaturze funkcji.

```python
def przedstaw_sie(**dane):
    for klucz, wartosc in dane.items():
        print(f"{klucz}: {wartosc}")

przedstaw_sie(imie="Jan", wiek=30, miasto="Kraków")
```

**Argumenty tylko nazwane (`*`)**

Argumenty zdefiniowane po samej gwiazdce `*` (lub po `*args`) można przekazać **wyłącznie w formie nazwanej**. Przydaje się to, gdy chcemy wymusić czytelne wywołania:

```python
def przelew(kwota, *, waluta, tytul=""):
    return f"Przelew {kwota} {waluta} - {tytul}"

print(przelew(100, waluta="PLN", tytul="czynsz"))  # OK
# przelew(100, "PLN")  # TypeError: przelew() takes 1 positional argument but 2 were given
```

### Pułapka mutowalnych wartości domyślnych

Wartości domyślne argumentów są **ewaluowane raz** w momencie definiowania funkcji, nie przy każdym wywołaniu. Użycie mutowalnego obiektu (np. listy) jako wartości domyślnej prowadzi do nieoczekiwanych zachowań:

```python
def dodaj_element(element, lista=[]):  # ŹLE
    lista.append(element)
    return lista

print(dodaj_element(1))  # [1]
print(dodaj_element(2))  # [1, 2] - lista przetrwała między wywołaniami!
print(dodaj_element(3))  # [1, 2, 3]
```

Poprawne rozwiązanie - użyj `None` jako wartości domyślnej i twórz nową listę wewnątrz funkcji:

```python
def dodaj_element(element, lista=None):  # DOBRZE
    if lista is None:
        lista = []
    lista.append(element)
    return lista

print(dodaj_element(1))  # [1]
print(dodaj_element(2))  # [2] - za każdym razem nowa lista
```

??? - danger "Mieszane użycie argumentów nie zawsze jest możliwe"
    W definicji funkcji obowiązuje kolejność: **argumenty pozycyjne bez wartości domyślnych, argumenty z wartościami domyślnymi, `*args` (lub sama `*`), argumenty tylko nazwane, a na końcu `**kwargs`**.

    ```python
    def funkcja_mieszana(a, b=10, *args, **kwargs):
        print(f"a: {a}, b: {b}")
        print(f"Argumenty dodatkowe (args): {args}")
        print(f"Argumenty nazwane (kwargs): {kwargs}")

    funkcja_mieszana(1, 2, 3, 4, imie="Ania", wiek=25)
    funkcja_mieszana(1, 2, 3, imie="Jan")  # też poprawne: args=(3,), kwargs={'imie': 'Jan'}
    ```

    Oraz kilka niepoprawnych wywołań:

    ```python
    funkcja_mieszana()                     # TypeError - brakuje argumentu a
    funkcja_mieszana(1, 2, 3, 4, 5, a=6)   # TypeError - a podane dwa razy (pozycyjnie i nazwą)
    funkcja_mieszana(a=1, 20)              # SyntaxError - argument pozycyjny po nazwanym
    ```

    Sama definicja również może być niepoprawna:

    ```python
    def funkcja_mieszana(a=10, b):  # SyntaxError - argument bez wartości domyślnej po argumencie z wartością domyślną
        print(f"a: {a}, b: {b}")
    ```

## Zwracanie wielu wartości

Python pozwala zwracać wiele wartości z funkcji - w rzeczywistości jest to zwracanie jednej **krotki**, którą można od razu rozpakować.

```python
def podziel_z_reszta(a, b):
    iloraz = a // b
    reszta = a % b
    return iloraz, reszta  # Python pakuje to w krotkę (iloraz, reszta)

wynik = podziel_z_reszta(17, 5)
print(wynik)        # (3, 2)
print(type(wynik))  # <class 'tuple'>

# Bezpośrednie rozpakowywanie
iloraz, reszta = podziel_z_reszta(17, 5)
print(f"17 / 5 = {iloraz} reszty {reszta}")
```

Gdy nie potrzebujemy wszystkich wartości, używamy `_` do pominięcia wybranych:

```python
iloraz, _ = podziel_z_reszta(17, 5)  # interesuje nas tylko iloraz
```

## 📝 Zadania

Kontynuuj pracę w pliku `python1course/zaj03/funkcje.py`.

1. Napisz funkcję `zamowienie_produktu`, która przyjmuje jeden obowiązkowy argument pozycyjny `nazwa_produktu` i dwa obowiązkowe argumenty nazwane: `cena` i `ilosc`. Funkcja powinna zwracać tekst podsumowujący zamówienie, zawierający nazwę produktu, łączną cenę (cena * ilość) oraz ilość zamówionego produktu.

    - Stwórz pustą listę, do której wstawisz wartości zwracane przez funkcję dla 3 różnych produktów.

    - Przeiteruj po wypełnionej liście, wyświetl teksty.

    - Zmodyfikuj funkcję tak, żeby oprócz tekstu podsumowującego zwracała także wartość zamówienia.

    - Na koniec wyświetl sumaryczną wartość zamówień (sumę z każdego zamówionego produktu).

    - Dodaj wartość domyślną dla argumentu `ilosc` równą 1.

    !!! warning "Ważna informacja"
        Wykorzystaj poniższy początek definicji i go nie modyfikuj (poza dodaniem wartości domyślnej). Gwiazdka `*` wymusi podawanie argumentów `cena` i `ilosc` jedynie w formie nazwanej.

        ```python
        def zamowienie_produktu(nazwa_produktu, *, cena, ilosc):
        ```

2. Napisz funkcję `oblicz_srednia_ocen`, która przyjmuje nieograniczoną liczbę ocen (użyj `*args`) i zwraca ich średnią. Dodatkowo funkcja powinna przyjmować opcjonalny argument nazwany `wagi` - słownik, który każdej **wartości oceny** przypisuje wagę (np. każda piątka ma wagę 3). Jeśli wagi są podane, funkcja powinna obliczyć średnią ważoną.

    Przykład wywołania:
    ```python
    # Średnia zwykła
    print(oblicz_srednia_ocen(4, 5, 3, 5))  # 4.25

    # Średnia ważona: (4*2 + 5*3 + 3*1 + 5*3) / (2 + 3 + 1 + 3) = 41 / 9
    print(oblicz_srednia_ocen(4, 5, 3, 5, wagi={4: 2, 5: 3, 3: 1}))  # 4.555...
    ```

3. **(Dodatkowe)** Stwórz funkcję `polynomial_calculator`, która implementuje kalkulator wielomianów. Funkcja powinna przyjmować:

    - `x` - wartość, dla której obliczamy wielomian,
    - `*args` - współczynniki wielomianu (od najwyższej potęgi),
    - `**kwargs` - opcjonalne parametry:
        - `precyzja` - liczba miejsc po przecinku wyniku (domyślnie 2),
        - `dziedzina` - krotka `(min, max)` określająca dozwolony zakres `x`, np. `dziedzina=(-10, 10)`.

    Funkcja oblicza wartość wielomianu dla podanego `x` i zwraca wynik zaokrąglony do podanej precyzji.

    Przykład: wielomian 2x³ + 3x² + x + 1 dla x=2
    ```python
    result = polynomial_calculator(2, 2, 3, 1, 1)
    # Obliczy: 2*(2³) + 3*(2²) + 1*(2) + 1 = 16 + 12 + 2 + 1 = 31
    ```

    Dodatkowo, jeśli podano `dziedzina`, funkcja powinna sprawdzić, czy `x` nie wykracza poza dziedzinę, i jeśli tak, wypisać odpowiednie ostrzeżenie.

    !!! tip "Wskazówka"
        Wielomian n-tego stopnia ma n+1 współczynników. Współczynniki w `*args` są podawane od najwyższej potęgi, np. dla ax² + bx + c przekazujemy (a, b, c). Do odczytania opcjonalnych parametrów przyda się `kwargs.get("precyzja", 2)`.

## Funkcje - praktyczne porady

1. Funkcje powinny być niezależne od otoczenia - argumenty jako input, return jako output.
2. Unikamy zmiennych globalnych.
3. Nie modyfikujemy przekazanych argumentów mutowalnych - chyba że jest to celem funkcji i jasno wynika z jej nazwy oraz dokumentacji.
4. Funkcja ma być mała i mieć jeden cel.
5. Nie zmieniamy wartości zmiennych z innych modułów.

## Adnotacje

Jest to sposób na dodawanie informacji o typach danych używanych w kodzie. Choć Python jest językiem dynamicznie typowanym i nie wymaga jawnego określania typów, adnotacje dają programiście możliwość wskazania, jakie typy danych powinny być używane, co poprawia czytelność i ułatwia pracę w zespołach.

??? - note "Idea adnotacji"
    Adnotacje stanowią coś w rodzaju "podpowiedzi" dla innych programistów oraz narzędzi analizujących kod (np. linterów, IDE), które mogą je wykorzystać do ułatwienia debugowania, uzupełniania kodu, czy znajdowania potencjalnych błędów.

!!! warning "Adnotacje nie są sprawdzane"
    Python podczas działania programu **ignoruje** adnotacje - wywołanie `greet(123)` dla poniższej funkcji nie zgłosi błędu. Typy sprawdzają dopiero zewnętrzne narzędzia, np. `mypy` czy IDE.

W poniższym przykładzie argument `name` ma być typu `str`, tak samo zwracana przez funkcję wartość.

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"
```

Zmienne także mogą posiadać swoje adnotacje.

```python
age: int = 25
name: str = "Alice"
```

Typy kolekcji zapisuje się, podając typ elementów w nawiasach kwadratowych, a kilka możliwych typów łączy się znakiem `|`:

```python
def get_user_info(user_id: int) -> dict[str, str] | None:
    if user_id == 1:
        return {"name": "Hubert", "email": "hubert@example.com"}
    return None  # Funkcja może zwrócić słownik lub None

def srednia(liczby: list[float]) -> float:
    return sum(liczby) / len(liczby)
```

??? - note "Starszy zapis z modułu `typing`"
    W starszym kodzie (przed Pythonem 3.9/3.10) spotkasz zapis `List[float]`, `Dict[str, str]` czy `Optional[...]` importowany z modułu `typing`. Oznacza on to samo, ale w nowym kodzie zaleca się wbudowane `list`, `dict` i operator `|`.
