---
layout: post
title: Design patterns 
subtitle: Factory Method
photo: business-equipment-factory-industrial-plant-357440.jpg
photo-alt: Factory Method
tags: [DesignPatterns, Python]
category: [Systems]
---

The factory method pattern is useful for situations that requires the creation of many different types of objects, all derived from a common base type. The pattern defines a method for the creation of objects, which factory subclasses can then override to specify the derived type that will be created. This enables the factory method to dynamically create at runtime a new object from a simple descriptor, such as an enum or a string. 

There are several different factory patterns ranging from the simple factory pattern to the more complex abstract factory. In the following we focus on the **factory method pattern** which lies in the middle in terms of complexity.

## Pattern design
<div class="table-wrapper">
  <table>
    <tbody>
      <tr><th>Summary</th><td>A utility class that dynamically creates an instance of a class at runtime from a family of derived classes.</td></tr>
      <tr><th>Problem</th><td>We want to decide at runtime what object is to be created based on some configuration or application parameter. When we write the code, we do not know what class should be instantiated.</td></tr>
      <tr><th>Solution</th><td>Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.</td></tr>
    </tbody>
  </table>
</div>
    


There four components required for a factory method pattern, two for the factory referred to as a `creator` and two for the object being created referred to as the `product`:
- `BaseCreator`: Performs two things: 1. Implements a method to create a new instance of an object using a mapping function. 2. Executes methods of the base product to initialise the object. The implementation of the mapping function is not defined and left to the concrete factory implementation.
- `ConcreteCreator`: Defines the implementation of the mapping function for creation of objects. Many concrete creators can be created from the same base creator.
- `BaseProduct`: Defines the interface for the product which must include the methods used to instantiate the product in the base creator.
- `ConcreteProduct`: Completes the implementation of the product type.

![UML diagram](/assets/images/posts/2020-04-28-design-patterns-factory/Factory.jpg)

## Implementation

```python
import abc
from enum import Enum
import math


class Shape(Enum):
    circle = 'circle'
    square = 'square'


class ShapeFactoryBase(abc.ABC):
    """Shape factory base class"""
    def get_shape(self, shape):
        """Method for instantiating each shape is fixed in the base class

        These methods must match the interface of the shape base class.
        """
        shape = self._create_shape(shape)
        shape.set_sides()
        shape.set_area_function()
        return shape

    @abc.abstractmethod
    def _create_shape(self, shape):
        """Method to create shapes is delegated to the concrete factory"""
        raise NotImplementedError("Method needs to be implemented in concrete class.")


class ShapeFactory(ShapeFactoryBase):
    """Shape factory concrete class"""
    def _create_shape(self, shape):
        """Method to assign mapping for creating shapes, called by the base class.

        shape (enum): Type of shape to make
        """
        if shape.value == Shape.circle.value:
            return Circle()
        elif shape.value == Shape.square.value:
            return Square()
        else:
            raise ValueError(f'Item {shape.value} is not recognised.')

# ----------------------------------------------------------------------------------------------------------------------


class ShapeBase(object):
    """Shape base class"""
    def __init__(self):
        self.sides = 0
        self.area = None

    @abc.abstractmethod
    def set_sides(self):
        raise NotImplementedError("Method needs to be implemented in concrete class.")

    @abc.abstractmethod
    def set_area_function(self):
        raise NotImplementedError("Method needs to be implemented in concrete class.")


class Circle(ShapeBase):
    """Circle concrete class"""
    def __init__(self):
        super().__init__()

    def set_sides(self):
        self.sides = 4

    def set_area_function(self):
        self.area = lambda x: x*x


class Square(ShapeBase):
    """Square concrete class"""
    def __init__(self):
        super().__init__()

    def set_sides(self):
        self.sides = 1

    def set_area_function(self):
        self.area = lambda x: 0.25 * math.pi * x * x


if __name__ == '__main__':

    factory = ShapeFactory()
    circle = factory.get_shape(Shape.circle)
    square = factory.get_shape(Shape.square)

    print(f'{Shape.circle.value} sides = {circle.sides}')
    print(f'{Shape.square.value} sides = {square.sides}')
```


