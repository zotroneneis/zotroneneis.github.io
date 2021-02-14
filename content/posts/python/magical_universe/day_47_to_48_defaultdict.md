---
title: collections.defaultdict
date: 2018-09-07
menu:
  sidebar:
    name: Defaultdict
    identifier: defaultdict
    parent: magical_universe
    weight: 24
---

**Topics:** `collections.defaultdict`     
**Updated** 2020-10-06

## The `collections` module

The [`collections` module](https://docs.python.org/3/library/collections.html#module-collections) in Python contains several useful classes. One of them is especially helpful for our Magical Universe: `collections.defaultdict`.

When defining our `CastleKilmereMember` class we specified `self.traits` to be an empty dictionary. New positive and negative traits can be added to a person using the `add_trait()` function. We can check whether a person possesses a certain trait using the `exhibits_trait()` function. The relevant parts of the class look as follows:

```python
class CastleKilmereMember:
    """ Creates a member of the Castle Kilmere School of Magic """
    def __init__(self, name: str, birthyear: int, sex: str):
        self.name = name
        self.birthyear = birthyear
        self.sex = sex
        self._traits = {}

    def add_trait(self, trait, value=True):
        self._traits[trait] = value

    def exhibits_trait(self, trait: str) -> bool:
        try:
            value = self._traits[trait]
            return value
        except KeyError as e:
            print(f"{self._name} does not have a character trait with the name {e}")
            return False
```

As visible in the definition of `exhibits_trait()` we have to catch and handle errors caused by querying the `_traits` dictionary with a non-existent key. Wouldn't it be much nicer if we could just return `False` in case a Castle Kilmere member does not possess a certain character trait? We already discussed that this can be achieved using the `dict.get()` function (see post on Duck Typing for more details). Another, even more powerful solution is to use the `defaultdict` class from the `collections` module.

## `collections.defaultdict`

`collections.defaultdict` is a subclass of the general dictionary type. What makes `defaultdict` perfect for our problem is that it allows to specify a *callable* whose return value will be used whenever a requested key cannot found. The basic usage of `collections.defaultdict` is as follows: 

```python
from collections import defaultdict

dict_ = defaultdict(default_factory)
```

if `default_factory` is not specified, i.e. if we just use `dict_ = defaultdict()` the dictionary will raise a `KeyError` whenever a requested key cannot be found (just like a normal dictionary). So we want to specify a default value.
    
Although we want to use `False` as our default value, we can't use `dict_ = defaultdict(False)`. The reason for this is simple: `defaultdict` requires a callable (e.g. a function) as an argument that provides the default value when invoked without arguments. `False` is not a callable but a boolean. So we have to define a function that returns `False` when called without arguments:

```python
def return_false() -> bool:
    return False

dict_ = defaultdict(return_false)
```

Alternatively, we could also specify a lambda function:

```python
dict_ = defaultdict(lambda: False)
```


The fact that `defaultdict` requires a callable makes it very powerful. We could create any kind of function and use the functions return value as a default. This has several important use cases. One common problem that can be solved with a defaultdict is grouping items in a collection. 


## Grouping items in a collection
Lets say we have a list of some of the pets Castle Kilmeres pupils are allowed to bring to school. We want to group the pets by type, that is, having all the owls together, all the cats and so on. 

```python
pets = [('Cotton', 'owl'), ('Ramses', 'cat'),
        ('Twiggles', 'owl'), ('Oscar', 'cat'),
        ('Louie', 'cat'), ('Bob', 'ferret'),
        ('Winston', 'owl'), ('Harry', 'owl')]
```

This can be achieved by providing the callable `list` as an argument to the defaultdict. We create a defaultdict using `list` as a default factory. To group the pets, we loop through our list of pets. Our first item in the list is `Cotton` (Luke's owl). When we try to find the key `owl` in the dictionary it won't be found. Since we used `list` as a default factory, a new empty list will be created and inserted into the dictionary for the key `owl`. We then append the name (which is `Cotton`) to this list. The next time we look for the key `owl` it will already be contained in the dictionary and the name can be appended to the existing list. 

```python
from collections import defaultdict

pets = [('Cotton', 'owl'), ('Ramses', 'cat'),
        ('Twiggles', 'owl'), ('Oscar', 'cat'),
        ('Louie', 'cat'), ('Bob', 'ferret'),
        ('Winston', 'owl'), ('Harry', 'owl')]

types_of_pets = defaultdict(list)
for name, type_ in pets:
    types_of_pets[type_].append(name)
```

Take a guess how the output looks like when looping through the resulting dictionary:
```python
for key, value in types_of_pets.items():
    print(f"{key}: {value}")
```

The output is hopefully what you expected. We get a list of all owls, a list of all cats, and so on:
```python
for key, value in types_of_pets.items():
    print(f"{key}: {value}")

>>> owl: ['Cotton', 'Twiggles', 'Winston', 'Harry']
>>> cat: ['Ramses', 'Oscar', 'Louie']
>>> ferret: ['Bob']
```

There are a lot more uses cases for the `defaultdict` class. For example, we could use a `defaultdict` to count the number of pets of each type. Use this as an exercise: which `default_factory` could we use to count the number of pets of each type?


