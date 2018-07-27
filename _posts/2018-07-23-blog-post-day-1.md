---
title: 'Day 1 - creating the Harry Potter universe'
date: 2018-07-23
permalink: /posts/2018/07/coding-challenge-day-1/
tags:
  - python
  - coding
  - object_orientation
---

**Topics:** object oriented programming, classes, inheritance

I want to start with using some of the stuff I learned from the "Python Tricks" book (see my reading list for more details on the book). Therefore, I will start creating a little 'Harry Potter universe' with classes and methods related to the Harry Potter world (I LOVE Harry Potter). 

So let's start with the most important basics.

## What is Object Oriented Programming ?

Object-oriented programming (OOP) is a specific programming paradigm. In OOP computer programs are designed by creating *objects* that interact with each other. Each object can contain:    
a) attributes    
b) behavior in the form of methods      
     
Most OOP programming languages, including Python, are *class-based*. This means that objects are instances of *classes*. This might sound quite abstract. But when you think about it, this is exactly how our world looks like. We have lots of classes and instances of these classes. For example, we have the class "Person" and lots of individual persons (like you and me) that are *instances* of the "Person" class. An attribute of the "Person" class might be "name" or "birthdate". A method might be "current_age" that computes the current age of the person.   
    
That's what I like about OOP: it reflects the structure of our world quite closely. This makes it easier to understand the concept.

## Classes in Python

A class acts as a *blueprint* for an object. It describes how objects (i.e. members) of a class are structured, what attributes an object of the class has and what methods / functionality. The syntax for creating an empty class is quite simple:

```python
class HogwartsMember:
    pass
```

Side note: ```pass``` is a Python keyword that can be used as a placeholder. It usually denotes places where we will eventually put code. It allows us to run the code above without getting an error

## Creating class objects

To create an actual object, i.e. an *instance* of the class we need to *instantiate* the class. This is also simple:

```python
hogwarts_member = HogwartsMember()
```

## Adding attributes and methods

So far our "HogwartsMember" class is boring. We need to add some attributes and methods to make it more interesting.

```python
class HogwartsMember:
    """
    Creates a member of the Hogwarts School of Witchcraft and Wizardry
    """

    def __init__(self, name, birthyear, sex):
        self._name = name
        self.birthyear = birthyear
        self.sex = sex
        
    def says(self, words):
        return f"{self._name} says {words}"

hagrid = HogwartsMember('Rubeus Hagrid', '1928', 'male')
print(hagrid.says("Hello!"))

```

This class has a method called ```__init__```. The *init* method is called whenever you create a new instance of the class. So when calling ```hagrid = HogwartsMember('Rubeus Hagrid', '1928', 'male')``` the ```__init__``` method of the ```HogwartsMember``` class is called with the arguments ```'Rubeus Hagrid', '1928', 'male'``` which represent the name, birthyear and sex of the Hogwarts member. The ```__init__``` method returns an instance of the class which is then assigned to
a variable called "hagrid".
   
Note: The first argument of the ```__init__``` method is called 'self'. This argument will be the first argument in most methods. It points towards an instance of the class whenever the method is called. For more details on this, see the [blog post of day 4]() or the [Python docs](https://docs.python.org/3/tutorial/classes.html).   
   
We also added a method called ```says``` that adds behavior to our class. In this case, it allows our Hogwarts member to say something.

## Inheritance

The HogwartsMember class is nice, but of course we want many other classes in our Harry Potter universe. For example, we want to create pupils, professors, ghosts, etc. But all of these are members of Hogwarts, right? This is what *inheritance* is used for.   
   
Inheritance allows us to create a new class that *inherits* all attributes and methods from the *parent class*. The resulting *child class* can override methods and attributes of the parent class and it can add new
functionality. Let's use the concept of inheritance to create a ```Pupil``` class!

```python
class Pupil(HogwartsMember):
    """
    Create a Hogwarts Pupil
    """

    def __init__(self, name, birthyear, sex, house, start_year, pet=None):
        super(Pupil, self).__init__(name, birthyear, sex)
        self.house = house
        self.start_year = start_year

        if pet is not None:
            self.pet_name, self.pet_type = pet

        self._owls = {
                'Study of Ancient Runes': False,
                'Arithmancy': False,
                'Astronomy': False,
                'Care of Magical Creatures': False,
                'Charms': False,
                'Defence Against the Dark Arts': False,
                'Divination': False,
                'Herbology': False,
                'History of Magic': False,
                'Muggle Studies': False,
                'Potions': False,
                'Transfiguration': False}


harry = Pupil(name='Harry James Potter',
              birthyear=1980,
              sex='male',
              house='Griffindor',
              start_year=1991,
              pet=('Hedwig', 'owl'))
```

In this new class we use the method ```super``` to call the ```init``` method of the parent class. Then, we add new attributes to the class. These attributes will be specific to object instances of the ```Pupil``` class. A pupil has more attributes than a simple Hogwarts member. For example, a pupil belongs to one of the Hogwarts houses and she/he started school in a specific year. Also, a pupil might own a pet.    
   
Furthermore, we added an attribute "owl". This attribute will be used later. It contains all the classes a pupil might take. When creating a new pupil, she/he won't have passed any OWL (Ordinary Wizarding Level) yet. But this might change!


## Instance and class attributes

A Python object can have two types of attributes:     
a) Instance attributes    
b) Class attributes    

