---
layout: post
title: Optimisation
subtitle: Improving execution performance
photo: gray-concrete-post-tunnel-1531677.jpg
photo-alt: Optimisation
tags: [Python]
category: [Optimisation]
---

Ensuring that code effectively utilises resources is a prime consideration for many projects, where inefficient code can lead to long processing times or large hardware loads. Code optimisation is therefore an important component of the development process that can more than pay off the extra effort. In some cases optimisations can reduce computational requirements and processing times by several orders of magnitude, significantly reducing running costs.

## Optimisation 
This series will explore common development optimisation techniques with an emphasis on Python development.

### Prerequisities
As with any refactoring, it is important to ensure that the functional behaviour of the code is unchanged after the application of optimisations. It is therefore critical that a suite of automated tests is in place before optimisations are implemented. This ensures that the functionality behaviour of the code is unchanged and streamlines the process of testing optimisation changes. 

### Analysis of bottlenecks
Before starting any optimisations, an assessment of the bottlenecks and performance issues of the program should first be ascertained. Optimisations should target components of greatest impact and stop where the trade-off between speed and maintainability becomes an issue. Often the largest impact is to be found in the big loops of an application, where a small slowdown is amplified by each iteration, or in the CPU/IO intensive parts of the calculation which stretch the hardware to its limits. There is little point in optimising a process that takes a few seconds or minutes if the slowest component takes hours, as this adds more to the code complexity that have developers to understand and maintain, potentially causing more problems than it solves.

A top-down analysis of the current performance of the code should therefore be performed prior to any optimisation to determine which types of optimisations are applicable. The performance impact of an inappropriate optimisation will often be small and add undue complexity making maintenance more difficult. For example if the calculation is IO-bound (the disk/network is busy and CPU is idle) then reducing the data footprint or improving data transfer rate would be most effective solution. Conversely, if a calculation is CPU-bound (the CPU is busy with low disk/network utilisation) then improving the processing efficiency or paralleism is more effective. 

### Quantifying performance
Timing can be performed with a profiler, by timing blocks of code or from an analysis of system logs. IDEs with `iPython` support have in addition magics such as `%timeit` and `%prun` that can also be used for this purpose.
Common tools in measuring the performance of code execution include: *profilers*, *timers* and *logs*.

The profiler is a tool for measuring the statistics of the execution of a program to quantify performance and identify bottlenecks. This typically can be performed by *deterministic* and *line* profilers. Deterministic profilers typically have the lowest overhead however only provide statistics for the utilisation at a function-level. Line profilers provide utilisation for each line of code however have a large overhead and output format. It is best to use deterministic profilers to identify general areas of inefficiency and target line profilers on sections of code to more precisely isolate problem areas.   

Care should be taken to measure the performance of a representative execution of the code. A component that runs slower but fewer times can have a smaller impact than a faster component that is called multiple times.

The Python distribution already comes with a C-based high-performance deterministic profiler `cProfile`. This can be added to code simply by:
```python
import cProfile

profile = cProfile.Profile()
profile.enable()

# Code block to profile
# ...

profile.disable()
profile.dump_stats('C:/Temp/profile.dmp')
```

The output of the profiler can be analysed by several methods, perhaps the most convenient is the interactive web browser interface [SnakeViz](https://pypi.org/project/snakeviz/), that enables you to visualise a drilldown of all functions encapusulated by the profile dump. This can be analysed by running SnakeViz with your dump file:

```cmd
snakeviz.exe profile.dmp
```
![snakeviz profile](..\assets\images\posts\2020-04-30-optimisation\snakeviz_profile.png)

Important statistics from the profile include:
- `ncalls`: The number of times this function is called. Functions with high numbers of calls will can the greatest benefit from optimisation.
- `cumttime`: The total time spent in this function. Functions with high `cumttime` are a good target for optimisation as they will be the largest component of the running time.
- `tottime`: The total time spent in this function, excluding any time spent in other functions within the function. If the `tottime` is similar in magnitude to the `cumtime` it means the majority of the time is spent in functions called within the function, which should be the targets of optimisation. 
- `percall`: This represents the average of the above statistics, dividing the times by `ncalls`.


Line profiling can be performed using [pprofile](https://pypi.org/project/pprofile/). This is considerably slower to run than other profiling methods and it produces a lot of output (every line of code hit) so make sure to specifically target offending code for this type of profiling.



Note that optimisations should not be targeted solely on making each function of the code faster but instead at improving the end-to-end performance of the code. Sometimes the performance of the program is improved by adding more processing that make some functions slower. For example adding additional processing to transform data to a more efficient format might make the processing stage less efficient but the rest of the application significantly faster.

#### Memory
```python

import tracemalloc
tracemalloc.start(10) # Set stack depth
time1 = tracemalloc.take_snapshot() # Before snapshot

# Block to profile-

time2 = tracemalloc.take_snapshot() # After snapshot
stats = time2.compare_to(time1, 'lineno') # Compare snapshots
for stat in stats[:3]:
    print(stat)

```



### Benchmarks and testing
To quantify The current performance should be benchmarked in order to measure how much faster a change is. You may find that sometimes an optimisation leads to worse performance so it is important to monitor and ensure it is behaving as expected. 


### Milliseconds count

It may seem that shaving a few milliseconds off a function call is a minor thing but when processing large amounts of data it can quickly add up. A saving of 1 millisecond per loop over a million dataentries for example adds over 15 minutes to the runtime.  

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

Performance testing to ensure that further changes do not sufficiently degrade performance.
 
 
## Optimisation workflow
In summary, the main methods to improve the performance of a program are:
0. Analysis of bottlenecks
1. Improve the performance of code and systems
2. Improve the algorithms utilised
3. Remove redundant calculations
4. Approximate calculations
5. Validate code changes and performance

This is an iterative process and can be repeated until the desired performance is achieved.
