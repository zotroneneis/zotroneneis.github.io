---
title: 'Day 21 - The mysterious ```if __name__ == "__main__"``` '
date: 2018-08-12
permalink: /posts/2018/08/coding-challenge-day-21/
tags:
  - python
  - coding
---

**Topics:** What is ```if __name__ == "__main__"``` doing?

## ```if __name__ == "__main__"```

I always wanted to dig into the statement ```if __name__ == "__main__"``` that is used in so many programs. I have used it for a long time already but until recently I had no idea what exactly it is doing. To make it as understandable as possible, I will divide the explanations into three steps.

### Step 1: Two ways of running code

We have saved our Harry Potter Universe in a file named ```harry_potter_universe.py```. First of all, we have to distinguish the two ways in which we can run our code:   
1. We can run the file directly, as in ```python harry_potter_universe.py```   
2. We can import the file from another module. In this case we would have a different script, for example 'simulate_quidditch_game.py'. In that script, we can use our Harry Potter classes by importing them at the beginning of the script. For example using: ```from harry_potter_universe import HogwartsMember, Pupil, Professor```.   
    
### Step 2: Setting up special variables

In both cases, the Python interpreter will read the source file ```harry_potter_universe.py```. But, as outlined in [this post](https://stackoverflow.com/questions/419163/what-does-if-name-main-do), the interpreter will first define a few special variables. One of the is the ```__name__``` variable. In case 1, so when running our file as the main program, the value of ```__name__``` is set to ```"__main__"```. In case 2, the value of ```__name__``` is set to the name of the module, which is ```"harry_potter_universe"```. 

### Step 3: Executing the code

Case 1: We run the file directly   

The interpreter has finished setting up the special variables, so the value of ```__name__``` is either ```"__main__"``` (case 1) or '''"harry_potter_universe" ''' (case 2). In a next step, the interpreter will read the file and execute all *top-level code* in our script. 'Top-level' code refers to all code at indentation level 0. So in our case, the import statements at the beginning of the ```harry_potter_universe.py``` file will be executed and all classes will be defined. However, none of the code inside the classes will be executed. At the end of the file we have an ```if``` block as top-level code. The ```if``` block starts with the line ```if __name__ == "__main__"```. With the knowledge from the previous steps we should be able to understand what this statement represents: it tests whether the current module, that is 'harry_potter_universe', is being run directly (case 1) or imported by another module (case 2)! In case 1, the output of ```__name__ == "__main__"``` will be ```True```, so the code within the ```if``` block will be exectuted. In case 2, the ```if``` clause won't be executed because the required condition is not met.


### Why "main"?

In case 1, the value of ```__name__``` is set to ```"__main__"``` because "harry_potter_universe.py" is run as the *main program*.



## Further reading:
[Python Docs](https://docs.python.org/3/library/__main__.html)
[Stackoverflow post](https://stackoverflow.com/questions/419163/what-does-if-name-main-do)

