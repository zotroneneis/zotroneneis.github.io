---
title: Extending the Magical Universe
date: 2018-08-29
menu:
  sidebar:
    name: Extending the universe II 
    identifier: extending_2
    parent: magical_universe
    weight: 21
---

**Topics:** Extending the Magical Universe with classmethods for `Charm, Hex, Curse`, etc.
 
Today I added classmethods to the different `Spell` subclasses. Speficially, I added one classmethod for each spell type: `Charm, Transfiguration, Jinx, Hex, Curse, CounterSpell, HealingSpell` such that at least one charm, hex, jinx, ... exists in our universe. Tomorrow I will write test code for the entire abstract base class `Spell`.
