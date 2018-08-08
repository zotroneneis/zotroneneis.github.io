---
title: 'Day 16, 17 and 18 - Data classes'
date: 2018-08-08
permalink: /posts/2018/08/coding-challenge-day-16-to-18/
tags:
  - python
  - coding
  - object_orientation
  - dataclasses
---

**Topics:** Data classes

Data classes are a feature that is new in Python 3.7. And taking a look at them is definitely worth it!

According to the [PEP](https://www.python.org/dev/peps/pep-0557/) on data classes, they are basically "mutable namedtuples with defaults". We already looked at namedtuples on [day 10 and 11](http://www.alpopkes.com/posts/2018/08/coding-challenge-day-10-and-11/). Namedtuples allow us to create an immutable class that primarily stores values (i.e. attributes). We used namedtuples for our ```DeathEater``` class (because once you become a death eather there is no way back. Unless, of course, you want to get killed by Voldemort).    
    
Data classes can do the same things as namedtuples. However, they make it much easier to create a class because a data class implements several useful methods by default. Let's create a data class and see what functionality it includes out of the box. We haven't specified the different houses of the Harry Potter universe yet so let's change that!

We can create a data class by using the ```@dataclass``` decorator. In case you are not using Python 3.7 you can add data classes to your Python 3.6 installation using ```pip install dataclasses```.

```python
from dataclasses import dataclass

@dataclass
class House:
    name: str
    founder: str
    traits: list
```

We can create an instance of the ```House``` class just as before:

```python
gryffindor = House('Gryffindor',
                   'Godric Gryffindor',
                   ['bravery', 'nerve', 'courage', 'chivalry', 'daring'])
```

## Default functionality of data classes

When defining the ```House``` class as above, Python automatically adds several [special methods](https://docs.python.org/3/glossary.html#term-special-method) to the class. For example, the class includes a ```__init__()``` that looks like:

```python
def __init__(self, name: str, founder: str, traits: list):
    self.name = name
    self.founder = founder
    self.traits = traits
```

That's nice, isn't it? All we had to do is list the attributes our ```House``` class should have. Python took care of the rest! Of course, ```__init__()``` is not the only special method added to the class. For example, Python also added a ```__repr__()``` method to ```House```. So we can run 

```python
print(gryffindor)
```

and get a nice output right away: ```House(name='Gryffindor', founder='Godric Gryffindor', traits=['bravery', 'nerve', 'courage', 'chivalry', 'daring'])```. Remember: up to now we had to manually add a ```__repr__()``` method to our classes!   
   
Another example: we can automatically compare objects of the ```House``` class. This usually involves implementing a custom ```__eq__()``` method (which can become quite complex). With data classes, ```__eq__()``` is implemented automatically. Let's test this by creating another Hufflepuff.

```python
hufflepuff = House('Hufflepuff',
                   'Helga Hufflepuff',
                   ['dedication', 'hardworking', 'fairness', 'patience', 'kindness', 'tolerance', 'loyalty'])

print(gryffindor == hufflepuff)
print(gryffindor == gryffindor)
```

As expected, these two expressions output ```False``` and ```True```. 


## Adding default values

We can easily add default values to the fields of our ```House``` data class. For example, we could add a field named 'founded_in'. Note: we don't know when Hogwarts was founded, but as discussed [here]() it was probably founded between 893 and 991. We will stick to the latter.

```python
@dataclass
class House:
    name: str
    founder: str
    traits: list
    founded_in: int = 991
```

## Adding methods

Although data classes typically store mostly values, a data class is still a regular class. Therefore, we can freely add methods to our ```House``` data class:

```python
import datetime

@dataclass
class House:
    name: str
    founder: str
    traits: list
    founded_in: int = 991

    def current_age(self):
        now = datetime.datetime.now().year
        return (now - self.founded_in) + 1
```

## Calling ```@dataclass``` with parameters

So far we have used the ```@dataclass``` decorator without any parameters. This corresponds to using the default values of the parameters. The parameters to ```dataclass()``` are (see [Python docs](https://docs.python.org/3/library/dataclasses.html) for full docstring):    
   
- init: If true (the default), a ```__init__()``` method will be generated   
- repr: If true (the default), a ```__repr__()``` method will be generated   
- eq: If true (the default), an ```__eq__()``` method will be generated   
- order: If true (the default is ```False```), ordering methods will be generated. Ordering methods are: ```__lt__(), __le__(), __gt__()```, and ```__ge__()``` which correspond to the operators ```<, <=, >, >= ```   
- unsafe_hash: If true (the default is ```False```), a ```__hash__()``` method is generated   
- frozen: If true (the default is ```False```), fields are *frozen* so assigning to fields will raise an exception. We will talk more about this parameter on [day 18](http://www.alpopkes.com/posts/2018/08/coding-challenge-day-18/).     
   
So, for example, we can compare two houses to each other, when setting the ```order``` parameter to ```True```:

```python
import datetime

@dataclass(order=True)
class House:
    name: str
    founder: str
    traits: list
    founded_in: int = 991

    def current_age(self):
        now = datetime.datetime.now().year
        return (now - self.founded_in) + 1
```

Now running ```print(hufflepuff < gryffindor)``` will work and produce the output ```False```. Why False? Because data classes compare objects as if the objects were tuples of their fields. So hufflepuff is "larger" than gryffindor because "G" comes before "H" in the alphabet.


## Gryffindor houses

The full ```House``` class will look as follows: 

```python
@dataclass
class House:
    name: str
    founder: str
    traits: list
    common_room: str
    head: Professor
    ghost: Ghost
    founded_in: int = 991

    def current_age(self):
        now = datetime.datetime.now().year
        return (now - self.founded_in) + 1
```

The ```head``` and ```ghost``` field will point towards an instance of the ```Professor``` and ```Ghost``` class. For example, for Gryffindor we will create Professor McGonagall and Nearly Headless Nick and reference those when instantiating the ```House``` class. See the [full code for day 16 to 18](https://github.com/zotroneneis/harry_potter_universe/blob/master/code_per_day/day_16_to_18.py) for all details.



## Further reading

Data classes have further functionality that we haven't discussed yet. If you want to know more about them, consider looking at [PEP 557](https://www.python.org/dev/peps/pep-0557/) or watching the [PyCon 2018 talk on dataclasses](https://www.youtube.com/watch?v=T-TwcmT6Rcw).

