## Metody instancji

Domyślny rodzaj metody. Pierwszy argument to `self` - instancja, na której metodę wywołano. Służą do operacji na stanie tego obiektu.

```python
class Licznik:
    def __init__(self, start=0):
        self.wartosc = start

    def zwieksz(self, krok=1):
        self.wartosc += krok
        return self.wartosc

c = Licznik(10)
print(c.zwieksz())   # 11
print(c.zwieksz(5))  # 16
```

Wywołanie `c.zwieksz(5)` Python tłumaczy na `Licznik.zwieksz(c, 5)`. Dlatego `self` musi być pierwszy w definicji, choć przy wywołaniu przez instancję go nie podajesz.

!!! tip "Zastosowanie"
    Odczyt i zmiana atrybutów instancji, logika związana z jednym obiektem.

Metody klasy (`@classmethod`) i statyczne (`@staticmethod`) omówimy na kolejnych zajęciach. Na razie wystarczą metody instancji oraz właściwości.

## Właściwości (`@property`)

Dekorator `@property` zamienia metodę w atrybut: czytasz ją **bez nawiasów**. Dzięki temu możesz dodać walidację albo wartość wyliczaną, nie zmieniając sposobu odwołania z zewnątrz.

Podkreślenie w `_celsius` to **konwencja**: "to szczegół implementacyjny, nie ruszaj go z zewnątrz". Python niczego nie blokuje - to umowa między programistami. Silniejszy mechanizm (`__nazwa`) pojawi się na kolejnych zajęciach.

```python
class Temperatura:
    def __init__(self, celsius):
        self._celsius = celsius

    @property
    def celsius(self):
        return self._celsius

    @celsius.setter
    def celsius(self, wartosc):
        if wartosc < -273.15:
            raise ValueError("Temperatura nie może być niższa niż zero absolutne.")
        self._celsius = wartosc

    @property
    def fahrenheit(self):
        return self._celsius * 9 / 5 + 32

t = Temperatura(100)
print(t.celsius)     # 100    - jak atrybut, nie jak metoda
print(t.fahrenheit)  # 212.0  - wyliczane na bieżąco, tylko do odczytu

t.celsius = 0        # działa, bo jest setter
print(t.fahrenheit)  # 32.0

# t.celsius = -300   # ValueError
# t.fahrenheit = 32  # AttributeError - brak settera, właściwość tylko do odczytu
```

Kolejność jest ważna: najpierw `@property` definiuje getter o danej nazwie, potem `@nazwa.setter` dopina setter do tej samej właściwości.

!!! tip "Zastosowanie"
    Kontrolowany dostęp do atrybutów, walidacja przy przypisaniu, wartości liczone w locie.

## Przykład

Zapoznaj się z kodem w `python1course.zaj05.fleet.ambulance`: konstruktor, `__str__` / `__repr__` oraz właściwość `status` (getter i setter z walidacją).

W tej klasie są też już `@staticmethod validate_id` oraz `@classmethod get_instances_count` - to zapowiedź kolejnych zajęć. Na razie wystarczy, że wiesz, iż `status` da się czytać i ustawiać jak atrybut, a niepoprawna wartość kończy się `ValueError`.
