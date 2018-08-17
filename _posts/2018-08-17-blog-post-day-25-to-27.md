---
title: 'Day 25 to 27 - Testing code with pytest'
date: 2018-08-17
permalink: /posts/2018/08/coding-challenge-day-25-to-27/
tags:
  - python
  - coding
  - testing
  - pytest
---

**Topics:** Testing code with pytest

Writing test functions for our code is extremely important. Since I have been lazy writing test code myself, I want to spend a little more time on this topic. My favorite testing framework is [pytest](https://pytest.org). I'm not an expert on testing, so please consider this post an introduction to testing rather than a thorough guide.

## Why pytest?

Using pytest has several advantages. First of all, pytest makes testing very easy because its syntax is simple and easy to understand. Furthermore, it offers pretty and useful failure information. Also, we often need less code with pytest than required to achieve equivalent functionality with frameworks like [unittest](https://docs.python.org/3/library/unittest.html).
   
You can install pytest using ```pip install pytest```.

## What makes a good test function?

I have a simple acronym that helps me remember how a good test function should look like. I found it in [this stackoverflow post](https://stackoverflow.com/questions/61400/what-makes-a-good-unit-test). A good test function should be *A (fast) TRIP*:    

**Automatic**: Test functions should be invoked automatically and we should automatically check the results for pass/fail.    

**Fast**: Good test functions run fast. If test code requires too much time, it will slow down production time and we won't run it as often as is desirable.    

**Thorough**: We should ensure that all key scenarios are tested.

**Repeatable**: A test function should produce the same result each and every time it's executed.        

**Independent**: A test should only test *one thing at a time*! Also, test functions should be independent of each other and no assumptions should be made about the order in which tests are executed.   

**Professional**: Test functions should contain clean, precise and readable code.    

## How testing functions should be named

Test functions should have long and descriptive names. Why? Because the test function are never called explicitely by the user. However, the function names will be displayed when a test fails. So if you want to know what functionality isn't working it's very useful to have a test function named, for example, ```test_square_of_negative_number()``` instead of just ```square()```.

## Basic usage of pytest

First of all, we will create a separate folder that holds our test functions. We will start with a file for testing the ```HogwartsMember``` class. In the next days and weeks, I will add more test code for the other classes and methods. In pytest file names should start or end with "test", so we will name our test file *test_hogwarts_member_class.py*. Before creating test functions for our Harry Potter universe, let's take a quick look at how a simple test function might look like.

Let's take our ```say_words()``` function from [day 5](http://alpopkes.com/posts/2018/07/coding-challenge-day-5/) as an example. The function looks as follows:

```python
def say_words(person, words):
    return f"{person} says: '{words}'"
```

A test function might look like this:

```python
def test_say_words():
    assert say_words("Ginny", "Careful Neville!") == "Ginny says: Careful Neville!"
```

We can run the test by running the command ```pytest``` from the command line within the *test_code* folder. The test should run without errors. 

## Testing the ```HogwartsMember``` class

Let's create a few tests that ensure that a Hogwarts member is created correctly and that we can add positive and negative traits, as well as check whether a member possesses a certain trait.

```python
def test_correctness_of_attributes_():
    hagrid = HogwartsMember('Rubeus Hagrid', 1928, 'male')
    assert hagrid.name == 'Rubeus Hagrid'
    assert hagrid.birthyear == 1928
    assert hagrid.sex == 'male'

def test_add_positive_traits():
    hagrid = HogwartsMember('Rubeus Hagrid', 1928, 'male')
    hagrid.add_trait('kind')
    hagrid.add_trait('wild')
    assert hagrid._traits == {'kind': True, 'wild': True}

def test_add_negative_trait():
    hagrid = HogwartsMember('Rubeus Hagrid', 1928, 'male')
    hagrid.add_trait('mean', False)
    assert hagrid._traits == {'mean': False}

def test_exhibit_traits():
    hagrid = HogwartsMember('Rubeus Hagrid', 1928, 'male')
    hagrid.add_trait('kind')
    hagrid.add_trait('wild')
    hagrid.add_trait('mean', False)

    assert hagrid.exhibits_trait('kind') == True
    assert hagrid.exhibits_trait('mean') == False
    assert hagrid.exhibits_trait('smart') == None
```


## Pytest fixtures

Since our Harry Potter universe contains mostly classes we have to create at least one member of the ```HogwartsMember``` class before we can test any of its functions. As an effect, we repeatedly used the same line of code to instantiate Hagrid. Is there a way to avoid this? Of course! There is a feature in pytest called *fixtures*. Fixtures can be used to encapsulate code that is repeatedly needed for (some) test functions.

We can create a fixture using the ```@pytest.fixture``` decorator and pass the fixture as an argument to the test functions that need it. So we could change our test code in the following way:

```python
import pytest
from harry_potter_universe.harry_potter_universe import HogwartsMember

@pytest.fixture
def hagrid():
  hagrid = HogwartsMember('Rubeus Hagrid', 1928, 'male')
  return hagrid

@pytest.fixture
def hagrid_with_traits():
    hagrid = HogwartsMember('Rubeus Hagrid', 1928, 'male')
    hagrid.add_trait('kind')
    hagrid.add_trait('wild')
    hagrid.add_trait('mean', False)
    return hagrid

def test_correctness_of_attributes_(hagrid):
    assert hagrid.name == 'Rubeus Hagrid'
    assert hagrid.birthyear == 1928
    assert hagrid.sex == 'male'

def test_add_positive_traits(hagrid):
    hagrid.add_trait('kind')
    hagrid.add_trait('wild')
    assert hagrid._traits == {'kind': True, 'wild': True}

def test_add_negative_trait(hagrid):
    hagrid.add_trait('mean', False)
    assert hagrid._traits == {'mean': False}

def test_exhibit_traits(hagrid_with_traits):
    assert hagrid_with_traits.exhibits_trait('kind') == True
    assert hagrid_with_traits.exhibits_trait('mean') == False
    assert hagrid_with_traits.exhibits_trait('smart') == None
```

Fixtures are much more powerful than this. So if you want to learn more about them consider reading the [pytest docs on fixtures](https://docs.pytest.org/en/latest/fixture.html).


## Testing output printed to the shell

Our ```print_traits()``` function prints the positive and negative traits. So we need a way to test whether the printed output is correct. This can be done using the ```capfd``` fixture as explained in this [stackoverflow post](https://stackoverflow.com/questions/20507601/writing-a-pytest-function-for-checking-the-output-on-console-stdout-in-python):

```python
def test_print_traits(capfd, hagrid_with_traits):
    hagrid_with_traits.print_traits()
    stdout, err = capfd.readouterr()
    stdout = stdout.strip()
    assert stdout == 'Rubeus Hagrid is kind, wild but not mean'

```

## Testing for exceptions

To show how we can test whether our code raises a certain exception, we can try to create a ```HogwartsMember``` without any attributes. This should raise a ```TypeError```:

```python
def test_init_raises_exception_with_missing_arguments():
    with pytest.raises(TypeError):
        hagrid = HogwartsMember()
```


## Further Reading
- Book [Python Testing with pytest](https://pragprog.com/book/bopytest/python-testing-with-pytest) by Brian Okken    
- [Testing Your Code — The Hitchhiker's Guide to Python ](https://docs.python-guide.org/writing/tests/)   
- [Pytest](https://pytest.org)


## Full test code for ```HogwartsMember``` class

This test code can also be found on the [GitHub repo](https://github.com/zotroneneis/harry_potter_universe/blob/master/test_code/test_hogwarts_member_class.py).

```python
import pytest
from harry_potter_universe.harry_potter_universe import HogwartsMember

@pytest.fixture
def hagrid():
    hagrid = HogwartsMember('Rubeus Hagrid', 1928, 'male')
    return hagrid

@pytest.fixture
def hagrid_with_traits():
    hagrid = HogwartsMember('Rubeus Hagrid', 1928, 'male')
    hagrid.add_trait('kind')
    hagrid.add_trait('wild')
    hagrid.add_trait('mean', False)
    return hagrid

def test_correctness_of_attributes_(hagrid):
    assert hagrid._name == 'Rubeus Hagrid'
    assert hagrid.birthyear == 1928
    assert hagrid.sex == 'male'

def test_add_positive_traits(hagrid):
    hagrid.add_trait('kind')
    hagrid.add_trait('wild')
    assert hagrid._traits == {'kind': True, 'wild': True}

def test_add_negative_trait(hagrid):
    hagrid.add_trait('mean', False)
    assert hagrid._traits == {'mean': False}

def test_exhibit_traits(hagrid_with_traits):
    assert hagrid_with_traits.exhibits_trait('kind') == True
    assert hagrid_with_traits.exhibits_trait('mean') == False
    assert hagrid_with_traits.exhibits_trait('smart') == None

def test_print_traits(capfd, hagrid_with_traits):
    hagrid_with_traits.print_traits()
    stdout, err = capfd.readouterr()
    stdout = stdout.strip()
    assert stdout == 'Rubeus Hagrid is kind, wild but not mean'

def test_init_raises_exception_with_missing_arguments():
    with pytest.raises(TypeError):
        hagrid = HogwartsMember()

def test_says(hagrid):
    assert hagrid.says("Hi Harry!") == "Rubeus Hagrid says: Hi Harry!"

def test_name_property(hagrid):
    assert hagrid.name == 'Rubeus Hagrid'

def test_age_property(hagrid):
    assert hagrid.age == 90 # this holds only if the current year is 2018!

def test_repr_output(capfd, hagrid):
    print(hagrid)
    stdout, err = capfd.readouterr()
    stdout = stdout.strip()
    assert stdout == 'HogwartsMember(Rubeus Hagrid, birthyear: 1928)'

```
