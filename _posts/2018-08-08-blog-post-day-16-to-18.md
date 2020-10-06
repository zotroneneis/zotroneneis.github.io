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
**Updated** 2020-10-05     
            
Data classes are a feature that is new in Python 3.7. And taking a look at them is definitely worth it!

## Data classes
According to the [PEP](https://www.python.org/dev/peps/pep-0557/) on data classes, they are basically "mutable namedtuples with defaults". We already looked at namedtuples on [day 10 and 11](http://www.alpopkes.com/posts/2018/08/coding-challenge-day-10-and-11/). Namedtuples allow us to create an *immutable class* that primarily stores *values* (i.e. attributes). We used namedtuples for our `DarkArmyMember` class (because once you become a member of the dark army there is no way back. Unless, of course, you want to get killed by Master Odon).    
    
Data classes can do the same things as namedtuples. However, they make it much easier to create a class because a data class implements several useful methods by default. Let's create a data class and see what functionality it includes out of the box. We haven't specified the different departments of the Magical Universe yet so let's change that!

We can create a data class by using the `@dataclass` decorator. In case you are not using Python 3.7 you can add data classes to your Python 3.6 installation using `pip install dataclasses`.

```python
from dataclasses import dataclass

@dataclass
class Department:
    name: str
    head: Professor
    founded_in: int
    ghost: Ghost
```

We can create an instance of the `Department` class just as before:

```python
briddle = professor.briddle()
mocking_knight = ghost.mocking_knight()
law_department = department('Department of Law',
                            briddle,
                            785,
                            mocking_knight)
```

## Default functionality of data classes
When defining the `Department` class as above, Python automatically adds several [special methods](https://docs.python.org/3/glossary.html#term-special-method) to the class. For example, the class includes a `__init__()` that looks like this:

```python
def __init__(self, name: str, head: Professor, founded_in: int, ghost: Ghost):
    self.name = name
    self.head = head
    self.founded_in = founded_in
    self.ghost = ghost
```

That's nice, isn't it? All we had to do is list the attributes our `Department` class should have. Python took care of the rest! Of course, `__init__()` is not the only special method added to the class. For example, Python also added a `__repr__()` method to `Department`. So we can run 

```python
print(law_department)
```

and get a nice output right away: `Department(name='Department of Law', head=Professor(Birdie Briddle, birthyear: 1931, subject: Foreign Magical Systems), ghost=Ghost(The Mocking Knight, birthyear: 1401, year of death: 1492), founded_in=785)`. Remember: up to now we had to manually add a `__repr__()` method to our classes!   
   
Another example: we can automatically compare objects of the `Department` class. This usually involves implementing a custom `__eq__()` method (which can become quite complex). With data classes, `__eq__()` is implemented automatically. Let's test this by creating the science department.

```python
blade = Professor.blade()
scary_scoundrel = Ghost.scary_scoundrel()
science_department = Department('Department of Science',
                                blade,
				788,
                                scary_scoundrel)

print(law_department == science_department)
print(law_department == law_department)
```

As expected, these two expressions output ```False``` and ```True```. 


## Adding default values
We can easily add default values to the fields of our `Department` data class. For example, we could add a defalt value for the field `founded_in`. 

```python
@dataclass
class Department:
    name: str
    head: Professor
    founded_in: int = 785
    ghost: Ghost   
```

## Adding methods
Although data classes typically store mostly values, a data class is still a regular class. Therefore, we can freely add methods to our `Department` data class:

```python
import datetime

@dataclass
class Department:
    name: str
    head: Professor
    founded_in: int = 785
    ghost: Ghost

    def calc_age_in_years(self) -> int:
        now = datetime.datetime.now().year
        return now - self.founded_in
```

## Calling `@dataclass` with parameters

So far we have used the `@dataclass` decorator without any parameters. This corresponds to using the default values of the parameters. The parameters to `dataclass()` are (see [Python docs](https://docs.python.org/3/library/dataclasses.html) for full docstring):    
   
- init: If true (the default), a `__init__()` method will be generated   
- repr: If true (the default), a `__repr__()` method will be generated   
- eq: If true (the default), an `__eq__()` method will be generated   
- order: If true (the default is `False`), ordering methods will be generated. Ordering methods are: `__lt__(), __le__(), __gt__()`, and `__ge__()` which correspond to the operators `<, <=, >, >= `   
- unsafe_hash: If true (the default is `False`), a `__hash__()` method will be generated   
- frozen: If true (the default is `False`), fields are *frozen* so assigning to fields will raise an exception. We will talk more about this parameter on [day 19](http://www.alpopkes.com/posts/2018/08/coding-challenge-day-19/).     
   
So, for example, we can compare two departments to each other, when setting the `order` parameter to `True`:

```python
import datetime

class Department:
    name: str
    head: Professor
    founded_in: int = 785
    ghost: Ghost

    def current_age(self):
        now = datetime.datetime.now().year
        return (now - self.founded_in) + 1
```

Now running `print(law_department > science_department)` will work and produce the output `False`. Why False? Because data classes compare objects as if the objects were tuples of their fields. So `science_department` is "larger" than `law_department` because "L" comes before "S" in the alphabet.


## Full class definition

The full `Department` class will look as follows: 

```python
@dataclass()
class Department:
    name: str
    head: Professor
    founded_in: int = 785
    ghost: Ghost

    def calc_age_in_years(self) -> int:
        now = datetime.datetime.now().year
        return now - self.founded_in
```

The `head` and `ghost` field will point towards an instance of the `Professor` and `Ghost` class. For example, for the department of law we will create Professor Briddle and the Mocking Knight and reference those when instantiating the `Department` class. 


## Further reading
Data classes have further functionalities that we haven't discussed yet. If you want to know more about them, consider looking at [PEP 557](https://www.python.org/dev/peps/pep-0557/) or watching the [PyCon 2018 talk on dataclasses](https://www.youtube.com/watch?v=T-TwcmT6Rcw).
