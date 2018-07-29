---
title: 'Day 5 - Decorators'
date: 2018-07-27
permalink: /posts/2018/07/coding-challenge-day-5/
tags:
  - python
  - coding
  - object_orientation
  - decorators
---

**Topics:** decorators

Today, we are going to look at decorators. Python's decorators are an advanced concept so don't worry if you don't immediately understand how they work. The more you will use and read about them, the clearer the concept will become.

In simple terms a decorator is a callable (i.e. a function, method of class) that takes a callable as an input and returns a callable.

Decorators allow us to extend and/or modify the behavior of the callable that they take as an input. And they do that without permanently modifying the callable itself! The callable's behavior is changed *only* when it is decorated.

Their are called decorators because they "decorate" other functions and allow us to run code before and after the wrapped function is executed.

