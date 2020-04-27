---
layout: post
title: Interpolation
subtitle: Reading between the lines
photo: low-angle-shot-of-high-rise-building-2096578.jpg
photo-alt: Interpolation
tags: [Optimisation, Python]
category: [Analysis]
---

Method to determine the value of a function for which precise values are known for a discrete grid of points. For a set of 
_n_ points there is a unique polynomial of order _n - 1_ that interpolates them.


## Lagrange polynomials

Commonly used for polynomial interpolation. Susceptable to Runge's phenomeneon

## Runge's phenomeneon

The higher order the interpolation the greater the chance of 'ringing', oscillations of the interpolation function.

## Chebyshev interpolation

If the grid can be chosen, the issue of ringing can be resolved by a careful choice of grid spacing. 

Chebyshev points are equidistant on the unit sphere. Interpolation using a Chebyshev shows better convergence properties 
than for other grid schemes.  