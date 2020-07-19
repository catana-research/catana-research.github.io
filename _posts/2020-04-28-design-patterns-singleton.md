---
layout: post
title: Design patterns
subtitle: Singleton
photo: background-beautiful-blossom-calm-waters-268533.jpg
photo-alt: Singleton
tags: [DesignPatterns, Python]
category: [Systems]
---

Singleton pattern ensures a class can have only one instance, attempts to create a new instance returns the existing instance if it exists. 


Useful for stateful objects.

Need to be careful when using singletons within multithreaded applications.


Summary: *A utility class that dynamically creates an instance of a class at runtime from a family of derived classes.*

Problem: We want to decide at runtime what object is to be created based on some configuration or application parameter. When we write the code, we do not know what class should be instantiated.

Solution: Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

## Pattern design

There four components required for a factory method pattern, two for the factory referred to as a `creator` and two for the object being created referred to as the `product`:
- `BaseCreator`: Performs two things: 1. Implements a method to create a new instance of an object using a mapping function. 2. Executes methods of the base product to initialise the object. The implementation of the mapping function is not defined and left to the concrete factory implementation.
- `ConcreteCreator`: Defines the implementation of the mapping function for creation of objects. Many concrete creators can be created from the same base creator.
- `BaseProduct`: Defines the interface for the product which must include the methods used to instantiate the product in the base creator.
- `ConcreteProduct`: Completes the implementation of the product type.


## Implementation

```python
import abc
from enum import Enum
import math


```


