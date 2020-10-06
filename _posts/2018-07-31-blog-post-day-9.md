---
title: 'Day 9 - Duck typing, EAFP principle'
date: 2018-07-31
permalink: /posts/2018/07/coding-challenge-day-9/
tags:
  - python
  - coding
  - object_orientation
  - duck_typing
  - EAFP
---

**Topics:** Duck typing, EAFP principle     
**Updated** 2020-10-04     

## Duck typing
Today we are going to look at some fundamental philosophies of programming: duck typing, EAFP and its opposite LBYL. Some of you may have heard of these principles already. However, because they are so fundamental we want to make sure that we apply them to our magical universe correctly.

So what is duck typing? The most common saying regarding duck typing is “If it walks like a duck, and it quacks like a duck, then it must be a duck.” But what the heck does this mean? I think it's easiest to explain this by an example.

Let's say our `Pupil` and our `Ghost` classes have a method called `fly`. Lets further say that we have a method `lets_fly` which gets an object as an input, checks whether that object is a Ghost and, if that's the case, calls the fly method on the object. If the given object is NOT a ghost, the method will print that you have to be a ghost in order to fly. Next, we create Lissy, one of our pupils, and the Mocking Knight, one of Castle Kilmeres ghosts. 

```python
class Pupil(CastleKilmereMember):
    ...
    def fly(self):
    	print(f"{self.name}: I'm flying!")

class Ghost(CastleKilmereMember):
    ...
    def fly(self):
    	print(f"{self.name}: Whooooosh!")

def lets_fly(thing):
    if isinstance(thing, Ghost):
    	thing.fly()
    else:
    	print("You have to be a ghost in order to fly!")

lissy = Pupil.lissy()
mocking_knight = Ghost('The Mocking Knight', 1401, 'male', 1492)
```

Now, what is the output of calling `lets_fly` on the Mocking Knight versus calling it with Lissy as an input? As we can see below, the first statement makes the Mocking Knight fly, whereas the second does not work. Is this the behaviour we want? I don't think so. After all, pupils can fly, too! They just need a broomstick to do so. This is where duck typing comes into place

```
lets_fly(mocking_knight)
>>> The Mocking Knight: Whooooosh!

lets_fly(lissy)
>>> You have to be a ghost in order to fly!
```

**Duck typing** is used to determine if an object can be used for a certain purpose. To do this not the *type* of the object is important but the *methods and properties* it has. This is precisely what we just talked about with the ghosts and pupils. It doesn't matter if you are a ghost or a pupil as long as you have a method that allows you to fly! With this knowledge we can rewrite our code as follows:

```python
def lets_fly(thing):
    thing.fly()

lets_fly(mocking_knight)
>>> The Mocking Knight: Whooooosh!

lets_fly(lissy)
>>> Lissy Spinster: I'm flying!
```

But wait a second. Don't we have to check if the fly method exists to make sure that no errors occur? Absolutely! This is where EAFP, a principle closely related to Duck Typing comes into play.

## Easier to ask for forgiveness than permission
EAFP is short for "easier to ask for forgiveness than permission". EAFP is a common coding style that is not unique to Python but used in several other programming languages. The main idea behind EAFP is to assume that valid keys or attributes exist, instead of checking that they exist first and only afterwards running the code. If this assumption proves false, we catch and handle the exceptions that are raised. We actually used this principle already. Can you guess where? Take a close look at the `@elms.setter` method of the `Pupil` class:

```python
@elms.setter
def elms(self, subject_and_grade):
    try:
        subject, grade = subject_and_grade
    except ValueError:
        raise ValueError("Pass and iterable with two items: subject and grade")

    passed = self.passed(grade)

    if passed:
        self._elms[subject] = True
    else:
        print('The exam was not passed so no ELM was awarded!')
```

The `try ... except ...`  code block implements the EAFP principle. We assume that the `subject_and_grade` tuple exists. If it does, we unpack its values into `subject, grade`. If `subject_and_grade` is not a valid tuple, a `ValueError` will be raised. We catch this error in the `except` block and handle it appropriately.  

