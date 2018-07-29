---
title: 'Day 6 - Properties, setter and getter methods'
date: 2018-07-28
permalink: /posts/2018/07/coding-challenge-day-6/
tags:
  - python
  - coding
  - object_orientation
  - decorators
  - properties
---

**Topics:** properties, ```@property``` and ```property()```, setters, getters

Today, I digged a little deeper into the ```@property``` decorator, how it is related to the ```property()``` function and how its getter and setter methods work. These two links ([link1](https://www.programiz.com/python-programming/property), [link2](https://stackoverflow.com/questions/17330160/how-does-the-property-decorator-work)) were really helpful. Of course, there is also the [official Python docs](https://docs.python.org/3.7/howto/descriptor.html) on the ```property()``` function.

## The ```@property``` decorator

Yesterday we looked at decorators. The ```@property``` decorator allows us to create a \textit{read-only} property. Let's add a simple property to our HogwartsMember class.

```python
class HogwartsMember:
    """
    Creates a member of the Hogwarts School of Witchcraft and Wizardry
    """

    ...

    @property
    def age(self):
        now = datetime.datetime.now().year
        return now - self.birthyear
```

This creates a property called *age* that can be accessed using the dot syntax:   

```python
hagrid = HogwartsMember(name='Rubeus Hagrid', birthyear=1928, sex='male')
print(hagrid.age)
```

With the current year (2018) this yields the output ```90```. Of course, we can also set a global ```now``` variable in our code that gives more appropriate ages. Since the attribute is *read-only* we can't change it:

```python
hagrid = HogwartsMember(name='Rubeus Hagrid', birthyear=1928, sex='male')
hagrid.age = 44
```

yields an ```AttributeError: can't set attribute```. Of course, we can modify the behavior of ```@property```. Specifically, we can define how a property is accessed, set and deleted by defining its ```getter, setter``` and ```deleter``` methods.


## The relation between ```@property``` and ```property()```

What you should always have in mind when using decorators is their syntax. In particular, remember that

```python
@property
def age(self):
    now = datetime.datetime.now().year
    return now - self.birthyear
```

Is equivalent to
```python
def age(self):
    now = datetime.datetime.now().year
    return now - self.birthyear

age = property(age)
```

The full signature of the ```property()``` function is given by ```property(fget=None, fset=None, fdel=None, doc=None) -> property attribute```.   
- ```fget``` is a function for getting the value of the attribute   
- ```fset``` is a function for setting the value of the attribute   
- ```fdel``` is a function for deleting the attribute   
   
All these arguments are *optional*. So we can create a property object like we did above. But we can add extra "power" to it by specifying a setter, getter and/or deleter
method. For example, we could use the setter method to implement certain constraints on the property value. Let's say we add an attribute about the OWL's (Ordinary Wizarding Level's) to our Pupil class:

```python
class Pupil(HogwartsMember):

    def __init__(self, name:str, birthyear:int, house:str, start_year:int):
        ...
        self._owls = {'Study of Ancient Runes': False, 'Arithmancy': False, 'Astronomy': False, 'Care of Magical Creatures': False, 'Charms': False, 'Defence Against the Dark Arts': False, 'Divination': False, 'Herbology': False, 'History of Magic': False, 'Muggle Studies': False, 'Potions': False, 'Transfiguration': False}

    @property
    def owls(self):
        return self._owls
```

Now, if we want to update the OWL's of a student passed, we have to make sure that she/he actually passed the exam. Otherwise, the OWL can't be awarded. Let's implement that using a setter method.

```python
    @owls.setter
    def owls(self, subject_and_grade):

        try:
            subject, grade = subject_and_grade
        except ValueError:
            raise ValueError("Pass and interable with two items: subject and grade")

        passed = self.passed(grade)

        if not passed:
            raise ValueError('The exam was not passed so no OWL was awarded!')

        self._owls[subject] = True
```

Of course we can also delete the OWL's of a student. But we should probably make sure that the user knows what he is doing when stealing a student all of her/his exams!

```python
    @owls.deleter
    def owls(self):
        print("Caution, you are deleting this students' OWL's! "
              "You should only do that if she/he dropped out of school without passing any exam!")
        del self._owls
```
