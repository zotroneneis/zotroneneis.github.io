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
**Updated** 2020-10-04    

## Tuples
Before looking at namedtuples, we should review what a tuple is. In Python, a tuple is a simple data structure that can be used for grouping arbitrary objects. Important to know is that tuples are *immutable*. That means that once a tuple has been created, it can not be changed anymore. We already used tuples in our Magical Universe. For example, we defined the `pet` attribute of the `Pupil` class to be a tuple:

```python
pet = ('name', 'type')
lissys_pet = ('Ramses', 'cat')
```

The `pet` tuple has two fields: `name` and `type` that can be accessed using their integer index. Once we create a tuple like `lissys_pet` we can't change it anymore. For example, running

```python
lissys_pet[0] = 'Twiggles'
```

throws a `TypeError: 'tuple' object does not support item assignment`.

## Namedtuples
As their name suggests, namedtuples are a variation (or rather extension) of plain tuples. In particular, namedtuples allow us to name the fields of the tuple. This makes it much easier to access the individual fields. Also, it makes our code more readable. In the plain tuple example above, we could access the values stored in the tuple only by using integer indices, as `lissys_pet[0]` or `lissys_pet[1]`. When having only two fields this is not too bad. But with more than three fields things become messy. 

Creating a namedtuple is easy. There are two kinds of namedtuples we can use: `collections.namedtuple` or `typing.NamedTuple`. When using `collections.namedtuple` our pet tuple would be defined as follows:

```python
from collections import namedtuple
Pet = namedtuple('Pet', ['name, type'])
lissys_pet = Pet('Ramses', 'cat')
```
We can access the fields of the namedtuple using the field names or their indices:
```python
name = lissys_pet[0]
# or
name = lissys_pet.name
```

`typing.NamedTuple` has a slightly different syntax and allows us to specify the type of each field. It also allows us to add methods to the class (although this is possible with `collections.namedtuple`, too, it's a little more complex). Another advantage of the namedtuple classes is that they come with an implementation of the `__repr__` method. So we don't have to create that ourselves anymore but get a nice string representation of our objects right away (as you can see in the example below).

```python
from typing import NamedTuple

class Pet(NamedTuple):
    name: str
    type: str

lissys_pet = Pet('Ramses', 'cat')
print(lissys_pet)
>>> Pet(name='Ramses', type='cat')
```

You can read more about `typing.NamedTuple` in the [Python docs](https://docs.python.org/3/library/typing.html).

## Dark Army class
We don't want our pupils, professors and ghosts to be immutable. A suitable group of people for an immutable class are the Dark Army members. Why? Because once you become a member of the Dark Army there is now way back. So let's create a NamedTuple for them!

```python
from typing import NamedTuple

class DarkArmyMember(NamedTuple):
    name: str
    birthyear: str

    @property
    def leader(self) -> 'DarkArmyMember':
        master_odon = DarkArmyMember('Master Odon', 1971)
        return master_odon
```

From now on we can easily create new Dark Army members:
```python
keres = DarkArmyMember('Keres Fulford', 1983)

print(keres)
>>> DarkArmyMember(name='Keres Fulford', birthyear=1983)

print(keres.leader)
>>> DarkArmyMember(name='Master Odon', birthyear=1971)
```

## Data classes
Note: When using Python 3.7 we can also use [dataclasses](https://docs.python.org/3/library/dataclasses.html) for creating immutable classes! Data classes are discussed in more detail on [day 16 to 18](http://alpopkes.com/posts/2018/08/coding-challenge-day-16-to-18/).


