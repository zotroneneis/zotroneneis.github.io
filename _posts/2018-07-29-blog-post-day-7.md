---
title: 'Day 7 - Underscore patterns when naming variables'
date: 2018-07-29
permalink: /posts/2018/07/coding-challenge-day-7/
tags:
  - python
  - coding
  - object_orientation
---

**Topics:** underscore patterns for variable naming, ```_variable, __variable, __variable__, _```

Today we are going to look at a very important concept in Python: the usage of underscores when naming variables! In Python we have five different naming conventions that involve underscores. 
      
**A variable with a single leading underscore like** ```_harry```
- This is a naming convention followed by most Python code   
- A name prefixed with an underscore should be treated as a *non-public* part of the API and it might be changed without notice   
- This holds independent of whether it is a function, a method or a data member   

----
   
**A variable with (at least) two leading underscores and at most one trailing underscore like** ```__harry```   
- When naming a variable like this it will trigger Python's so called *name mangling*
- This means that the variable name is internally replaced with ```_classname__var```      
- ```classname``` is the name of the current class with any leading underscore(s) stripped   
- For example, for the HogwartsMember class a variable named ```__name``` would internally be represented as ```_HogwartsMember__name```   
- Although there are no 'private' variables in Python, there is a valid use-case for private variables in classes (namely to avoid naming conflicts with childclasses)   
- The name mangling is enforced by the Python interpreter   
   
---- 
   
**A variable with double leading and double trailing underscores like** ```__init__```   
- These are so called "magic" Python objects or attributes   
- They have a special meaning that is defined by Python    
- Therefore, we should never invent such variable names but only use the existing ones as documented   

----
   
**A single underscore ```_``` on its own**      
- This naming convention is sometimes used to denote a variable you don't care about or won't use further   
- For example you might use it for a running index as in ```list_comprehension = [_ for _ in range(5)]```   

----

**A variable with a single trailing underscore like** ```list_```    
- Another naming convention    
- Used to avoid naming conflicts with names that have a special meaning in Python, like ```list``` or ```all```   
   
   
