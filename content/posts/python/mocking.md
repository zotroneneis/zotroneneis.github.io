---
title: Mocking in Python
date: 2020-10-11
menu:
  sidebar:
    name: Mocking
    identifier: mocking
    parent: python
    weight: 1
---

**Topics:** Mocking in Python

Today, I want to talk about mocking. I became interested in this topic a few months back when I started to work in a data engineering project at work. In this project we have a lot of tests that relies on mocking to test code with external dependencies. If you don't like reading long blog posts, consider listening to one of the podcast episodes I did on this topic: there is one at [Talk Python to Me]() and the other at [Test and Code]().

## Introduction
Before getting started you should note that mocking is a controversial topic. Some people think it is great while others regard it only as a last resort that should be be avoided. Personally, I believe that mocking can be a great tool to improve testing but should be used with care. Most importantly, it should not be used as a tool to fix badly written code. However, the Zen of Python tells us that practicality beats purity. So rather use mocking than no testing at all.

## What is mocking?
A mock simulates the existence and behavior of a real object. This allows developers to   
- Improve the quality of their tests     
- Test code in a controlled environment    
- Test code which has external dependencies    
- ...    

The python library for mocking is `unittest.mock`. If you prefer using `pytest`, don't worry - `pytest` supports the `unittest.mock` library. You can also take a look at pytest libraries for mocking like [pytest-mock](https://pypi.org/project/pytest-mock/).

