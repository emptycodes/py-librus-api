Documentation in english can be found [there.](README.md)
# py-librus-api
# Spis treści
1. [Wstęp](#wstęp)
2. [Instalacja](#instalacja)
3. [Przykładowe użycie](#przykładowe-użycie)
4. [Spis funkcji](#spis-funkcji)
# Wstęp
API do e-dziennika librus synergia.
Nie ma gwarancji na to, że API będzie rozwijane!
# Instalacja
`pip install py-librus-api`
# Przykładowe użycie
```python
from py_librus_api import Librus


librus = Librus()

"""Zapętla się dopóki użytkownik nie zaloguje się"""
while not librus.logged_in:
    if not librus.login(login, password):
        print("Logowanie nie powiodło się! Sprawdź twój nick i/lub hasło.")
    else:
        print("Zalogowano.")

```
# Spis funkcji
**Wymagane parametry/funkcje zaznaczone są prefixem `!`**<br>
**Jeżeli przy nazwie jest `*` oznacza to, że poniżej będzie wyjaśnienie jakiejś rzeczy**
## !login(!login, !password)
Funkcja zwraca true gdy logowanie powiodło się i false gdy nie.<br>
`login` - Zmienna string z loginem użytkownika.<br>
`password` - Zmienna string z hasłem użytkownika.<br>
Przykładowe użycie:
```python
librus.login(login_var, password_var)
```
## Możesz sprawdzić czy użytkownik jest zalogowany!
```python
if librus.logged_in:
    ...
```
## Jeżeli użytkownik nie jest zalogowany, funkcje zwrócą -1!
## Jeżeli nastąpi błąd połączenia, funkcje zwrócą -2!
## get_lucky_number()
Zwraca szczęśiwy numerek w formacie `int`
## get_grades()
Zwraca wszystkie oceny użytkownika w poniższym formacie.<br>
**Do wypisania ocen w konsoli zaleca się używanie pretty-print (pprint)!**
```
grades = {
  "Biologia": [
    {
      "grade": "5",
      "Weight": "3",
      "Category": "Kartkówka",
      'Teacher': {'FirstName': 'Jan', 'LastName': 'Kowalski'},
      "Comment": "kartkówka z działu o płazach",
      "To_the_average": "Tak"
    }
    ...
  ]
  ...
}
```
## get_teachers(mode*)
Funkcja zwraca dane nauczycieli, w formacie zależnym od wybranego trybu.
Np. `print`
```
get_teachers(mode="print")
```
### Lista trybów oraz format w jakim zwracane są dane:
#### normal (domyślny)
```
{
  1123576: {'FirstName': 'Jan', 'LastName': 'Kowalski'},
  1983456: {'FirstName': 'Grażyna', 'LastName': 'Kowalska'},
  ...
}
```
#### fullname
```
[
  "Jan Kowalski",
  "Grażyna Kowalska",
  ...
]
```
#### fullname-id
```
[
  '1476937: Jan Kowalski',
  '1484010: Grazyna Kowalska',
  ...
 ]
```
## get_school_free_days()
Zwraca listę dni wolnych od szkoły w formacie:
```
[
    {'DateFrom': '2019-01-01', 'DateTo': '2019-01-01', 'Name': 'Nowy Rok'},
    ...
]
```
## get_teacher_free_days()
Zwraca listę nieobecności nauczyciela w formacie*:
```
[
    {
        'DateFrom': '2018-10-09',
        'DateTo': '2018-10-09',
        'Teacher': {'FirstName': 'Jan', 'LastName': 'Kowalski'},
        'TimeFrom': '13:40:00',
        'TimeTo': '15:15:00',
        'Type': 'szkolenie'
    },
]
```
**Może się zdarzyć, że pola `TimeFrom` i `TimeTo` nie będą istniały!**

## get_attendances()
Zwraca listę obecności, nieobecności itp. w tym formacie:
```
[
{'AddDate': '2018-10-29 12:52:51',
  'AddedBy': {'FirstName': 'Jan', 'LastName': 'Kowalski'},
  'Date': '2018-10-29',
  'Id': 123456,
  'Lesson': {'Subject': 'Chemia',
            'Teacher': {'FirstName': 'Jan', 'LastName': 'Kowalski'}},
  'LessonNo': 6,
  'Semester': 1,
  'Type': {'IsPresenceKind': True,
           'Name': 'Obecność',
           'Order': 1,
           'Short': 'ob',
           'Standard': True}}
  ...
]
```