---
layout: post
title: Numba
subtitle: Just in time computation
photo: light-landscape-nature-red-33041.jpg
photo-alt: Clustering data
tags: [Numba, Python]
category: [Optimisation]
---

Python is a great language to develop in as it is easier and faster to write and test code. Its main drawback is its execution speed which is often slower than more low-level languages, such as C++. There are however several Python packages, such as [Numba](https://numba.pydata.org/numba-doc/latest/index.html), that enable code to be executed using low-level objects which gives comparable speed to C++.

### Just-in-time compilation 

_Numba_ is a _just-in-time_ (JIT) compiler that translates at runtime Python code to more efficient precompiled functions written in C++. This can provide significant speedups to Python code that is computationally and loop intensive. One of the benefits of using Numba is that it is simple to implement, requiring only the addition of decorators to Python functions to be leveraged.
 
To demonstrate the capabilities of Numba consider    
         
                                                    

### Nopython

- Never pass Pandas or Python datatypes to Numba or Cython optimised functions. Instead use pass an `np.ndarray` using the `.to_numpy()` operator (use of `.values` is being deprecated). 
- If you are unsure whether the objects you are passing to numba are in a no Python format you can specify `nopython=True` in the decorator. This ensures numba runs without Python objects and if there are any such objects they can be easily identified as they will throw an error during execution.
- Ensure all that all `numba` functions with `nopython=True` also call functions with `nopython=True`. A function called without `nopython=True` will cause the rest to run in Python mode, losing the performance gains.



### Inspecting the execution plan

The implementation of 

f.inspect_types()


### Numba vs Numpy 

In most cases Numba will have similar performance to in-built Numpy functions, with a maximum speedup of a factor of 2 expected in the best case. When considering a Numba implementation it is worth first considering if an efficient Numpy functional implementation already exists, in which case it may be easier and maintainable to go with Numpy. In either case, its always worth benchmarking both to compare their performance. 