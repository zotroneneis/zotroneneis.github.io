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

In the Python Tricks book I already learned about the functioning of decorators. I also knew that ```@property``` is a way of creating a read-only property. However, I was curious about its relation to ```property()``` and the setter and getter methods. The most important things I learned:

Creating a read-only property with ```@property``` is just a different way of using the ```property()``` function. So when considering our Harry Potter classes, using

```python
@property
def name(self):
    return self._name
```

Is equivalent to
```python
def name(self):
    return self._name

name = property(name)
```

The full signature of the ```property()``` function is ```property(fget=None, fset=None, fdel=None, doc=None) -> property attribute```. ```fget``` is a function for getting the value of the attribute, ```fset``` is a function for setting the value of the attribute and ```fdel``` is a function for deleting the attribute. All these arguments are *optional*. So we can create a property object like we did above. But we can add extra "power" to it by specifying a setter, getter and/or deleter
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

Of course we can also delete the OWL's of a student. But we should probably make sure that the user know what he is doing when stealing a student all of her/his exams!

```python
    @owls.deleter
    def owls(self):
        print("Caution, you are deleting this students' OWL's! "
              "You should only do that if she/he dropped out of school without passing any exam!")
        del self._owls
```