All the attributes in the examples above are *instance attributes*:    
- Instance attributes are tied to a particular object instance
- The contents of an instance variable (e.g. the actual name of a HogwartsMember) are stored on the instance itself, not on the class. For example, the content of the *name* attribute of the instance *hagrid* is *Rubeus Hagrid*. Other instances of the HogwartsMember class will have different names.   
- So the contents of an instance variable are independent from one object to the next!   
- When you modify the contents of an instance variable it will only affect the particular object instance, not any of the other objects

A *class attribute* is created inside the class definition. Let's add a class attribute to the HogwartsMember class

```python
class HogwartsMember:
    """
    Creates a member of the Hogwarts School of Witchcraft and Wizardry
    """
    
    # class attribute
    location = 'England'

    def __init__(self, name, birthyear, sex):
        ...

```

Class attributes:   
- Are not tied to any particular instance of the class   
- Instead, all instance of the class share access to the same set of class variables   
- When you modify a class variable, all instances of the class will be affected at the same time. 

We can access and modify class attributes both with and without an instance of the class:

```python
hagrid = HogwartsMember('Rubeus Hagrid', '1928', 'male')
print(hagrid.location)
print(HogwartsMember.location)
```

## Further reading:   
The [Python documentation](https://docs.python.org/3/tutorial/classes.html) contains an entire chapter on classes.

## All code for today
```python
class HogwartsMember:
    """
    Creates a member of the Hogwarts School of Witchcraft and Wizardry
    """

    def __init__(self, name, birthyear, sex):
        self._name = name
        self.birthyear = birthyear
        self.sex = sex
        
    def says(self, words):
        return f"{self._name} says {words}"


class Pupil(HogwartsMember):
    """
    Create a Hogwarts Pupil
    """

    def __init__(self, name, birthyear, sex, house, start_year, pet=None):
        super(Pupil, self).__init__(name, birthyear, sex)
        self.house = house
        self.start_year = start_year

        if pet is not None:
            self.pet_name, self.pet_type = pet

        self._owls = {
                'Study of Ancient Runes': False,
                'Arithmancy': False,
                'Astronomy': False,
                'Care of Magical Creatures': False,
                'Charms': False,
                'Defence Against the Dark Arts': False,
                'Divination': False,
                'Herbology': False,
                'History of Magic': False,
                'Muggle Studies': False,
                'Potions': False,
                'Transfiguration': False}

class Professor(HogwartsMember):
  """
  Creates a Hogwarts professor
  """

  def __init__(self, name, birthyear, sex, subject, house=None):
      super(Professor, self).__init__(name, birthyear, sex)
      self.subject = subject
      if house is not None:
          self.house = house

    
class Ghost(HogwartsMember):
    """
    Creates a Hogwarts ghost
    """

    def __init__(self, name, birthyear, sex, year_of_death, house=None):
        super(Ghost, self).__init__(name, birthyear, sex)
        self.year_of_death = year_of_death

        if house is not None:
            self.house = house



if __name__ == "__main__":
  hagrid = HogwartsMember('Rubeus Hagrid', '1928', 'male')
  print(hagrid.says("Hello!"))

  harry = Pupil(name='Harry James Potter',
                birthyear=1980,
                sex='male',
                house='Griffindor',
                start_year=1991,
                pet=('Hedwig', 'owl'))

```

<!---
## Old text
I want to start with using some of the stuff I learned from the "Python Tricks" book (see my reading list for more details on the book). Therefore, I will start creating a little 'Harry Potter universe' with classes and methods related to the Harry Potter world (I LOVE Harry Potter). Today my code includes the following concepts:   
- Object orientation / two classes: HogwartsMember, Pupil
- I used inheritance for the pupil class. This means that the Pupil class *inherits* all attributes and methods from its parent class
- I used the @property decorator (see [day 2](http://alpopkes.com/posts/2018/07/blog-post-3/) for details on @property)
- I created a static method (see [day 4](http://alpopkes.com/posts/2018/07/blog-post-5/) for details on static methods)
- I used **function annotations**. Function annotations are a very cool Python 3 feature. They allow us to add arbitrary metadata to function arguments and the return value of a function. Why this is useful? First of all, it allows you to document of what type your function parameters are. Furthermore, they can be used for things like type checking. For more use cases, look [here](https://www.python.org/dev/peps/pep-3107/). For the syntax of function annotations see [day 3](http://alpopkes.com/posts/2018/07/blog-post-4/). 
-->
