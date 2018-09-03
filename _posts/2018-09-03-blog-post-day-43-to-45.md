---
title: 'Day 43 to 45 - Custom exception classes'
date: 2018-09-03
permalink: /posts/2018/08/coding-challenge-day-43-to-45/
tags:
  - python
  - coding
  - exceptions
---

**Topics:** Custom exception classes

## Errors and Exceptions

First of all, what is an exception? As outlined in the [Python docs](https://docs.python.org/3/tutorial/errors.html) there are two big kinds of errors: *syntax errors* and *exceptions*. 

**Syntax Error:**   
A syntax error is an error that makes it impossible to *parse* a program. That's why it's also called a *parsing error*. Whenever we made a mistake in the syntax of our code (for example, we might forgot a ':' somewhere) the parser will complain with a ```Syntax Error: invalid syntax```, pointing us to the offending line.
    
**Exception:**    
An exception is an error that is detected while a program is being executed. Python has several [built-in exceptions](https://docs.python.org/3/library/exceptions.html#bltin-exceptions) like ```ValueError``` or ```TypeError```. However, when creating a module, it can often be useful to create custom exception classes that are more specific than the generic exceptions built into Python.

## How to define a custom exception class

As mentioned in the Python docs, exceptions should be derived from the ```Exception``` class. Either directly as in:

```python
class MySpecificError(Exception):
  pass 
```

or indirectly by subclassing a more specific exception like ```ValueError```:
```python
class MySpecificError(ValueError):
  pass 
```

Custom exception classes can be very simple (like the ones above) or complex, providing additional details and containing methods. If you want to see how custom exception classes are used in larger projects, take a look at the [Open AI gym exception classes](https://github.com/openai/gym/blob/master/gym/error.py) or the [Python click package](https://github.com/pallets/click/blob/master/click/exceptions.py).


## When are custom exception classes useful?

Most of the time, the standard Python exception classes (like ```TypeError, ValueError``` etc.) are sufficient, especially when customizing them with a specific message. Consider the ```self.birthyear``` attribute of our ```CastleKilmereMember``` class. The birthyear is used for computing the current age of a Castle Kilmere member. So if ```birthyear``` has the wrong type, that is, it's not an integer like ```1991```, we won't get a valid age. So we should introduce a type check on the birthyear in our ```__init__()``` constructor:

```python
if type(birthyear) is not int:
    raise TypeError("The birthyear is not a valid integer. Try something like 1991")
```

However, when defining a module we might have several distinct errors in that module. In such a case it can be useful to create custom exception classes. In the example above we might throw a specific error like ```InvalidBirthyearError```. When creating custom exception classes it's common practice to define a base exception class for the module, and subclass specific exception classes for the different error conditions. We will create a file called ```error.py``` to define our exception classes and use them in our code.

```python
class BaseError(Exception):
    """Base class for exceptions in this module"""
    pass

class InvalidBirthyearError(BaseError):
    """Raised when the birthyear argument is not a valid integer like 1991"""
    pass
```

With this custom exception we could change our type check:

```python
import error
...

if type(birthyear) is not int:
    raise error.InvalidBirthyearError("The birthyear is not a valid integer. Try something like 1991")
```

## Potential benefits of custom exception classes

Custom exception classes can have several benefits:    
- When something goes wrong, error messages will be more specific and readable    
- Anyone using the code (teammates, clients, etc.) can be sure to catch all exceptions our module may raise if they catch ```BaseException```     
- If needed, more specific exceptions can be catched, giving problem-specific details       
- We (or the maintainers of the module) can add and modify existing exceptions without breaking existing client code     

<!-- Source: https://softwareengineering.stackexchange.com/questions/343262/is-it-better-to-have-many-specified-exceptions-or-some-general-that-are-raised-w -->


## Creating our own exception classes

There are other places of our Magical Universe in which custom exception classes might offer more clarity. Consider the first part of our ```exhibit_trait()``` function

```python
    ...

    def exhibits_trait(self, trait):
        try:
            value = self._traits[trait]
        except KeyError:
            print(f"{self._name} does not have a character trait with the name '{trait}'")
            return
        ...
```

If a person does not posses a trait, a ```KeyError``` will be raised which is catched and handled by our ```except``` block. Instead of just printing that a Castle Kilmere member does not possess a specific trait, we could also raise a ```TraitDoesNotExistError```. First, we have to define this exception class in our ```error.py``` file:

```python
class BaseError(Exception):
    """Base class for exceptions in this module"""
    pass

class InvalidBirthyearError(BaseError):
    """Raised when the birthyear argument is not a valid integer like 1991"""
    pass

class TraitDoesNotExistError(BaseError):
    pass

```

Now we can use the exception to tell the user precisely what happened and what he can do about it:

```python
    ...

    def exhibits_trait(self, trait):
        try:
            value = self._traits[trait]
        except KeyError:
            raise error.TraitDoesNotExistError(f"{self._name} does not possess a character trait with the name '{trait}'. Use the add_trait() function to add traits.")

            return
        ...
```

Keep in mind that these examples are not supposed to mean that we have to use custom exception classes in our code. You should stick to the built-in Python exceptions whenever they are sufficient. Use custom exception classes only when they are needed.




<!-- ## Real world examples -->
<!-- https://github.com/openai/gym/blob/master/gym/error.py -->
<!-- https://github.com/openai/gym/blob/master/gym/envs/atari/atari_env.py -->
<!-- https://github.com/pallets/click/blob/master/click/exceptions.py -->
<!-- https://github.com/requests/requests/blob/master/requests/exceptions.py -->