## Example class
In the spirit of our magical universe [The Tales of Castle Kilmere](http://alpopkes.com/posts/2018/07/the_tales_of_castle_kilmere/) we will work with an example class `Spell` in this blog post. Our `Spell` class is simple: each spell has a name, an incantation and a defining feature:

```python
class Spell:
   """Creates a spell"""
   def __init__(self, name: str, incantation: str, defining_feature: str):
      self.name = name
      self.incantation = incantation
      self.defining_feature = defining_feature
```

## Naming conventions and Test Doubles
Similar to the question of whether mocking is good or bad there is disagreement about the terminology of mocking. Some people prefer a more fine-grained terminology in which different types of 'mocking behavior' are distinguished. Others believe that this makes things unnecessarily complicated. For completeness, let’s take a look at the more fine-grained view. In this view, objects which are not real can be either dummies, fakes, stubs, mocks or spies. All of these are so called *test-doubles*. Note that the definitions of the different kinds of test doubles are not set in stone but are controversial and vary between sources.

### Dummies
A dummy is an object which is passed around but not intended to be used in your tests. It will have no effect on the behaviour of your tests. An example for a dummy could be an attribute that is needed in order to instantiate a class but not required for the test. Looking at our `Spell` class we might want to test casting a spell but won't require the `defining_feature` attribute for this:

```python
class Spell:
   """Creates a spell"""
   def __init__(self, name: str, incantation: str, defining_feature: str):
      self.name = name
      self.incantation = incantation
      self.defining_feature = defining_feature

   def cast(self):
      print(f"{self.incantation}!")

def test_cast_spell():
   name = "The Stuporus Ratiato spell"
   incantation = "Stuporus Ratiato"
   defining_feature = "" # this is a dummy

   assert spell.cast == "Stuporus Ratiatio!"
```

### Fakes
A fake implements a fake version of a class or method. It has a working implementation but takes some kind of shortcut such that it is not suitable for production. For example, we could implement an in memory database which is used for testing but not in production. In this setting, the in memory database would function as a fake.

### Stubs
A stub is a non-real object which has pre-programmed behavior. Most of the time, stubs simply return fixed values (also refered to as canned data). For example, let’s say the `Spell` class has a method `get_similar_spells` which searches a database for similar spells. The logic behind this function is very complex and the function takes several minutes to complete. During testing, we don't want to wait several minutes for a result. Therefore, we could replace the real implementation with a stub that returns hard-coded values; taking only a small fraction of the time.

### Mocks
Mocks are closely related to stubs. A mock does not have predetermined behavior. Instead, it has to be configured with our expectations. The important difference between mocks and stubs is that a mock records which calls have been executed. Therefore, it can be used to verify not only the result (which can be done with a stub, too) but HOW the result was achieved / that the correct methods have been invoked. Mocks are the type of test double we will be talking about in this blog post.

If you are confused about the difference between stubs and mocks, that a look at [this stackoverflow post](https://stackoverflow.com/questions/3459287/whats-the-difference-between-a-mock-stub). It might be helpful.

### Spies
Spies are used to wrap real objects and, by default, route all method calls to the original object. In this sense, they intercept and record all calls to a real object. This allows us to verify calls to the original object (e.g. how often a certain method has been called) without replacing the original object (as, for example, a mock does). I haven’t use spies myself yet and find them the hardest to understand.

## When should we mock?
Mocking can be used whenever we don't want to actually call an object. Let's say our `Spell` class has a function `save` which writes file to disk and function `remove` which deletes local file:

```python
class Spell:
   """Creates a spell"""
   def __init__(self, name: str, incantation: str, defining_feature: str):
      self.name = name
      self.incantation = incantation
      self.defining_feature = defining_feature

   def save(self, save_path: str) -> dict:
      spell_as_dict = {"name": self.name,
	               "incantation": self.incantation,
	               "description": self.description}
      with open(save_path, "w") as file:
         json.dump(spell_as_dict, file, indent=4)

      return spell_as_dict

   def remove(self, file_path: str):
      os.remove(file_path)
```


When testing these methods we don’t want to write to disk every time the test runs
Same holds for function ‘remove’

## How to mock
The `unittest.mock` library has three core functionalities:
1. The `Mock` class
2. The `MagicMock` class
4. The `patch` method

### The `unittest.mock.Mock` class
The `Mock` class can be used for mocking any object. A mock simulates the object it replaces. To achieve this, it creates attributes on the fly. In other words: you can access whatever methods and attributes you like, the mock object will simply create them. This is quite magical, don't you think?

```python
from unittest.mock import Mock
my_mock = Mock()

# Let's try to access some attribute
my_mock.fancy_attribute
>>> <Mock name='mock.fancy_attribute' id='140586483377056'>

# What about a method with inputs?
my_mock.fancy_method(3, 4)
>>> <Mock name='mock.fancy_method()' id='140586482172880'>

# We can assert that certain methods can be called
my_mock.fancy_method.assert_called_with(3, 4)
>>> True
```

When taking a close look at the example above we can see that each call returns a mock object with a different ID. This is an important feature of mocks: they will automatically create child mocks when you access a property or method. You should keep in mind that the methods and attributes you create have nothing to do with the 'true' implementation of a method. For example, we could mock the `json` library and access the method `dumps` on the mock object. We could call `dumps` with all kinds of attributes (or non at all) without the mock complaining about it:

```python
json = Mock()
json.dumps()
>>> <Mock name='mock.dumps()' id='140586482201840'> 
json.dumps.assert_called_once()
>>> True
```

We can configue a mock object to do exactly what we want it to do. For instance, we can set the
return value of a mock, or side effects it should exhibit:

```python
my_mock = Mock()
my_mock.return_value = 3
my_mock()
>>> 3

my_mock = Mock(side_effect=Exception("Python rocks!"))
my_mock()
---------------------------------------------------------------------------
Exception                                 Traceback (most recent call last)
<ipython-input-14-faa07e9a9283> in <module>
----> 1 my_mock()

~/anaconda3/lib/python3.8/unittest/mock.py in __call__(self, *args, **kwargs)
   1079         self._mock_check_sig(*args, **kwargs)
   1080         self._increment_mock_call(*args, **kwargs)
-> 1081         return self._mock_call(*args, **kwargs)
   1082 
   1083 

~/anaconda3/lib/python3.8/unittest/mock.py in _mock_call(self, *args, **kwargs)
   1083 
   1084     def _mock_call(self, /, *args, **kwargs):
-> 1085         return self._execute_mock_call(*args, **kwargs)
   1086 
   1087     def _increment_mock_call(self, /, *args, **kwargs):

~/anaconda3/lib/python3.8/unittest/mock.py in _execute_mock_call(self, *args, **kwargs)
   1138         if effect is not None:
   1139             if _is_exception(effect):
-> 1140                 raise effect
   1141             elif not _callable(effect):
   1142                 result = next(effect)

Exception: Python rocks!
```

### `Mock` vs. `MagicMock`
I mentioned that the `unittest.mock` library contains two classes: `Mock` and `MagicMock`. So what is the difference between them? `MagicMock` is a subclass of `Mock`. It contains all magic methods pre-created and ready to use (e.g. `__str__`, `__len__`, etc.). Therefore, you should use `MagicMock` when you need magic methods, and `Mock` if you don't need them.

### The `unittest.mock.patch` method
The third main functionality contained in the `unittest.mock` library is the `patch` function. So what is patching? The `patch()` function looks up an object in a given module and replaces it with another object. By default, the object is replaced with a new `MagicMock` (but this can be changed). When you want to use `patch` you have to use the syntax `patch(‘package.module.target’)`. For example, consider we have a file `example.py` which contains the following code: 

```python
from db import db_write
def foo():
   x = db_write()
   return x
```

We want to mock the call to the `db_write` function in our tests. With patching, this can be dones as follows:
```python
from unittest.mock import patch
from example import foo

@patch('example.db_write')
def test_foo(mock_write):
   mock_write.return_value = 10
   x = foo()
   assert x == 10
```
What is happening when using patching like this? In simple terms, the call to `db_write` is replaced with a call to `MagicMock`:
```python
def foo():
   x = MagicMock()
   return x
```
By replacing the call to `db_write` with a call to `MagicMock`, everything happening in `db_write` is not being executed. The only thing that is called is the `MagicMock` object!

In the example above we used `patch()` as a decorator. We could also use it as a context manager or with manual starting/stopping. When to use which version depends on the *scope* you want/need. Consider the decorator example:

```python
from unittest.mock import patch
from example import foo

@patch('example.db_write')
def test_foo_decorator(mock_write):
   mock_write.return_value = 10
   x = foo()
   assert x == 10
```
In this example, the `db_write` function will be replaced by a `MagicMock` for the entire test
function. In other words: in the test function `test_foo_decorator` you don't have access to the true implementation of `db_write` anymore, only to the mocked version. A more fine-grained scheme can be achieved using a context manager:
```python
def test_foo_context_manager(mock_write):
   with patch('example.db_write`, return_value = 10):
      x1 = foo()
      assert x1 == 10

   x2 = foo() # When calling foo here, db_write won't be mocked
```

Manual starting and stopping can be useful when you need to mock certain parts of your code for all tests in your test file. I haven't yet used this functionality myself but you can find a good description [here](https://docs.python.org/3/library/unittest.mock.html).

Note: there is also `patch.object` and `patch.dict`. We won’t discuss them here to avoid confusion, please check out the [documentation](https://docs.python.org/3/library/unittest.mock.html) to find out how they are used.

## Where do we patch?
When using patching it is important to know *where* `patch` is applied. Specifically, we should patch where the object is *looked up*. This might not be the place where the object is defined! In the example above (the one where our file is named `example.py`) we import the method `db_write` from the module `db`:
```python
from db import db_write
def foo():
   x = db_write()
   return x
```
Consequently, the `example` module knows about `db_write`. However, it does **not** know about `db`. This is nicely illustrated when importing the `example` module (e.g. in an IPython shell) and calling `dir(example)` to see a list of attributes for the `example` object.
```python
import example
dir(example)
>>>
['__builtins__',
 '__cached__',
 '__doc__',
 '__file__',
 '__loader__',
 '__name__',
 '__package__',
 '__spec__',
 'db_write',
 'foo']
```
As we can see here, `db_write` is in the namespace of `example`, but `db` is **not** in the
namespace. Therefore, to mock the call to `db_write` in the `foo` function, we have to patch
`example.db_write`:
```python
from unittest.mock import patch
from example import foo

@patch('example.db_write')
def test_foo_decorator(mock_write):
   mock_write.return_value = 10
   x = foo()
   assert x == 10
```
Think about this for a moment - `db_write` is *defined* in the module `db` but *used* in the module `example`.

Let's drive this point home with another example. Let's say that we import the entire `db` module in `example.py`:
```python
import db
def foo():
   x = db.db_write()
   return x
```
In this case, the `example` module knows about `db` but **not** `db_write`:
```python
import example
dir(example)
>>>
['__builtins__',
 '__cached__',
 '__doc__',
 '__file__',
 '__loader__',
 '__name__',
 '__package__',
 '__spec__',
 'db',
 'foo']
```
To mock the call to `db_write` in the `foo` function, we need to mock `db.db_write` or, if you find it easier to understand `example.db.db_write`:
```python
from unittest.mock import patch
from example import foo

@patch('example.db.db_write')
def test_foo_decorator(mock_write):
   mock_write.return_value = 10
   x = foo()
   assert x == 10
```

## Common mocking problems and how to solve them
In the beginning of this post we saw that we can configure `Mock` objects. For example, we can set their return value or side effects like exceptions. The fact that `Mock` objects create attributes and methods on the fly makes them sensitive to mistakes. For example, if you call `.asert_called_once()` instead of `.assert_called_once()` on a Mock, the test will not raise an `AssertionError` because you have created a new method on the Mock object called `asert_called_once()` instead of calling the actual function.

Another easy mistake occurs when forgetting that a Mock object does not know the interface of your class / method. In other words: the Mock object does not know which attributes it can be called with. We talked about that earlier with the `json.dumps()` example. We can illustrate this problem when taking a closer look at the attributes of the thing we are mocking. Let's say we we mock our `Spell` class. When calling `dir()` on the mocked object we see all kinds of attributes:
```python
import spell_class
from unittest.mock import patch

with patch('spell_class.Spell') as mocked_spell:
   print(dir(mocked_spell))
>>>
['assert_any_call', 'assert_called', 'assert_called_once', 'assert_called_once_with', 'assert_called_with', 'assert_has_calls', 'assert_not_called', 'attach_mock', 'call_args', 'call_args_list', 'call_count', 'called', 'configure_mock', 'method_calls', 'mock_add_spec', 'mock_calls', 'reset_mock', 'return_value', 'side_effect']
```
For example, we can see the different assertions that can be made on the Mock object. Hover, we don’t see the actual methods of the `Spell` class, e.g. the `save` or `remove` method! This is because the call to `patch` returns a `MagicMock` which is completely unrelated to the `Spell` class.
     
Problems like these can be solved using the `spec=True` attribute in the `patch` call. This will cause the `MagicMock` to look like the object we are patching. We could no longer call methods without argument or with the wrong arguments. Also, the misspelled `.asert_called_once()` would raise an Exception:
```python
import spell_class
from unittest.mock import patch

with patch('spell_class.Spell', spec=True) as mocked_spell:
   print(dir(mocked_spell))
>>> ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'assert_any_call', 'assert_called', 'assert_called_once', 'assert_called_once_with', 'assert_called_with', 'assert_has_calls', 'assert_not_called', 'attach_mock', 'call_args', 'call_args_list', 'call_count', 'called', 'configure_mock', 'method_calls', 'mock_add_spec', 'mock_calls', 'remove', 'reset_mock', 'return_value', 'save', 'side_effect']

