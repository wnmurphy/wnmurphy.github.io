---
title: Python Class Overview
date: 2021-10-02T15:32:50.887-08:00
layout: single
permalink: /python-class-overview/
category: 'programming'
---

I thought it would be fun to review classes in Python and produce a blog post as an artifact, maybe to help others, or to serve as a brush-up for future-me.

Why do we even care about classes? They allow you to define your own reusable data types. 

Classes also allow you to store common functionality in a central location, then split off more specialized versions of a thing that are wrapped with extra functionality.


## Overview

A class is a template for an object, which is created when you instantiate the class. Resulting objects are "instances" of that class.

Classes in Python contain methods and fields ("attributes"). These are referred to generally as class "members." 

Instances of a given class have access to the methods and attributes defined in the class definition.


## Basic class definition

A basic class definition might look like:

```python
class MyClass():

	class_variable_that_should_be_inherited_by_all_instances = 10

	def __init__(self, value_passed_during_instantiation):
		self.attribute_to_set_on_instantiation = value_passed_during_instantiation

	def get_value_of_attribute_for_this_instance(self):
		return self.attribute_to_set_on_instantiation
```

I'm using obnoxiously long variable names for clarity. I hope you don't mind.

Notice that the naming convention is capitalized `CamelCase`, rather than `snake_case` like everything else in Python.

This class has two methods, a conventional "dunder" ("double-underscore", internal) `__init__` method, and a getter method that we define. `__init__` is automatically called when the class is instantiated, and any kind of instance customization or setup you want to do should be written here. In this case, we've designed the class to accept a value and set it on the instance at the time of instantiation.

Notice that we pass `self` into these methods, so they know what to bind to when the class is instantiated; they should be bound to the instance, not the class definition. Honestly, this is a little wonky and would be better if implicit, but it's okay, we still love you Python.

Let's use `dir` to introspect and see what's in the class we just defined (we could also call `MyClass.__dict__` to get more information like member addresses and types):

```python
>>> dir(MyClass)
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'class_variable_that_should_be_inherited_by_all_instances', 'get_value_of_attribute_for_this_instance']
```

You can see that this class has a bunch of out-of-the-box dunder methods for internal use, but that it also has the class variable and getter method we added.

Notice that it has no `attribute_to_set_on_instantiation`, because we hain't instantiated it yet.


## Class instantiation

To use the class, we need to instantiate it. To instantiate an object of this class, we just call the class and assign the result to a variable so we have a handle on it and can refer to it usefully:

```python
my_instance = MyClass("init_value")
```

Then we can use the instance by accessing it's methods and attributes.

```python
my_instance.get_value_of_attribute_for_this_instance() # "init_value"
my_instance.class_variable_that_should_be_inherited_by_all_instances # 10
```

That's the gist of classes in Python.


## Inheritance

Inheritance is an object-oriented programming (OOP) principle, referring to a hierarchy of classes, where subclasses/children inherit access to any members contained in their superclasses/parents.

For example, you can have a general superclass like `Musician`, and have more specific subclasses like `Violinist` and `HarmonicaPlayer` ...this may be a dumb example, but let's roll with it.

```python
class Musician():

	def make_sound(self, sound_to_make):
		print(sound_to_make)
```

The idea is that subclasses typically entail the behavior of their superclass, but also add a layer of additional specialization. Methods and attributes common to all musicians should be contained in `Musician`, and those are inherited by all subclasses. Then, any violin-specific methods or attributes (i.e. `apply_rosin_to_bow`) can be defined in `Violinist`.

When you define `Violinist`, you pass the class to inherit from as an argument, like:

```python
class Violinist(Musician):
	
	def apply_rosin_to_bow(self, how_much_rosin_to_apply):
		self.rosin += how_much_rosin_to_apply
```

Now, any violin player we make has access to whatever methods/attributes we defined in `Musician`:

```python
timmy = Violinist()
timmy.make_sound("*sad emotional sounds*") # "*sad emotional sounds*"
```


## Fancy decorators

There are a few other ways we can set up members in a class using decorators.


### Static methods

A static method is one that can be accessed in a class without instantiation. These are typically useful for when you want to define a class and use it as a toolkit; it's more like a namespace or collection/container.

Let's pretend all musicians are paid the same, and before we go ahead and hire (instantiate) one for our wedding, we'd like to know how much it's going to cost.

```python
class Musician():

	@staticmethod	
	def calculate_pay_rate(hours):
		return hours * "$"

	def make_sound(self, sound_to_make):
		print(sound_to_make)
```

Notice we don't need to pass `self` in to a static method; these methods have no referent that needs to be bound. They're "static" because they don't change their behavior when the class is instantiated.

```python
hours = 3
Musician.calculate_pay_rate(hours) # $$$
```

Okay, cool. So hiring a musician for three hours will cost us three dollar signs. Notice that we don't need to instantiate a `Musician` to use the static method.


### Class methods

A class method is one that is bound to the class definition, not the instance of that class. Recall that we passed `self` in to each of the methods so they refer to the instance object after instantiation.

Sometimes you may want to instantiate a class, but then have the instance retain a binding to the class definition itself. In that case, you can use the `@classmethod` decorator and pass in `cls` instead (this could be any name, but `cls` is conventional).

One use of this could be to define factory methods/alternate constructors to churn out different types of `Musician`s without having to define subclasses for them:

```python
class Musician():

	def __init__(self, instrument):
		self.instrument = instrument

	@staticmethod	
	def calculate_pay_rate(hours):
		return hours * "$"

	def make_sound(self, sound_to_make):
		print(sound_to_make)

	@classmethod
	def make_new_violinist(cls):
		return cls("violin")

	@classmethod
	def make_new_harmonica_player(cls):
		return cls("harmonica")
```

```python
guy_from_blues_traveler = Musician.make_new_harmonica_player()
```


## Abstract classes

An abstract class contains at least one abstract method, which is just an empty placeholder that serves as contract that has to be fulfilled by anything that implements the abstract class.

Defining an abstract class is like saying, "we don't care how you do it, but if you want to build something that counts as a `$THING`, your `$THING` needs to have a way to do `$METHOD`."

Python doesn't have native abstract classing, but from Python 3.4+ you can use the `ABC` module.

```python
from abc import ABC, abstractmethod

class Animal(ABC):
	
	@abstractmethod
	def eat(self):
		pass

	@abstractmethod
	def sleep(self):
		pass

	@abstractmethod
	def poop(self):
		pass

	@abstractmethod
	def move(self):
		pass
```

This example defines a contract which says "If you want to create your own Animal, we don't care how it does these things, but it must have ways to eat, sleep, poop, and move."

Abstract classes can't be instantiated, only subclassed. Python will throw an exception unless each of these abstract methods are overridden (defined) on your subclass.
