---
layout: post
title: Interpolation
subtitle: Reading between the lines
photo: low-angle-shot-of-high-rise-building-2096578.jpg
photo-alt: Interpolation
tags: [Optimisation, Python]
category: [Analysis]
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>


http://terpconnect.umd.edu/~petersd/460/spline_n.pdf
https://arxiv.org/pdf/1505.04648.pdf


Interpolation is a method of approximation that seeks to encode the underlying function of a set of known data points. 
Unlike regression where a best fit is sought for often noisy or imperfect data, interpolation assumes that the precise 
values of a function are known for a discrete grid of points. This can be useful to determine the value of a function at an unknown data point or as an optimisation technique to replace expensive caculations with a fast approximate method.

Two commonly used methods of interpolation are piecewise interpolation and polynomial interpolation.

Piecewise interpolation is in general stable however can show inferior performance to polynomial interpolation, which with the correct configuration can achieve machine precision.

The degree n Chebyshev polynomial is:
    $$T_{n}(x) = \cos(n \cos^{-1}(x))$$
    
This can also be expressed by the recurrance relation:

T_{n+1}(x) = 2xT_{n}(x) - T_{n-1}(x)
T_{0}(x) = 1
T_{1}(x) = x

T_{n}(x) has extrema at x_{i} = \cos( \frac{(i\pi}{n} ), i = 1, .., n.


Commonly the function is not explicitly decomposed as the basis Chebyshev polynomials but as a superposition of Chebyshev basis functions:

f(x) = \sum c_{i} T_{i}(x)


Chebyshev interpolants can be computed by the barcentric formula in a numerically stable way in linear time.

Interpolation works best for functions that can be expressed in an analytical mathematical expression. Functions that are 
a product of approximate calculations or Monte Carlo may exhibit noise which the interpolating function will also fit, 
degrading accuracy.

Weierstrass approximation theorem:

>>>    Every continuous function on a bounded interval can be approximated to arbitrary accuracy by polynomials.
Let f be a continuous function on [−1, 1], and let ε > 0 be arbitrary. Then there exists a
polynomial p such that
kf − pk < ε

## Lagrange polynomials

http://terpconnect.umd.edu/~petersd/460/interp460.pdf
Assume that x1,..., xn are different from each other. Then for any y1,..., yn there exists a unique polynomial
pn−1(x) = c0 +c1x+···+cn−1x
n−1
such that
p(xj) = yj
for j = 1,...,n

Approximate an analytic function by a polynomial.
For a set of _n_ points there is a unique polynomial of order _n - 1_ that interpolates them.


Commonly used for polynomial interpolation. For large n, polynomial interpolation is susceptible to Runge's phenomenon.

## Runge's phenomeneon

The higher order the interpolation the greater the chance of 'ringing', oscillations of the interpolation function.

## Chebyshev interpolation

If the grid can be chosen, the issue of ringing can be resolved by a careful choice of grid spacing. 

Chebyshev points are equidistant on the unit hemisphere. Interpolation using a Chebyshev shows better convergence properties 
than for other grid schemes.  

Error decays polynomially for differentiable functions and exponentially for analytic functions.


Error bound:

|f(x) - f_{interp}(x)| \leq (\frac{b - a}{2})^{n+1} \frac{B_{n+1}}{2^{n}(n+1)!}

Where B_{n+1} = max_{x \inset[a,b]}|f^{(n+1)}(x)|

Error estimate exponentially decreases with n.