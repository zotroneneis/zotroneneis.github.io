---
title: Immutable data classes
date: 2018-08-10
menu:
  sidebar:
    name: Immutable data classes
    identifier: immutable_data_classes
    parent: magical_universe
    weight: 14
---

**Topics:** Immutable data classes       
**Updated** 2020-10-05       
      
We have talked a lot about data classes in the [last post](http://alpopkes.com/posts/2018/08/coding-challenge-day-16-to-18/). There is one further characteristic of data classes that I would like to study - immutability.   
    
We can make a dataclass *immutable* such that it fulfills the same purpose as `typing.NamedTuple`. To make a dataclass immutable we have to set `frozen=True` when creating the class. Let's see how we can change our `DarkArmyMember` class from `typing.NamedTuple` to a dataclass without losing its functionality.   
   
Up to now our `DarkArmyMember` class looked as follows:

```python
class DarkArmyMember(NamedTuple):
    name: str
    birthyear: int

    @property
    def leader(self):
        master_odon = DarkArmyMember('Master Odon', 1971)
        return master_odon

    def cast(self, spell: str):
        print(f"{self.name}: {spell.incantation}!")
```

Once we instantiate a member of this class, we can't change it anymore. Running

```python
keres = DarkArmyMember('Keres Fulford', 1983)
keres.name = 'Adrien'
```

will raise `AttributeError: can't set attribute`. When converting the class to a data class we can keep all our code.

```python
@dataclass(frozen=True)
class DarkArmyMember:
    name: str
    birthyear: int

    @property
    def leader(self):
        master_odon = DarkArmyMember('Master Odon', 1971)
        return master_odon

    def cast(self, spell: str):
        print(f"{self.name}: {spell.incantation}!")
```

Let's make sure that the class is immutable. Running 

```python
keres = DarkArmyMember('Keres Fulford', 1983)
keres.name = 'Adrien'
```

will raise `dataclasses.FrozenInstanceError: cannot assign to field 'name'`. And we can still get a nice representation of the object without having to write our own `__repr__()` method. Running `print(keres)` will return `NewDarkArmyMember(name='Keres Fulford', birthyear=1983)`.   
   
Note: be careful with the datatypes of your fields. When a field contains a mutable datatype (for example a list) the field will stay mutable, even when setting `frozen=True` (this also holds for namedtuples, by the way). So when you want to have a truly immutable class, make sure that all fields use immutable data types (for example a tuple instead of a list). 


## Further reading
Data classes have further functionalities that we haven't discussed yet. If you want to know more about them, consider looking at [PEP 557](https://www.python.org/dev/peps/pep-0557/) or watching the [PyCon 2018 talk on dataclasses](https://www.youtube.com/watch?v=T-TwcmT6Rcw).

