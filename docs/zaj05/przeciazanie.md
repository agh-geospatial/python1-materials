Przy `print(obiekt)` Python nie wypisuje sam z siebie Twoich atrybutów. Domyślnie dostajesz coś w stylu `<Samochod object at 0x...>`. Żeby to zmienić, definiujesz metody specjalne `__str__` i `__repr__` (nazwy z podwójnym podkreśleniem, *dunder methods*).

| Metoda | Wywoływana przez | Przeznaczenie |
|---|---|---|
| `__str__` | `print()`, `str()` | Czytelna dla człowieka |
| `__repr__` | `repr()`, konsola interaktywna, debugger | Techniczna, najlepiej da się wkleić z powrotem do Pythona |

Jeśli zdefiniujesz tylko `__repr__`, Python użyje jej także tam, gdzie brakuje `__str__`. Odwrotnie nie: sam `__str__` nie zastąpi `__repr__`.

```python
class Punkt:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Punkt({self.x}, {self.y})"

    def __repr__(self):
        return f"Punkt(x={self.x!r}, y={self.y!r})"

p = Punkt(1, 2)
print(p)        # Punkt(1, 2)      - __str__
print(repr(p))  # Punkt(x=1, y=2)  - __repr__
```

`!r` w f-stringu wstawia wynik `repr()` danej wartości, więc napisy dostaną cudzysłowy.

!!! tip
    `__repr__` niech wygląda jak kod tworzący obiekt. `__str__` może być krótszy i opisowy.

`__init__` też jest metodą specjalną - Python woła ją przy `Punkt(1, 2)`.

W `Ambulance` jest dodatkowo `__eq__` z `isinstance` (porównanie po `id` i `vehicle_type`). Szersze przeciążanie (`+`, `len`, `in`, iteracja) ćwiczymy na `IncidentQueue` na kolejnych zajęciach.
