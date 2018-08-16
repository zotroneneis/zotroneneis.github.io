---
title: 'Day 19 - Immutable data classes'
date: 2018-08-10
permalink: /posts/2018/08/coding-challenge-day-19/
tags:
  - python
  - coding
  - object_orientation
  - dataclasses
---

**Topics:** Immutable data classes

We have talked a lot about data classes in the [last post](http://alpopkes.com/posts/2018/08/coding-challenge-day-16-to-18/). There is one further characteristic of data classes that I would like to study - immutability.   
    
We can make a dataclass *immutable* such that it fulfills the same purpose as ```typing.NamedTuple```. To make a dataclass immutable we have to set ```frozen=True``` when creating the class. Let's see how we can change our ```DeathEater``` class from ```typing.NamedTuple``` to a dataclass without losing its functionality.   
   
Up to now our ```DeathEater``` class looked as follows:

```python
class DeathEater(NamedTuple):
    """ Creates a death eater """
    name: str
    birthyear: str

    @property
    def leader(self):
        voldemort = DeathEater('Voldemort', 1926)
        return voldemort

    def __repr__(self):
        return f"{self.__class__.__name__}({self.name}, birthyear: {self.birthyear})"

    def cast(self, spell):
        print(f"{self.name}: {spell.incantation}!")
```

Once we instantiate a member of this class, we can't change it anymore. Running

```python
lucius = DeathEater('Lucius Malfoy', 1953)
lucius.name = 'Draco'
```

will raise ```AttributeError: can't set attribute```. When converting the class to a data class we can keep most of the code. However, we won't need the ```__repr__()``` method anymore!

```python
@dataclass(frozen=True)
class NewDeathEater():
    """ Creates a death eater """
    name: str
    birthyear: str

    @property
    def leader(self):
        voldemort = DeathEater('Voldemort', 1926)
        return voldemort

    def cast(self, spell):
        print(f"{self.name}: {spell.incantation}!")
```

Let's make sure that the class is immutable. Running 

```python
lucius = NewDeathEater('Lucius Malfoy', 1953)
lucius.name = 'Draco'
```

will raise ```dataclasses.FrozenInstanceError: cannot assign to field 'name'```. And we can still get a nice representation of the object without having to write our own ```__repr__()``` method. Running ```print(lucius)``` will return ```NewDeathEater(name='Lucius Malfoy', birthyear=1953)```.  
   
Note: be careful with the datatypes of your fields. When a field contains a mutable datatype (for example a list) the field will stay mutable, even when setting ```frozen=True```. So when you want to have a truly immutable class, make sure that all fields use immutable data types (for example a tuple instead of a list). 


## Further reading

Data classes have further functionalities that we haven't discussed yet. If you want to know more about them, consider looking at [PEP 557](https://www.python.org/dev/peps/pep-0557/) or watching the [PyCon 2018 talk on dataclasses](https://www.youtube.com/watch?v=T-TwcmT6Rcw).