with patch('spell_class.Spell', spec=True) as mocked_spell:
   mocked_spell.asert_called_once()
>>>
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-3-d3058c55a4ad> in <module>
      3 
      4 with patch('spell_class.Spell', spec=True) as mocked_spell:
----> 5    mocked_spell.asert_called_once()
      6 

~/anaconda3/lib/python3.8/unittest/mock.py in __getattr__(self, name)
    635         elif self._mock_methods is not None:
    636             if name not in self._mock_methods or name in _all_magics:
--> 637                 raise AttributeError("Mock object has no attribute %r" % name)
    638         elif _is_magic(name):
    639             raise AttributeError(name)

AttributeError: Mock object has no attribute 'asert_called_once'
```

Similar to `spec` are `autospec` and `specset`. Take a look at the [documentation](https://docs.python.org/3/library/unittest.mock.html) or at the PyCon talk [“Demystifying the patch function”](https://www.youtube.com/watch?v=ww1UsGZV8fQ) for more details.

One caveat when using `spec` (or `autospec` or `specset`) is that you can *not* access attributes created in the `__init__` method, that is, attributes that exist on the instances and not the class. Due to the internal workings of `spec` (and also `autospec` and `specset`) they cannot know about any dynamically created attributes. They only know about visible attributes. For our `Spell` example above, you won't see the attributes `name`, `incantation` and `description` in the output of `dir(mocked_spell)`. They are not valid attributes of the mock object anymore. Also, speccing comes at a cost and will slow down your tests

## Code design that prevents mocking
When you stumble into *mock hell* (a term introduced and explained in the talk ["Mocking and Patching Pitfalls"](https://www.youtube.com/watch?v=Ldlz4V-UCFw)) during development, it might be the right time to consider one of the design principles that prevent the need for mocking. For example, you could use dependency injection or an adaptor pattern. These principles are explained in the PyCon talk ["Stop using Mocks - For a While"](https://www.youtube.com/watch?v=rk-f3B-eMkI).

## Wrap-Up
Mocking is a controversial topic. It should be used with care and never to fix badly written code. The Python library for mocking - `unittest.mock` - comes with three core functionalities: `Mock, MagicMock` and `patch`. Mocking might seem confusing in the beginning but once you understand the basics (for example where to patch) it can be very helpful, especially with production code. There are lots of functionalities in the `unittest.mock` library that help prevent common problems, so it is useful to take a look at the documentation when you want to start using mocking. Lastly, don’t get confused by the different naming conventions! If you feel more comfortable with the fine-grained view on test doubles great! But sticking to a simpler scheme is also fine.

