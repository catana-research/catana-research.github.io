---
layout: post
title: Parallelisation
subtitle: Concurrent processing
photo: gray-sand-dunes-3155583.jpg
photo-alt: Parallelisation
tags: [Optimisation, Parallistion, Dask]
category: [Systems]
---


Parallelisation




### Parallisation scaling
A naive expectation of parallelisation is that a doubling of the processing power will half the wall clock time. In practice however however the speedup is always smaller due to several factors including the additional overhead of parallelisation and the effective utilisiation of threads.

For instance, the additional effort to serialise, distribute and collate the computation from multiple nodes adds to the running time and asymmetry in job complexity means, for some distribution schemes, nodes will be idle whilst they wait for other nodes to finish. 

In addition not all the processes can be parallised, leading to a limit on the speedup from parallisation, which is described by [Amdahl's law](https://en.wikipedia.org/wiki/Amdahl%27s_law). If the proportion of the execution time that can be parallelised is given by $p$, then the maximum improvement by a parallelisation across $s$ threads is given by:
  $\frac{1}{(1 - p) + \frac{p}{s}$
  
This is useful in determining both the expected speedup given the thread resources and also in determining maximum improvement to the processing time as $s$ becomes large: 
$\frac{1}{1 - p}$

**TODO: Possible to put Altair plot here?**

For example, if 95% of the program can be parallelised then given unlimited threads the maximum speedup that can be achieved is 20x. This also illustrates the exponential increase in resources required for improvement, with 50% of this speedup achieved with the first 19 threads and 95% of this speedup requiring 380 threads. This relationship also demonstrates that performance can be improved by increasing the proportion of execution time that is parallelised. In this case, if the proportion of parallelised code can be increased to ~100% we can achieve the same performance as a cluster with 380 threads with only 19.