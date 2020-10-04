---
title: 'Day 3 - Type annotations'
date: 2018-07-25
permalink: /posts/2018/07/coding-challenge-day-3/
tags:
  - python
  - coding
  - object_orientation
  - function_annotations
---

**Topics:** Type annotations
**Updated** 2020-10-04

## Type annotations
Type annotations are a very cool feature that came out with Python 3.5. They allow us to add arbitrary metadata to function arguments and the return value of a function. Why this is useful? First of all, it allows us to document of what type our function parameters are. Furthermore, they can be used for things like type checking. For more use cases, look [here](https://www.python.org/dev/peps/pep-3107/). 
   
   
Let's add annotations to our `___init___` constructors:

```python
class CastleKilmereMember:
    """ Creates a member of the Castle Kilmere School of Magic """

    def __init__(self, name: str, birthyear: int, sex: str):
        ...


class Professor(CastleKilmereMember):
    """ Creates a Castle Kilmere professor """

    def __init__(self, name: str, birthyear: int, sex: str, subject: str):
        ...


class Ghost(CastleKilmereMember):
    """ Creates a Castle Kilmere ghost """

    def __init__(self, name: str, birthyear: int, sex: str, year_of_death: int):
        ...


class Pupil(CastleKilmereMember):
    """ Create a Castle Kilmere Pupil """

    def __init__(self, name: str, birthyear: int, sex: str, start_year: int, pet=None):
	...
```

## Specifying the return type
It's easy to specify the return value of a function. Remember the `says` function of our class `CastleKilmereMember`? It takes a string of words as an input and returns a string. This can be
specified as follows:

```python
class CastleKilmereMember:
    def __init__(self, name: str, birthyear: int, sex: str):
    	...
        
    def says(self, words: str) -> str:
        return f"{self.name} says: {words}"
```

We can also reference classes as a return value. Our classmethod `school_headmistress` returns an instance of the `CastleKilmereMember` class. We can specify this by using the name of the class as a type annotation for the return value:

```python
class CastleKilmereMember:
    ...
        
    @classmethod
    def school_headmistress(cls) -> 'CastleKilmereMember':
        return cls('Miranda Mirren', 1963, 'female')
```

We need to provide the class name in colons because the type annotation represents a *forward reference*. This behaviour will change with Python 3.10 where classes can be used as type annotations drectly. We can get this new behaviour already with Python 3.7+ with the import `from __future__ import annotations`:

```python
from __future__ import annotations

class CastleKilmereMember:
    ...

    @classmethod
    def school_headmistress(cls) -> CastleKilmereMember:
        return cls('Miranda Mirren', 1963, 'female')
        
```
   
## Using nested type annotations
The `pet` attribute our our Pupil class is given by a tuple which contains the name of the pet (a string) and its type (also a string). We can specify this using a nested type annotation: 

```python
from typing import Tuple
class Pupil(CastleKilmereMember):
    def __init__(..., pet: Tuple[str, str]):
           ...
```

Simple datatypes like `str` and `int` can be used directly as type hints. For more complicated types like `tuple` or `list` we need to import them from the `typing` module (example `from typing import Tuple`). With Python 3.8+ we can use the types directly as type hints.

## Where to put spaces when using type annotations
Since the post I mentioned above does not include whitespaces between the different parts of an annotation I checked the correct syntax of function annotations. As mentioned in [PEP8](https://www.python.org/dev/peps/pep-0008/?) the proper usage of spaces is as follows:   
   
Function annotations should use the normal rules for colons and always have spaces around the -> arrow if present. 

Yes:

```
def munge(input: AnyStr): ...
def munge() -> AnyStr: ...
```

No:

```
def munge(input:AnyStr): ...
def munge()->PosInt: ...
```

When combining an argument annotation with a default value, use spaces around the = sign (but only for those arguments that have both an annotation and a default).

Yes:

```
def munge(sep: AnyStr = None): ...
def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
```

No:

```
def munge(input: AnyStr=None): ...
def munge(input: AnyStr, limit = 1000): ...
```
