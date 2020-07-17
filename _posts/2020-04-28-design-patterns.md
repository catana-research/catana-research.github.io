---
layout: post
title: Design patterns
subtitle: An introduction
photo: red-glittered-wallpaper-751373.jpg
photo-alt: Design patterns
tags: [Design patterns, Python]
category: [Systems]
---

Building programs without a good design can lead to spaghetti code that is unstable and difficult to maintain. Without common standards, it is difficult for multiple developers to work on the same project in a harmoneous way.

*Design patterns* are a collection of common design structures that prescribe a common methodology for the implementation of a solution. They are techniques that have been developed to provide a maintainable and scalable solution to common problems. This also helps to better communicate the design of a solution by expressing it in terms of commonly known structures.


Design patterns help to decouple the logic in different parts of the solution, making maintenance easier.

## Core design principles

Design patterns follow a core set of principles to ensure solutions are maintainable and extensible. These crystallise some of the better instincts that developers have in mind when developing solutions and discourage those that are bad. 

### Encapsulate static code
- Encapsulate what varies (needs to changed in future) and separate it from what stays the same (which should be defined in the base class).
    - This ensures that changes made only affect the behaviour of this encapsulated code.
    - Encapsulation enables other Classes to reuse the logic that are not derived from the base class.
    
- Classes should be open for extension but closed for modification.
    - New features should be able to be added without the need to modify existing code.
- Favour composition over inheritance.
    - Inheritance is static at runtime and all subclasses must inherit the same behaviour.
    - Inheritance leads to class explosion, rigid design and functionality not appropriate to subclasses. 
    - Encapsulating the unchanging structure and composing behaviours reduces this issue and allows the functionality to be extended beyond the original design of the base class. 

- Code reuse
    - Avoid duplicating code in multiple areas. Better tested.

- Program to an interface or base class, not to an implementation.
    - Guidelines:
        - No variable should hold a reference to a concrete class, use factories instead.
        - No class should derive from a concrete class, only from an abstract class. Otherwise the classes will be tightly coupled. 
       - No method should override an implemented method of any of its base classes, a base class should be a static pure abstraction. If a method needs to be overridden, it should not be in the base class.
    - For example:
    
```python
# Programming to an implementation
d = Dog()
d.bark()

# Programming to an interface
animal = Dog()
animal.make_noise()
```
    
- Strive for loosely coupled designs between objects that interact.
    - Loosely coupled designs allows for a flexible system as the minimize the interdependency between objects.
    - Strongly coupled designs make changes difficult without changing/breaking code in multiple places.

- Depend on abstractions, do not depend on concrete classes.

- Try and split logic for IO and processing to seperate functions or classes.


## Preliminaries UML

As design patterns describe the structure of code, UML is an effective method of explaining their structure.  

![UML diagram](/assets/images/posts/2020-04-28-design-patterns/UML-symbols.jpg)


## Common patterns

In the following we will look at a few common patterns and describe their use in solving problems. For each we will sketch out an abstract structure of the pattern followed by a more realistic example.

Although design patterns can add better structure to code, they are not always appropriate. Sometimes it is better to have a simple than an overengineered solution. Often it is quicker to develop a prototype and only then determine if a better structure is required. 
