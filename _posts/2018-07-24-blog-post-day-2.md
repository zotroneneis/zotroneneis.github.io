---
title: 'Day 2 - Types of class methods'
date: 2018-07-24
permalink: /posts/2018/07/coding-challenge-day-2/
tags:
  - python
  - coding
  - object_orientation
  - class_methods
  - instance_methods
  - static_methods
---

**Topics:** class methods, instance methods, static methods, using class methods as alternative constructors    
**Updated** 2020-10-03   

## Types of methods
A class can have three types of methods: instance methods, class methods and static methods.
   
**Instance methods** are the most common type of method. They take at least the parameter *self* as an input. This parameter points towards an instance of the class when the method is called. An instance method can modify both *object state* (through the `self` parameter) and *class state* indirectly (through the `self.__class__` parameter).

Our base class already has an instance method. The `says` methods takes the input `self` and a string of words the current member of Castle Kilmere should say. When the `says` method is called on an object, the `self` parameter can be used to access attributes which were set in the dunder init method.

```python
def says(self, words):
    return f"{self.name} says {words}"
```

**Class methods** look similar to instance methods in the sense that they take at least one parameter as an input. This parameter is, however, not `self` but `cls`. `cls` points towards the *class* - not a particular object instance - when the method is called. Therefore, a class method can only modify *class state* but not *object state*. Still, changing the class state will still affect all instances of the class. Another important thing to know about classmethods is that we need to use the `@classmethod` decorator when implementing a class method. We will talk about decorators in a later blog post. Right now you only need to remember to put a `@classmethod` on top of the function. An example for a class method can be seen below (see section on alternative constructors).
   
**Static methods** take neither a `self` nor `cls` parameter as an input. Therefore, they can modify neither object state nor class state. Although they are related to the class, they are yet independent and can only access the data they are provided with.   

Do you remember the Pupil class? It had an attribute called `_elms`, standing for "Elementarey Level of Magic". This attribute contains all the obligatory classes a pupil has to take. If a student passes one of her ELMs or not depends on the grade she gets. Castle Kilmere has a fixed grading scheme where grades range from 'E' for 'Excellent' to 'H' for 'Horrible'. This is perfect material for a static method!

Our static method `passed` will receive a grade as an input and will return whether or not this means pass or fail. Notice that the method uses no class or instance state but only has access to the attributes its provided with (which is the grade in this case).

```python
class Pupil(CastleKilmereMember):
    """ Create a Castle Kilmere Pupil """
    ...

    @staticmethod
    def passed(grade):
        """ Given a grade, determine if an exam was passed.  """
        grades = {
		'E': True,
		'Excellent': True,
                'O': True,
                'Ordinary': True,
                'A': True,
                'Acceptable': True,
                'P': False,
                'Poor': False,
                'H': False,
                'Horrible': False,
                }

        return grades[grade]
```

## What are class and static methods good for?
Class and static methods allow a developer to communicate what intention she had in mind when implementing the method. For example, using a static method expresses that the method is independent from the rest of the class. Also, class methods can be used as **alternative constructors**.   
   
## Class methods as alternative constructors   
- In Python a class can only have one constructor (the `self.__init__()` method)
- However, we can use `@classmethod` to create factory functions
- These factory functions allow us to create class objects that are configured exactly the way we want them
- In this way, they act like additional constructors
   
For example, we probably want to create the main characters of our Magical Universe. Typing all the names and other attributes every time would be very slow. Adding a few class methods allows us to speed up the process. We can create a Lissy constructor as follows:
   
```python
@classmethod
def lissy(cls):
    return cls('Lissy Spinster',
                2008,
		'female',
		2020,
		('Ramses', 'cat'))
```

## All code for today
```python
class CastleKilmereMember:
    """ Creates a member of the Castle Kilmere School of Magic """
    def __init__(self, name, birthyear, sex):
        self.name = name
        self.birthyear = birthyear
        self.sex = sex
        
    def says(self, words):
        return f"{self.name} says: {words}"
    
    @classmethod
    def school_headmistress(cls):
        return cls('Miranda Mirren', 1962, 'female')   


class Pupil(CastleKilmereMember):
    """ Create a Castle Kilmere Pupil """
    def __init__(self, name, birthyear, sex, start_year, pet=None):
        super().__init__(name, birthyear, sex)
        self.start_year = start_year

        if pet is not None:
            self.pet_name, self.pet_type = pet

        self._elms = {
                  'Critical Thinking': False,
                  'Self-Defense Against Fresh Fruit': False,
                  'Broomstick Flying': False,
                  'Magical Theory': False,
                  'Foreign Magical Systems': False,
                  'Charms': False,
                  'Defence Against Dark Magic': False,
                  'History of Magic': False,
                  'Potions': False,
                  'Transfiguration': False}
    
    @classmethod
    def luke(cls):
        return cls('Luke Bery', 2008, 'male', 2020, ('Cotton', 'owl'))

    @classmethod
    def lissy(cls):
        return cls('Lissy Spinster', 2008, 'female', 2020, ('Ramses', 'cat'))

    @classmethod
    def adrien(cls):
        return cls('Adrien Fulford', 2008, 'male', 2020, ('Unnamed', 'owl') )



class Professor(CastleKilmereMember):
    """ Create a Castle Kilmere Professor """
    def __init__(self, name, birthyear, sex, subject, department):
        super().__init__(name, birthyear, sex)
        self.subject = subject
        self.department = department

    @classmethod
    def blade(cls):
        return cls('Blade Bardock', 1988, 'male', 'Potions', 'Science')

    @classmethod
    def briddle(cls):
        return cls('Birdie Briddle', 1931, 'female', 'Foreign Magical Systems', 'Law')

    @classmethod
    def radford(cls):
        return cls('Rupert Radford', 1958, 'male', 'Illusions 101', 'Creativity and Arts')

    @classmethod
    def giddings(cls):
        return cls('Gabriel Giddings', 1974, 'male', 'Broomstick Making', 'Engineering')

    
class Ghost(CastleKilmereMember):
    """ Creates a Castle Kilmere ghost """
    def __init__(self, name, birthyear, sex, year_of_death):
        super().__init__(name, birthyear, sex)
        self.year_of_death = year_of_death

if __name__ == "__main__":

    blade = Professor.blade()
    lissy = Pupil.lissy()
```
