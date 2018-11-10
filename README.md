Dokumentacja w języku polskim jest [tutaj.](README_pl.md)
# py-librus-api
# Table of contents
1. [Intro](#intro)
2. [Instalation](#instalation)
3. [Exmaple usage](#example-usage)
4. [Functions](#functions)
5. [Threading](#threading)
# Intro
API for librus e-register.<br>
There is no guarantee of developing this API further more!
# Instalation
`pip install py-librus-api`
# Example usage
```python
from py_librus_api import Librus

librus = Librus()
librus.login(username, password)

print(librus.get_lucky_number())
```
More info in [functions](#functions)
# Functions
**Required params/functions are marked with `!` prefix.**<br>
**`*` means that there is explanation below or something is optional.**
## !login(!username, !password)
Function returns `true` if logging in was successful and `false` when not.<br>
`username` - Variable that contains user login. <br>
`password` - Variable that contains user password.<br>
Example usage:
```python
librus.login(username, password)
```
## You can check if user is logged in!
```python
if librus.logged_in:
    ...
```
## If user is not logged in, "User not logged in" exception will be raised!
## If connection error occurs, "Connection error" will be raised!
## get_lucky_number()
Returns lucky number (`int`).
## get_grades()
**For displaying grades in the console, it is recommended to use pretty-print (pprint)!**
Returns all user grades in this foramt:<br>
```
grades = {
  "Biologia": [
    {
      "Grade": "5",
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
**Note that subject names are in language provided by Librus API (in this example it's polish)**
## get_teachers(mode*)
Returns teachers' personal data (firstname, lastname) in couple of formats.
You can choose format like that:
```python
librus.get_teachers(mode="print") # etc.
```
### List of formats:
#### normal (default)
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
Returns a list of days free from school.
Format:
```
[
    {'DateFrom': '2019-01-01', 'DateTo': '2019-01-01', 'Name': 'Nowy Rok'},
    ...
]
```
## get_teacher_free_days()
Returns a list of teachers' absence.
Format:
```
[
    {
        'DateFrom': '2018-10-24',
        'DateTo': '2018-10-26',
        'Teacher': {'FirstName': 'Jan', 'LastName': 'Kowalski'},
        'TimeFrom': '13:40:00',
        'TimeTo': '15:15:00',
        'Type': 'szkolenie'
    },
]
```
**It can happen that `TimeFrom` and `TimeTo` won't exist!**

## get_attendances()
Returns attendances in this format:
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
           'Standard': True}
           }
 ...
]
```
# Threading
In version 0.4 threading was added to improve performance of API by downloading multiple data at the same time. You can improve it even more by running functions using `threading` by yourself.<br>
**Note: Speed of the API mostly depends on user's internet speed!**
### Example
```python
from py_librus_api import Librus
from threading import Thread # <-- Note that there is new import!

# Log in as always
librus = Librus()
librus.login(username, password)

threads = []

# Get lucky number
a = Thread(target=librus.get_lucky_number)
a.start()
threads.append(a)

# Get grades
b = Thread(target=librus.get_grades)
b.start()
threads.append(b)

for thread in threads:
    thread.join()
```
### Comparison
I compared above code (+2 other functions) with regular execution without `threading`.<br>
**Note: Logging in time is not counted.**<br>
**Note 2: Time is measured in seconds [s].**
#### API before and after threading update
```
[WITHOUT THREADING AT ALL]: 6.034927606582642
[AFTER THREADING UPDATE]: 4.9559431076049805
```
As you can see, it's not the best improvement, but still it's something isn't it?
#### API after threading update vs even more threaded code (example above)
```
[AFTER THREADING UPDATE]: 4.9559431076049805
[AFTER THREADING UPDATE + EVEN MORE THREADING (example)]: 1.9219775199890137
```
Now that's what we're talking about. If you want the best speed, you have to use `threading` by yourself and of course the newest version of this API!