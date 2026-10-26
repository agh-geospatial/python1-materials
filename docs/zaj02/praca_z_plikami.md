# Praca z plikami w Pythonie

## Wprowadzenie
Praca z plikami jest kluczową częścią programowania. Python oferuje wbudowane funkcje do odczytu i zapisu plików, które pozwalają na manipulowanie danymi zapisanymi na dysku. 

Podczas pracy z plikami warto stosować **menedżery kontekstu** (`with`), które dbają o zamknięcie pliku po zakończeniu bloku - nawet jeśli w jego trakcie wystąpi błąd. Zapobiega to pozostawianiu otwartych plików, niezapisanym danym i błędom dostępu.

### Podstawowe operacje na plikach

#### Otwieranie i czytanie pliku tekstowego
```python
with open('example.txt', 'r', encoding='utf-8') as file:
    content = file.read()
    print(content)  # Wyświetla zawartość pliku
```

- `'r'` - tryb odczytu (`read`),
- `encoding='utf-8'` - kodowanie znaków, potrzebne do poprawnej obsługi np. polskich liter.

!!! warning "Zawsze podawaj `encoding`"
    Jeśli nie podasz kodowania, Python może użyć domyślnego kodowania systemu - na Windows często jest to `cp1250`, a nie UTF-8. Ten sam kod może wtedy działać na jednym komputerze, a na innym psuć polskie znaki lub zgłaszać `UnicodeDecodeError`.

Ścieżka do pliku jest liczona względem **katalogu roboczego**, z którego uruchomiono skrypt (można go sprawdzić przez `os.getcwd()`), a nie względem położenia samego pliku `.py`.

#### Odczyt linii po linii
Aby nie wczytywać całego pliku do pamięci, można czytać go linia po linii:
```python
with open('example.txt', 'r', encoding='utf-8') as file:
    for line in file:
        print(line.strip())  # Usuwanie białych znaków z początku i końca, w tym znaku nowej linii
```

Metoda `readlines()` zwraca od razu listę wszystkich linii:
```python
with open('example.txt', 'r', encoding='utf-8') as file:
    linie = file.readlines()
print(len(linie))
```

#### Zapisywanie do pliku
```python
with open('output.txt', 'w', encoding='utf-8') as file:
    file.write("To jest przykładowy tekst.\n")
    file.write("Druga linia tekstu.\n")
```

- `'w'` - tryb zapisu (`write`), tworzy plik lub nadpisuje istniejący,
- `'a'` - tryb dopisania (`append`), dodaje dane na końcu pliku zamiast go nadpisywać.

Metoda `write()` nie dodaje znaku nowej linii - trzeba go dopisać samodzielnie (`\n`).

#### Wczytywanie tylko części pliku
Jeśli plik jest duży, warto odczytywać go fragmentami:
```python
with open('example.txt', 'r', encoding='utf-8') as file:
    chunk = file.read(1024)  # Czyta 1024 znaki na raz
    while chunk:             # Pusty łańcuch oznacza koniec pliku
        print(chunk)
        chunk = file.read(1024)
```

W trybie tekstowym argument `read()` oznacza liczbę **znaków**, a w trybie binarnym (`'rb'`) - liczbę **bajtów**.

### Praca z plikami binarnymi
Pliki binarne (np. obrazy, pliki audio) odczytuje się w trybie `'rb'`.
```python
with open('image.png', 'rb') as file:
    data = file.read()
    print(data[:20])  # Wyświetla pierwsze 20 bajtów
```
Zapisywanie plików binarnych:
```python
with open('copy.png', 'wb') as new_file:
    new_file.write(data)
```

### Automatyczne zamykanie plików
Menedżery kontekstu (`with`) automatycznie zamykają plik, ale można to zrobić ręcznie:
```python
file = open('example.txt', 'r', encoding='utf-8')
content = file.read()
file.close()
```
Jednak lepiej zawsze używać `with open()`, ponieważ w przypadku błędów plik może pozostać otwarty.

---

## Pliki JSON
JSON to popularny tekstowy format wymiany danych. Jego struktura odpowiada typom Pythona: obiekt JSON to słownik, tablica to lista, a do tego dochodzą liczby, łańcuchy znaków, `true`/`false` (`True`/`False`) i `null` (`None`). Do obsługi JSON służy wbudowany moduł `json`.

#### Odczyt pliku JSON
```python
import json

with open('dane.json', 'r', encoding='utf-8') as file:
    dane = json.load(file)  # Zwraca słownik lub listę

print(type(dane))
```

#### Zapis do pliku JSON
```python
import json

wynik = {
    "kurs": "Python",
    "studenci": ["Ania", "Łukasz"],
    "liczba_zajec": 14,
}

with open('wynik.json', 'w', encoding='utf-8') as file:
    json.dump(wynik, file, ensure_ascii=False, indent=2)
```

- `ensure_ascii=False` - zapisuje polskie znaki wprost, zamiast w postaci `\u0141`,
- `indent=2` - formatuje plik wcięciami, dzięki czemu jest czytelny dla człowieka.

!!! warning "Nie wszystko da się zapisać do JSON"
    JSON nie obsługuje m.in. zbiorów (`set`) ani krotek. Próba zapisania zbioru kończy się błędem `TypeError: Object of type set is not JSON serializable` - przed zapisem trzeba go zamienić na listę, np. `list(zbior)`. Krotki są zapisywane jako listy, więc po odczycie wrócą jako listy.

Funkcje `json.loads()` i `json.dumps()` (z literą `s` od *string*) działają analogicznie, ale na łańcuchach znaków zamiast na plikach.

---

## Podsumowanie
1. **Menedżery kontekstu** (`with open()`) ułatwiają zarządzanie plikami.
2. **Kodowanie** zawsze podawaj jawnie, np. `encoding='utf-8'`.
3. **Czytanie dużych plików** można optymalizować, odczytując je linia po linii lub fragmentami (`read(1024)`).
4. **Pliki binarne** wymagają trybów `'rb'` i `'wb'`.
5. **Moduł `json`** zamienia pliki JSON na słowniki i listy (`json.load`) i odwrotnie (`json.dump`).

## 📝 Zadania

Stwórz plik `python1course/zaj02/pliki.py` i wykonaj w nim poniższe zadania.

1. Zapisz do pliku `notatki.txt` trzy linie tekstu zawierające polskie znaki. Otwórz plik w edytorze i sprawdź, czy znaki wyświetlają się poprawnie.
2. Dopisz do tego samego pliku czwartą linię, nie usuwając poprzednich.
3. Wczytaj plik linia po linii i wypisz każdą linię poprzedzoną jej numerem, zaczynając od 1 (np. `1: Pierwsza linia`). Zadbaj o to, żeby między liniami nie pojawiały się puste wiersze.
4. Stwórz słownik opisujący Ciebie (np. imię, kierunek, lista ulubionych przedmiotów) i zapisz go do pliku `ja.json` w czytelnej postaci, z polskimi znakami.
5. Wczytaj plik `ja.json` do nowej zmiennej i sprawdź operatorem `==`, czy jest równa oryginalnemu słownikowi.