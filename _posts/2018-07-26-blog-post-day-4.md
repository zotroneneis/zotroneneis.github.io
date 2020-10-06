---
title: 'Day 4 - To-string conversion'
date: 2018-07-26
permalink: /posts/2018/07/coding-challenge-day-4/
tags:
  - python
  - coding
  - object_orientation
  - to_string_conversion
---

**Topics:** to-string conversion, ```__repr__, __str__ ```     
**Updated:** 2020-10-04      

## To-string conversion
Today we are going to look at the two methods that control how an object is converted into a string object. When we just print an object, we won't get a useful representation. For example, when trying to print the `bromley` instance:

```python
bromley = CastleKilmereMember(name='Bromley Huckabee', birthyear=1959, sex='male')
print(bromley)
```

We get this output: `<__main__.CastleKilmereMember object at 0x7f81853bfc50>`. It contains the name of the class an the ID of the object (its memory address). This is not very useful when we want to know what the object is. But there is a simple solution to our problem!
   
We can control how objects of our classes are converted into string objects. Specifically, the to-string conversion is controlled by two methods: `__repr__` and `__str__` that serve different purposes:   

The result of the `__repr__` method should be as *unambiguous* as possible. It should function as a debugging aid for developers. Therefore, it should be explicit about what the object is. Furthermore, if possible, `__repr__` should return a representation of the instance that can be used to recreate it.
   
The result of the `__str__` method should be *readable*. Just think about how the object should look like for a user, not a developer.
   
By defining these special Python methods we can control how our objects will be represented when converting them into strings. It's recommended to implement at least the `__repr__` method for a class. Why `__repr__`? Because it will ensure a useful conversion of objects to strings. When Python looks for the `__str__` method but `__str__` hasn't been implemented, it will fall back to using `__repr__`. So as long as `__repr__` is defined we will get a helpful representation of our object.   
   
Let's add a `__repr__` method to our CastleKilmereMember class!

```python
class CastleKilmereMember:
    def __init__(self, name: str, birthyear: int, sex: str):
        ...

    def __repr__(self) -> str:
        return (f"{self.__class__.__name__}(name='{self.name}', "
                f"birthyear={self.birthyear}, sex='{self.sex}')")
```

Now, the string representation of Bromley is much more useful
```python
print(bromley)
>>> CastleKilmereMember(name='Bromley Huckabee', birthyear=1959, sex='male')
```
Further, we can use the output of our `__repr__` method to re-create the Bromley instance. This is done by first calling `__repr__` and evaluating the result:

```python
bromley == eval(repr(bromley))
>>> True
```

Since our subclasses inherit all methods from the parent class they will also contain an implementation for `__repr__`. But we want the output of `__repr__` to be as unambiguous as possible. Therefore, we should add separate `__repr__` methods to ensure that we use all information we have about the objects. For example, the `__repr__` implementation of the `Professor` class will look as follows:

```python
class Professor(CastleKilmereMember):
    """ Creates a Castle Kilmere professor """
    ...
    def __repr__(self) -> str:
        return (f"{self.__class__.__name__}(name='{self.name}', "
                f"birthyear={self.birthyear}, sex='{self.sex}', "
		f"subject='{self.subject}')")
```

