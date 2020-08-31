---
layout: post
title: Optimisation
subtitle: Improving execution performance
photo: gray-concrete-post-tunnel-1531677.jpg
photo-alt: Optimisation
tags: [Python]
category: [Optimisation]
---

Ensuring optimal code execution is a prime consideration for many projects where inefficient code can lead to long processing times or high hardware loads. The process of optimisation is therefore an important component of the development process which can in some cases reduce computational requirements by orders of magnitude resulting in significant cost savings.

This series will explore common development optimisation techniques with an emphasis on Python development.


## Optimisation prerequisities

### Analysis of bottlenecks

Before starting any optimisations, an assessment of the bottlenecks and performance issues of the program should first be ascertained. There is little point in spending effort in optimising a component that has little bearing on the overall performance of the code.

A top-down analysis of the current performance of the code should therefore be performed prior to any optimisation. This helps to determine which types of optimisations are applicable. The performance impact of an inappropriate optimisation will often be small and add undue complexity making maintenance more difficult. For example if the calculation is IO-bound (the CPU is often idle and disk or network is busy) then reducing the data footprint or improving data transfer rate would be most effective solution. Conversely, if a calculation is fully utilising the CPU-bound (busy CPU and low disk or network utilisation) then improving the processing efficiency is more effective. 

Optimisations should target components of greatest impact and stop where the trade-off between speed and maintainability becomes an issue. Often the largest impact is to be found in the big loops of an application, where a small slowdown is amplified by each iteration, or in the compute or IO heavy parts of the code which stretch the hardware to its limits. There is little point in optimising a process that takes a few seconds or minutes if the slowest component takes hours, as this adds more to the code complexity that have developers to understand and maintain, potentially causing more problems than it solves.    

### Quantifying performance

Depending on the 

Choose a profiler appropriate to your need, `cprofile` measures the CPU time, if you are more interested in wall clock time there are other other profilers. 

Timing can be performed with a profiler, such as `cprofile` which comes with the Python distribution, by timing blocks of code or from an analysis of system logs. IDEs with `iPython` support have in addition magics such as `%timeit` and `%prun` that can also be used for this purpose.
 
When you have 


Care should be taken to measure the performance of a representative execution of the code. A component that runs slower but fewer times can have a smaller impact than a faster component that is called multiple times. 



![snakeviz profile](..\assets\images\posts\2020-04-30-optimisation\snakeviz_profile.png)


### Benchmarks and testing

Prior to performing any optimisations, a comprehensive test uite should first be in place. This ensures that any code changes or refactoring does not change the operation in an unintended 

## Optimisation

### Improving code performance

After identifying the bottlenecks in the program a good starting point is 
One method of improving performance is to increase the underlying efficiency of existing code which changes to the algorithm.   

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

 

### Improving the algorithm

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

