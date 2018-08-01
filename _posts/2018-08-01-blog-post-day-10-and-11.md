---
title: 'Day 10 & 11 - Namedtuples'
date: 2018-08-01
permalink: /posts/2018/08/coding-challenge-day-10-and-11/
tags:
  - python
  - coding
  - object_orientation
  - tuples
  - namedtuples
---

**Topics:** Immutable classes, namedtuples

## Tuples
Before looking at namedtuples, we should review what a tuple is. In Python, a tuple is a simply data structure that can be used for grouping arbitrary objects. Important to know is that tuples are *immutable*. That means that once we create a tuple we can not change it anymore.   

We already used tuples in our Harry Potter universe. For example, we defined the ```pet``` attribute of the ```Pupil``` class to be a tuple:

```python
pet = ('pet_name', 'pet_type')
harrys_pet = ('Hedwig', 'owl')
```

The 'pet' tuple has two fields: 'pet_name' and 'pet_type' that can be accessed using their integer index. Once we create a tuple like ```harrys_pet``` we can't change it anymore. For example, running

```python
harrys_pet[0] = 'Pigwidgeon'
```

will throw a ```TypeError: 'tuple' object does not support item assignment```.

## Namedtuples
As their name suggests, namedtuples are a variation (or rather extension) of plain tuples. In particular, namedtuples allow us to name the fields of a tuple. This makes it much easier to access the indiviual fields. Also, it makes the code more readable. In the plain tuple example above, we can access the values stored in the tuple only by using integer indices, as ```harrys_pet[0]``` or ```harrys_pet[1]```. When having only two fields this is not too bad. But with more than three fields things become messy. 

Creating a namedtuple is easy. There are two kinds of namedtuples we can use: ```collections.namedtuple``` or ```typing.NamedTuple```. When using ```collections.namedtuple``` our pet tuple would be defined as follows:

```python
from collections import namedtuple
Pet = namedtuple('Pet', 'pet_name pet_type')
harrys_pet = Pet('Hedwig', 'owl')
```

We can access the fields of the namedtuple using the field names or their indices:
```python
name = harrys_pet[0]
# or
name = harrys_pet.pet_name
```

```typing.NamedTuple``` has a slightly different syntax and allows us to specify the type of each field. It also allows us to add methods to the class (although this is possible with collections.namedtuple, too, it's a little more complex):

```python
from typing import NamedTuple

class Pet(NamedTuple):
    pet_name: str
    pet_type: str

    def __repr__(self):
        return f"{self.pet_name}, {self.pet_type}"

harrys_pet = Pet('Hedwig', 'owl')
print('harrys_pet: ', harrys_pet)
```

You can read more about ```typing.NamedTuple``` in the [Python docs](https://docs.python.org/3/library/typing.html).

## Death eater class
We don't want our pupils, professors, ghosts to be immutable. A suitable group of people for an immutable class are the death eaters. So let's create a NamedTuple for them!

```python
from typing import NamedTuple

class DeathEater(NamedTuple):
    name: str
    birthyear: str

    @property
    def leader(self):
        voldemort = DeathEater('Voldemort', 1926)
        return voldemort

    def __repr__(self):
        return f"{self.__class__.__name__}({self.name}, birthyear: {self.birthyear})"
```

From now on we can easily create new death eaters:
```python
lucius = DeathEater('Lucius Malfoy', 1953)
print('Lucius: ', lucius)
print("Leader: ', lucius.leader)

bellatrix = DeathEater('Bellatrix Lestrange', 1951)
print('bellatrix: ', bellatrix)
```

## Data classes

Note: When using Python 3.7 we can also use [dataclasses](https://docs.python.org/3/library/dataclasses.html) for creating immutable classes!


