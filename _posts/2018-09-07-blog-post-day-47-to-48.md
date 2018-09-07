---
title: 'Day 47 to 48 - ```collections.defaultdict``` '
date: 2018-09-07
permalink: /posts/2018/08/coding-challenge-day-47-to-48/
tags:
  - python
  - coding
  - defaultdict
  - collections
---

Topics: ```collections.defaultdict```

## The ```collections``` module

The [```collections``` module](https://docs.python.org/3/library/collections.html#module-collections) in Python contains several very useful classes. One of them is helpful for our Magical Universe: ```collections.defaultdict```.

When defining our ```CastleKilmereMember``` class, we specified ```self.traits``` to be an empty dictionary. New positive and negative traits can be added to a person using the ```add_trait()``` function. We can check whether a person possesses a certain trait using the ```exhibit_trait()``` function. The relevant parts of the class look as follows:

```python
class CastleKilmereMember:
    """
    Creates a member of the Castle Kilmere School of Magic
    """

    def __init__(self, name: str, birthyear: int, sex: str):
        self._name = name
        self.birthyear = birthyear
        self.sex = sex
        self._traits = {}

    def exhibits_trait(self, trait):
        try:
            value = self._traits[trait]
        except KeyError:
            print(f"{self._name} does not have a character trait with the name '{trait}'")
            return

        if value:
            print(f"Yes, {self._name} is {trait}!")
        else:
            print(f"No, {self._name} is not {trait}!")

        return value
```


As visible in the definition of ```exhibit_trait()``` we have to catch and handle errors caused by querying the ```_traits``` dictionary with a non-existent key. Wouldn't it be much nicer if we could just return ```False``` in case a Castle Kilmere member does not possess a certain character trait? This can be achieved using ```collections.defaultdict```.

## ```collections.defaultdict```

```collections.defaultdict``` is a subclass of the general dictionary type. What makes ```defaultdict``` perfect for our problem is that it allows to specify a default value which is returned whenever a requested key cannot found. The basic usage of ```collections.defaultdict``` is as follows: 

```python
dict_ = defaultdict(default_factory)
```

if ```default_factory``` is not specified, i.e. if we just use ```dict_ = defaultdict()``` the dictionary will raise a ```KeyError``` whenever a requested key cannot be found. So we want to specify a default value.
    
Although we want to use ```False``` as our default value, we can't use ```dict_ = defaultdict(False)```. The reason for this is simple: ```defaultdict``` requires a callable (e.g. a function) as an argument that provides the default value when invoked without arguments. ```False``` is not callable. So we have to define a function that returns ```False``` when called without arguments:

```python
def return_false():
    return False

dict_ = defaultdict(return_false)
```

Alternatively, we could also specify a lambda function:

```python
dict_ = defaultdict(lambda: False)
```

The fact that ```defaultdict``` requires a callable makes it very powerful. We could create any kind of function and use the functions return value as a default. For our problem returning ```False``` is sufficient. Our adapted ```CastleKilmereMember``` class will look as follows:

```python
from collections import defaultdict

class CastleKilmereMember:
    """
    Creates a member of the Castle Kilmere School of Magic
    """

    def __init__(self, name: str, birthyear: int, sex: str):
        self._name = name
        self.birthyear = birthyear
        self.sex = sex
        self._traits = defaultdict(lambda: False)

    def add_trait(self, trait, value=True):
        self._traits[trait] = value

    def exhibits_trait(self, trait):

        value = self._traits[trait]

        if value:
            print(f"Yes, {self._name} is {trait}!")
        else:
            print(f"No, {self._name} is not {trait}!")

        return value

if __name__ == "__main__":

    bromley = CastleKilmereMember('Bromley Huckabee', '1959', 'male')

    bromley.add_trait('tidy-minded')
    bromley.add_trait('kind')

    bromley.exhibits_trait('kind')
    bromley.exhibits_trait('mean')

```




