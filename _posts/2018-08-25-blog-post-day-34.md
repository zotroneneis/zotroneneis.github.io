---
title: 'Day 34 - Multisets, ```collections.Counter```'
date: 2018-08-25
permalink: /posts/2018/08/coding-challenge-day-34/
tags:
  - python
  - coding
  - multisets
---

**Topics:** Multisets, ```collections.Counter```
 
## Counting objects

Today I want to present a very useful class in Python: ```collections.Counter```. You might wonder what is so great about this class: it allows us to count all kinds of objects! And who doesn't love counting? For example, consider all the potion ingredients Cleon, Flynn and Cassidy need to buy for a school year. We can easily create a shopping list for them using ```collections.Counter```.

## What is ```collections.Counter``` ?

The ```Counter``` class is a dictionary subclass. To be more precise, a Counter is an unordered collection of elements, where the elements are stored as dictionary keys and their counts as dictionary values.   
    
Because a Counter is a kind of dictionary, the stored elements must be *hashable*. In Python, all immutable objects (like strings) are hashable, while mutable objects (like lists) are not hashable (remember: immutable means that once we created an object, we can't change it anymore).

The [definition of hashable](https://docs.python.org/3.7/glossary.html) states that "an object is hashable if it has a hash value which never changes during its lifetime (it needs a __hash__() method), and can be compared to other objects (it needs an __eq__() method)".
   
## Creating a shopping list

Let's see how we can create a shopping list and count all potion ingredients we need to buy.

```python
from collections import Counter
from magical_universe import Potion

# Step 1: Create the potions

flask_of_remembrance = Potion(['raven eggshells', 'tincture of thyme', 'unicorn tears',
                                'dried onions', 'powdered ginger root'])

vial_of_anger = Potion(['dried dragon skin', 'leeches', 'shredded elephant tusk',
                        'horned flies', 'earthworm juice', 'dried onions'])

ancient_wisdom = Potion(['tincture of thyme', 'leeches', 'drakus flower', 'lavender sprig',
                          'earthworm juice', 'cactus juice', 'dried onions'])

brew_of_lies = Potion(['horned flies', 'leeches', 'drakus flower', 'horned flies',
                        'unicorn tears', 'cactus juice'])

# Step 2: Create list of all potions
all_potions = [flask_of_remembrance, vial_of_anger, ancient_wisdom, brew_of_lies]

# Step 3: Create the shopping list!
shopping_list = Counter()

for potion in all_potions:
    for ingredient in potion:
        shopping_list[ingredient] += 1

print(f"Final shopping list: {shopping_list}")
```

## Further reading

The ```collections.Counter``` class has a few useful methods, for example, ```Counter.most_common()```. To learn more about the Counter class, look at the [Python docs](https://docs.python.org/3/library/collections.html#collections.Counter).
