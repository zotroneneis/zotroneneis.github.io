---
title: Decorators
date: 2018-07-27
menu:
  sidebar:
    name: Decorators
    identifier: decorators
    parent: magical_universe
    weight: 7
---

**Topics:** decorators     
**Updated** 2020-10-04     

Today, we are going to look at *decorators*. Python's decorators are an advanced concept so don't worry if you don't immediately understand how they work. The more you will use and read about them, the clearer the concept will become. I won't go into too much detail here, so if you want to know more about decorators, take a look at [Dan Bader's website](https://dbader.org/blog/python-decorators) or the [PEP on decorators](https://www.python.org/dev/peps/pep-0318/#on-the-name-decorator).   
   
## What are decorators?
In simple terms a decorator is a callable that takes a callable as an input and returns a callable. What is a callable? Typically, when we're talking about callables we mean functions or (callable) classes. For simplicity, I will stick to functions from now on. But you could equally decorate any other callable.

Decorators allow us to extend and/or modify the behavior of the function that they take as an input. And they do that without permanently modifying this 'input' function itself! The functions behavior is changed *only* when it is decorated.  
  
This sounds very abstract so let's look at an example. The simplest decorator would be one that returns its input function:   
```python
def useless_decorator(function):
    return function
```

We can apply the decorator to a function by 'wrapping' it:   
```python
def say_hello() -> str:
    return "Hey there!"

say_hello = useless_decorator(say_hello)
```

A shortcut for wrapping/decorating a function is given by the `@decorator` syntax. Instead of wrapping our function as above (that is, by calling `say_hello = useless_decorator(say_hello)`) we can say:

```python
@useless_decorator
def say_hello() -> str:
    return "Hey there!"
```

Right now our decorator only returns its input function. Therefore, the behavior or output of `say_hello()` isn't changed at all. This is not particularly useful. So let's take a look at how we can extend and/or modify the behavior of the wrapped function.

## Modifying the wrapped function's behavior
When we want to modify the behavior of the wrapped function, our decorator must be a little more complex. Specifically, the decorator must define a new function (called the 'wrapper' function). This new 'wrapper' function is then used to wrap the input function and modify the input function's behavior. An example might look as follows:

```python
def goodbye(function):
    def wrapper():
        original_output = function()
        new_output = original_output + f" Goodbye, have a good day!"
        return new_output
    return wrapper

@goodbye
def say_hello() -> str:
    return f"Hey there!"
```

Now, the output of `print(say_hello())` will be: `Hey there! Goodbye, have a good day!`.

## Wrapping functions that take input arguments
Of course we want to be able to decorate all kinds of functions, not only ones that don't take any input arguments. Consider a function similar to the `says` function of our `CastleKilmereMember` class:  

```python
def say_words(person: str, words: str) -> str:
    return f"{person} says: {words}"

```
Calling `print(say_words("Lissy", "Hey Luke!"))` results in `Lissy says: Hey Luke!`.
   
How can we decorate this function? We somewhat need to make sure that our `goodbye` wrapper function can process the arguments `person` and `words`. This is not that hard. We simply use `*args` and `**kwargs` to collect all positional and keyword arguments and forward them to the orginal input function:

```python
def goodbye(function):
    def wrapper(*args, **kwargs):
        original_output = function(*args, **kwargs)
        new_output = original_output + f" Goodbye, have a good day!"
        return new_output
    return wrapper

@goodbye
def say_words(person, words):
    return f"{person} says: {words}"
```

Now calling `print(say_words("Lissy", "Hey Luke!"))` results in `Lissy says: Hey Luke! Goodbye, have a good day!`
   
As mentioned in the beginning decorators are a complex topic. So far, we have only scratched the surface. Tomorrow, we will look at the `@property` decorator which is often used in classes. However, decorators can be useful in many contexts. So make sure to read more about them and try to use them in your own code!   

## Why are decorators called decorators?
Decorators are called decorators because they "decorate" other functions and allow us to run code before and after the wrapped function is executed.


