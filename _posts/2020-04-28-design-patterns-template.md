---
layout: post
title: Design patterns
subtitle: Template method
photo: pattern-metal-pipes-grid-35543.jpg
photo-alt: Template
tags: [DesignPatterns, Python]
category: [Systems]
---

The *Template method* is a skeleton framework that defines a set of methods to be executed. The definition of these methods however is often not provided and left to subclasses to implement. This ensures that the underlying orchestration logic is fixed in the base class, decoupled from the subclasses. The subclasses are therefore only responsible for the implementation of these methods and agnostic to their execution. Although strucute is a fixed, optional *hook* methods can also be provided that may be overriden by the concrete implementation to provide added flexibility to the algorithm.

Templates often employ the 'Hollywood Principle' (*“don’t call us, we’ll call you”*), leaving the template to orchestrate the execution of methods so that the low-level components only need to provide an implementation of the methods. This provides code that is coded to the abstraction, decoupling the code.


## Pattern design

Summary: *Skeleton framework of abstract methods that are left to the concrete implementation to define. Hooks are provided that may be overriden by the concrete implementation to provide added flexibility to the algorithm.*

Elements: 
- `BaseClass`
- `ConcreteClass`

![UML diagram](/assets/images/posts/2020-04-28-design-patterns-template/Template.jpg)


## Implementation

Base class:
```python
# https://refactoring.guru/design-patterns/template-method/python/example#lang-features
from abc import ABC, abstractmethod


class AbstractClass(ABC):
    """
    The Abstract Class defines a template method that contains a skeleton of
    some algorithm, composed of calls to (usually) abstract primitive
    operations.

    Concrete subclasses should implement these operations, but leave the
    template method itself intact.
    """

    def template_method(self) -> None:
        """
        The template method defines the skeleton of an algorithm.
        """

        self.base_operation1()
        self.required_operations1()
        self.base_operation2()
        self.hook1()
        self.required_operations2()
        self.base_operation3()
        self.hook2()

    # These operations already have implementations.
    def base_operation1(self):
        print("AbstractClass says: I am doing the bulk of the work")

    def base_operation2(self):
        print("AbstractClass says: But I let subclasses override some operations")

    def base_operation3(self):
        print("AbstractClass says: But I am doing the bulk of the work anyway")

    # These operations have to be implemented in subclasses.

    @abstractmethod
    def required_operations1(self):
        pass

    @abstractmethod
    def required_operations2(self):
        pass

    # These are "hooks." Subclasses may override them, but it's not mandatory
    # since the hooks already have default (but empty) implementation. Hooks
    # provide additional extension points in some crucial places of the
    # algorithm.
    def hook1(self) -> None:
        pass

    def hook2(self) -> None:
        pass
```

Concrete implemenations:
```python
class ConcreteClass1(AbstractClass):
    """
    Concrete classes have to implement all abstract operations of the base
    class. They can also override some operations with a default implementation.
    """

    def required_operations1(self) -> None:
        print("ConcreteClass1 says: Implemented Operation1")

    def required_operations2(self) -> None:
        print("ConcreteClass1 says: Implemented Operation2")


class ConcreteClass2(AbstractClass):
    """
    Usually, concrete classes override only a fraction of base class'
    operations.
    """

    def required_operations1(self) -> None:
        print("ConcreteClass2 says: Implemented Operation1")

    def required_operations2(self) -> None:
        print("ConcreteClass2 says: Implemented Operation2")

    def hook1(self) -> None:
        print("ConcreteClass2 says: Overridden Hook1")


def client_code(abstract_class: AbstractClass) -> None:
    """
    The client code calls the template method to execute the algorithm. Client
    code does not have to know the concrete class of an object it works with, as
    long as it works with objects through the interface of their base class.
    """

    # ...
    abstract_class.template_method()
    # ...


if __name__ == "__main__":
    print("Same client code can work with different subclasses:")
    client_code(ConcreteClass1())
    print("")

    print("Same client code can work with different subclasses:")
    client_code(ConcreteClass2())
```


