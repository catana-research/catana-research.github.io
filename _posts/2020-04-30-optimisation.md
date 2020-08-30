---
layout: post
title: Optimisation
subtitle: 
photo: gray-concrete-post-tunnel-1531677.jpg
photo-alt: Clustering data
tags: [Python]
category: [Optimisation]
---

Ensuring optimal code execution is a prime consideration for many projects where inefficient code can lead to long processing times or high hardware loads. The process of optimisation is therefore an important component of the development process which can in some cases reduce computational requirements by orders of magnitude resulting in significant cost savings.

This series will explore common optimisation techniques with an emphasis on Python development.


### Analysis of bottlenecks

Before starting any optimisations an assessment of the bottlenecks and performance issues of the program should first be
ascertained. There is little point in spending effort in optimising a component of the code that has little bearing on 
the overall performance of the code.

The first step before any optimisation should therefore be a top down analysis of the current performance of the code. 
This ensures optimisations target components that have the greatest impact and helps determine which types of optimisations 
are applicable.
 
Care should be taken to measure the performance of a representative sample of the code. A component that runs slower but 
fewer times can have a smaller impact than a faster component that is run multiple times. 

The type of performance issue should also be considered. If the calculation is IO-bound then improving reducing the data transfer or improving data transfer 
rates is most effective. If a calculation is CPU bound then improving the processing efficiency is more effective. If 
 optimisations are wrongly targeted the improvement in performance will be smaller or negligible.
    

Timing can be performed with a profiler, such as `cprofile`, by timing blocks of code or from an analysis of system logs.
 
When you have 


### Improving performance of the code

One method of improving performance is to increase the efficiency of existing code. This leaves the algorithm   

Where possible use:
- [Numpy]()
- Numba, Cython
- C++ or low level language for CPU intensive calculations

Such optimisations can be easy to implement, resulting in noticable speedups with smaller effort. These are also typically 
safer to implement as they can require less changes to the code that may introduce bugs.

System optimisations:
- Using faster disk storage (SSD)
- Using more processing power (multithreading if parallelisable)

Multithreading can be powerful for embarrassingly parallisable problems and there exist frameworks, such as dask distributed, 
that makes multithreading of code easy without major code changes. For more complex problems however the issues associated 
with multithreading such as race conditions and thread locks can be more trouble than they are worth. Most grid infrastructure 
also performs better with distributed single threaded programs.  

 

### Improve the algorithms utilised

Solution implementations can often be naive, working well for low data volumes but scaling poorly. A lot of research has 
been conducted into the best methods of solving a variety of problems and it is unlikely you will be able to better them. 
E.g. binary search, m-map

### Remove redundant calculations

Paradoxically, one of the most effective methods at optimising calculations is not to perform the calculation in the first 
place. Being smarter in your algorithm through caching of intermediate results or removing redundant operations has one
of the greatest capabilities to improve performance.

### Approximations

If acceptable, CPU-intensive calculations can be replaced by faster but less accurate algorithms. Some approximate 
methods, such as _Chebyshev interpolation_, however can provide results near to exact as the original calculation for a 
fraction of the computation time.

### Validation

Check result against original code for a sufficient breadth of test input data.

It is important that any code changes made during the optimisation process are captured in the testing suite. By incorporating 
new unit and regression tests, any new edge cases that may have been introduced can be monitored.  
 
 
## Optimisation workflow

In summary, the main methods to improve the performance of a program are:
0. Analysis of bottlenecks
1. Improve the performance of code and systems
2. Improve the algorithms utilised
3. Remove redundant calculations
4. Approximate calculations
5. Validate code changes and performance 

This is an iterative process and can be repeated until the desired performance is reached.