The EAFP implementation of our `lets_fly` method would look as follows:
```python
def lets_fly(thing):
    try:
    	thing.fly()
    except (AttributeError, TypeError) as e:
    	print(e)

lets_fly(mocking_knight)
>>> The Mocking Knight: Whooooosh!

lets_fly(lissy) 
>>> Lissy Spinster: I'm flying!

lets_fly(bromley)
>>> 'CastleKilmereMember' object has no attribute 'fly'
```

As we can see in the example above, we get exactly the behavior we want. Both the Mocking Knight and Lissy can fly, whereas Bromley, who is way too old to fly on a broomstick, is not able to fly.

To add another example of the EAFP principle to our Magical Universe we first extend our `CastleKilmereMember` class with a `_traits` attribute and two methods for adding and printing traits:

```python
class CastleKilmereMember:
    """ Creates a member of the Castle Kilmere School of Magic """
    def __init__(...):
    	...
        self._traits = {}

    def add_trait(self, trait, value=True):
        self._traits[trait] = value

    def print_traits(self):
        true_traits = [trait for trait, value in self._traits.items() if value]
        false_traits = [trait for trait, value in self._traits.items() if not value]

        if true_traits:
            print(f"{self._name} is {', '.join(true_traits)}")
        if false_traits:
            print(f"{self._name} is not {', '.join(false_traits)}")
        if (not true_traits and not false_traits):
            print(f"{self._name} does not have traits yet")
    ...
```

Let's use this code to add traits to Bromley, Castle Kilmeres loyal caretaker. As we all know, Bromley is a very kind man, who is tidy-minded (after all, he's a caretaker). But if there is something Bromley is NOT it's impatient. We can now account for Bromleys personality by adding the different traits to the bromley instance:

```python
bromley = CastleKilmereMember(name='Bromley Huckabee', birthyear=1959, sex='male')
bromley.add_trait("kind")
bromley.add_trait("tidy-minded")
bromley.add_trait("impatient", value=False)
```

For our new `_traits` attribute we also need a method that checks if a Castle Kilmere member exhibits a certain character trait or not. The method should accept a trait as a string and should return whether or not the person in question possesses that trait. An implementation using EAFP would look as follows:

```python
def exhibits_trait(self, trait: str) -> bool:
    try:
        value = self._traits[trait]
        return value
    except KeyError as e:
    	print(f"{self.name} does not have a character trait with the name {e}")
        return False
```

The `try ... except ...` code block assumes that the given trait exists as a key. If this assumption proves false (i.e. a `KeyError` is raised) it handles the error.

## Look before you leap (LBYL)
The opposite of the EAFP principle is called 'Look before you leap' (LBYL). When using LBYL we would first test if a key exists before performing the look-up operation. So our code from above would rather look as follows:

```python
def exhibits_trait(self, trait: str) -> bool:
    if trait in self._traits:
        value = self._traits[trait]
        return value
    else:
        print(f"{self._name} does not have a character trait with the name '{trait}'")
        return False
```
As mentioned in [this stackoverflow post](https://stackoverflow.com/questions/11360858/what-is-the-eafp-principle-in-python): "The LBYL version has to search the key inside the dictionary twice, and might also be considered slightly less readable."

## `dict.get()`
Before closing this section I want to mention that an even better solution for our `exhibits_trait` function exists. This solution makes use of the `dict.get()` function. The `dict.get()` function can be used to look up a key in a dictionary and return a certain value as a default if the requested key cannot be found. So we wouldn't need a try/except block for the `exhibits_trait` function at all!

```python
class CastleKilmereMember:

    def __init__(...):
    	...
        self._traits = {}

    def add_trait(self, trait: str, value=True):
        self._traits[trait] = value
			
    def exhibits_trait(self, trait: str) -> bool:
    	value = self._traits.get(trait, False)
        return value
```




