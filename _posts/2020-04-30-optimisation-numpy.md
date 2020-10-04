---
layout: post
title: Numpy
subtitle: Numerical computing in Python
photo: photo-of-cave-rock-formation-2537642.jpg
photo-alt: Numpy
tags: [Numpy, Python]
category: [Optimisation]
---

*Numpy* is an extremely popular Python library for scientific and high-performance applications, that enables calculations in Python to be efficiently performed by executing calculations using precompiled libraries written in C/C++ and Fortran. The [API](https://numpy.org/doc/1.19/reference/index.html) is extensive and well-documented, making it easy to substitute Python operations with the more efficient operations of Numpy.


## The `nd.array`

The underlying array data structure of Numpy is the `nd.array`, a multidimensional array that stores data efficiently in a contiguous block of memory in row-major `'C'` order. The objects in the array are stored with a datatype specified by `dtype`, along with `shape`, a description of its size and its `stride`, which describes how to iterate over the container. Because the underlying array and its methods, are executed from precompiled C++, calculations with `nd.arrays` are very efficient.

The `nd.array` instance and its slices are views of the underlying data location and can be passed around efficiently. When using multiple views care should be taken, as changes to any object referencing to this data will affect all other `nd.array` objects. If you wish to keep the arrays separate the `.copy` operator can be used.

By default, the array is row-major ordered, making iteration of row data very efficient. If this is not the desired behaviour you can specify column-major 'Fortran' order using the `order='F'` kwarg.


Where possible, preallocate arrays of the correct size. The fastest way to do this is to use `np.empty`, which pre-allocates without initialisation an array of the specified datatype. For instance an `ndarray` of 10 64-bit floating-point numbers can be quickly allocated by:
```python
np.empty(10, dtype=np.float64)
```
If having elements of the array left uninitialised makes you uncomfortable you can alternatively use `np.full` which also initialises a default value, in this example we set all elements to zero:
```python
np.full(10, 0., dtype=np.float64)
```
This comes with a performance hit, with the allocation of an array of a million floats taking 20 times longer for `np.full`.


It is preferable to use Numpy functionality and data types instead of Python where possible, using `nd.array` instead of `list`. For instance, when determining the unique elements of a list it is 5x slower to use the following Python call: 
```python
unique_elemenst = list(set(x))
```

Than the corresponding Numpy function:
```python
unique_elemenst = np.unique(x)
```

## Vectorize

Vectorization is extremely powerful in Numpy as it enables code to not only be written in a compact way but also greatly improves performance by removing Python loops. 

## Efficienct looping 

By default Numpy arrays are row-ordered, with contiguous memory across the highest dimension. It is therefore more efficient to iterate over the highest dimension than lower dimensions:

```python
arr[:, 0]  # Is inefficient
arr[0, :]  # Is efficient
```

## `ufunc`



## Use numpy methods instead of Python




## Avoid copying data, use `inplace` where appropriate




- Use Numpy functionality instead of Python where possible, for example use `nd.array` instead of `list`.