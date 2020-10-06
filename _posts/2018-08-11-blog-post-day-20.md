---
title: 'Day 20 - Decorators within classes'
date: 2018-08-11
permalink: /posts/2018/08/coding-challenge-day-20/
tags:
  - python
  - coding
  - object_orientation
  - decorators
---

**Topics:** Decorators within a class     
**Updated** 2020-10-05     
    
After having talked about decorators already on [day 5](http://www.alpopkes.com/posts/2018/07/coding-challenge-day-5/) and [day 6](http://www.alpopkes.com/posts/2018/07/coding-challenge-day-6/) I would like to revisit the topic to discuss how decorators can be used within classes. 

Let's put ourselves in the position of a Castle Kilmere member during the time the school is in war with Master Odon and his Dark Army. So these are dark, scary times. People at Castle Kilmere are constantly scared that something might happen to them, their family or their friends. So when talking to each other between classes, they weren't laughing and fooling around. They were whispering and getting to the next class or common room as quickly as possible. This means that we have to adapt the behaviour of our `says()` function in the `CastleKilmereMember` class. However, its output should only change during Master Odon's reign of terror. This is a great application for decorators!

Currently, the `CastleKilmereMember` class (at least the part relevant for us) looks like this:

```python
class CastleKilmereMember:
    """ Creates a member of the Castle Kilmere School of Magic """
    def __init__(self, name: str, birthyear: int, sex: str):
        self.name = name
        self.birthyear = birthyear
        self.sex = sex
        self._traits = {}

    def says(self, words: str) -> str:
        return f"{self.name} says {words}"
```

We want to decorate the `says()` function such that the corresponding person is *whispering* instead of talking aloud. For example, instead of an output like 'Lissy says: Be careful Luke!' we want 'Lissy whispers: Be careful Luke...'. When not dealing with classes, but a simple `says()` function, we would create a decorator like this one:

```python
def whisper(function):
    def wrapper(*args):
        original_output = function(*args)
        # We split the output into two parts, to replace "says" by "whispers"
        name, words = original_output.split("says: ")
        # We need to remove exclamation marks when whispering
        words = words.replace('!', '.')
        new_output = name + 'whispers: ' + words
        return new_output
    return wrapper

@whisper
def says(person: str, words: str) -> str:
    return f"{person} says: {words}"
```

When not applying the `@whisper` decorator, the output of `says("Lissy", "Be careful Luke!")` is `Lissy says: Be careful Luke!`. When decorating the function its output is changed to `Lissy whispers: Be careful Luke...`. That's exactly what we want! However, we can't use the current version of our `whisper` decorator inside the `CastleKilmereMember` class. We need to incorporate the `self` argument of our class. This is done as follows: 

```python
class CastleKilmereMember:
    """ Creates a member of the Castle Kilmere School of Magic """
    def __init__(self, name: str, birthyear: int, sex: str):
        self.name = name
        self.birthyear = birthyear
        self.sex = sex
        self._traits = {}

    def whisper(function):
        def wrapper(self, *args):
            original_output = function(self, *args)
            first_part, words = original_output.split(' says: ')
            words = words.replace('!', '.')
            new_output = f"{first_part} whispers: {words}.."
            return new_output
        return wrapper

    @whisper
    def says(self, words: str) -> str:
        return f"{self.name} says: {words}"
```

Let's test our function. First, we need to add a new constructor to the `Pupil` class to create Lissy.

```python
from typing import Tuple, Optional 
   
class Pupil(CastleKilmereMember):
    """ Create a Castle Kilmere Pupil """
    def __init__(self, name: str, birthyear: int, sex: str, start_year: int, pet: Optional[Tuple[str, str]] = None):
        super().__init__(name, birthyear, sex)
        self.start_year = start_year
        self.known_spells = set()

        if pet is not None:
            self.pet_name, self.pet_type = pet

    @classmethod
    def lissy(cls):
        return cls('Lissy Spinster', 2008, 'female', 2018, ('Ramses', 'cat'))
```

Next, we instantiate Lissy and let her say a few words.

```python
lissy = Pupil.lissy()
print(lissy.says("Be careful Luke!"))
```

The output of this is `Lissy Spinster whispers: Be careful Luke...`. That's it! Our decorator is working correctly and accessing the `self` attribute.
