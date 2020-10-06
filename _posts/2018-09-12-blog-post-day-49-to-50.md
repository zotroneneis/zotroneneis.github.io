---
title: 'Day 49 to 50 - Config files'
date: 2018-09-10
permalink: /posts/2018/08/coding-challenge-day-49-to-50/
tags:
  - python
  - coding
  - configs
---

**Topics:** Using config files
 
## What are config files
As a last topic I would like to take a look at the usage of config files. First of all we have to establish what a config file is. The [Wikipedia definition](https://en.wikipedia.org/wiki/Configuration_file) definition is precise and easy to understand: *configuration files (or config files) are files used to configure the parameters and initial settings for some computer programs*.

In our case we can use a config file to organize and speed up the process of creating members of our magical universe. 

## Advantages of config files

Using configuration files has many benefits. A few of them are:
- Config files allow us to separate our code from the parameters the code depends on.
- When a program uses many different parameters, config files make it easier to organize the parameters.
- Also, config files make it easier to cooperate with people who can't code. When the parameters are saved in a separate file, anyone can specify desired parameter values and run the code.
- Config files can be used independently of the programming language our program runs in.
- Config files can be used as a kind of documentation. For example, when a config file is used to store the parameters of a machine learning model, saving the config file together with the trained model documents the parameter values used.

## YAML and JSON

There are different languages that can be used for creating config files. Major ones are JSON and YAML. I prefer using yaml because yaml config files are very readable and easy to compose. If you are interested in the differences between the two formats you can check out [this link](https://www.json2yaml.com/).

To create a yaml file we first have to install PyYAML using `pip install pyyaml`. Afterwards, we can create a config file by using the ending `.yaml`, as in `config.yaml`. The syntax of the file looks as follows:

```yaml
bromley:
    name: 'Bromley Huckabee'
    birthyear: 1959
    sex: male

lissy:
    name: 'Lissy Spinster'
    birthyear: 2008
    sex: 'female'
    start_year: 2020
    pet: [Ramses, cat]
```

## Creating members of the Magical Universe using a config file

In order to use our `config.yaml` file to create new Magical Universe members, we have to open and load the file. After that, we can easily instantiate new members of the different classes:

```python
import yaml
from magical_universe import CastleKilmereMember, Pupil

if __name__ == "__main__":

    with open('config.yaml', 'r') as c:
        config = yaml.load(c)

        bromley = CastleKilmereMember(**config['bromley'])
        print(bromley)

        lissy = Pupil(**config['lissy'])
        print(lissy)
```

This speeds up the process of creating new members a lot! Also, we don't have to type the names, birthyears, etc. over and over again. Once they are specified in the config file we can reuse them as often as we like.

