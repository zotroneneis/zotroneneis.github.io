---
title: functools.wraps - avoiding losing metdata when applying decorators
date: 2018-09-06
menu:
  sidebar:
    name: functools.wraps
    identifier: functools_wraps
    parent: magical_universe
    weight: 23
---

**Topics:** `functools.wraps`

We have discussed decorators on [day 5](http://alpopkes.com/posts/2018/07/coding-challenge-day-5/), [day 6](http://alpopkes.com/posts/2018/07/coding-challenge-day-6/) and [day 20](http://alpopkes.com/posts/2018/08/coding-challenge-day-20/) already. Therefore, we know that decorators allow us to extend or modify the behavior of a function *without permanently modifying the function itself*. In our example we modified the `says()` function of our `CastleKilmereMember` class to be whispering instead of talking normally:

```python
class CastleKilmereMember:
    """ Creates a member of the Castle Kilmere School of Magic """
    def __init__(self, name: str, birthyear: int, sex: str):
        self.name = name
        self.birthyear = birthyear
        self.sex = sex
        self._traits = {}

    def whisper(function):
        def wrapper(self, *args):
            ''' Whispering decorator '''
            original_output = function(self, *args)
            first_part, words = original_output.split(' says: ')
            words = words.replace('!', '.')
            new_output = f"{first_part} whispers: {words}.."
            return new_output
        return wrapper

    @whisper
    def says(self, words: str) -> str:
        '''Allows a Castle Kilmere Member to talk'''
        return f"{self.name} says: {words}"
```

When applying a decorator we replace one function with another. In our case, we replaced the `says()` function with the `whisper()` function. This has consequences. For example, the metadata attached to our "original" function is replaced by the metdata of the new function: when looking at the name or docstring of the `says()` function we won't get the result one would expect. The output of 

```python
bromley = CastleKilmereMember('Bromley Huckabee', 1959, 'male')
print(bromley.says.__name__)
```

is `wrapper`, not `says`. The same holds for the docstring:

```python
print(bromley.says.__doc__)
```

outputs `Whispering decorator`. But we don't want to lose any information associated with our original `says()` function! If applying a decorator corresponds to losing information about a function, we would have a serious problem. Luckily, there is a solution: the [`functools` module](https://docs.python.org/3/library/functools.html). Applying `functools.wraps` to the wrapper function carries over the name, docstring, arguments list, etc. of the original input function:

```python
def whisper(function):
    @functools.wraps(function)
    def wrapper(self, *args):
        ''' Whispering decorator '''
        original_output = function(self, *args)
        first_part, words = original_output.split(' says: ')
        words = words.replace('!', '.')
        new_output = f"{first_part} whispers: {words}.."
        return new_output
    return wrapper
```

Now 

```python
print(bromley.says.__name__
print(bromley.says.__doc__)
```

returns the desired result, namely `says` and `Allows a Castle Kilmere Member to talk`.


