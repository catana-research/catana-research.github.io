---
layout: post
title: Bootstrap
subtitle: Estimating errors from data
photo: closeup-photo-of-brown-lace-up-boot-936361.jpg
photo-alt: Bootstrap
tags: [Bootstrap, Python]
category: [Analysis]
---

A common problem when performing predictions with a model is a suitable estimate of the error in a prediction. For simple models there are often analytical forms that can be derived however for complex models this may not be possible. The prediction of a model is only useful if we can say something of its uncertainty so how do we resolve this issue?

## Bootstrap

- The Bootstrap method is an empirical method of error estimation that uses the distribution of the variable of interest in order to estimate statical properties.
- The Bootstrap method uses a sampling with replacement to a collection of bootstrap samples, upon which a statistical analysis can be performed. These bootstrap samples seek to emulate the underlying population from which the dataset has been drawn.

- It is important that the bootstrap samples are of the same size of the original sample.
- The number of bootstrap samples should be large enough for the result to converge to a stable value, typically this is 500-1000 samples. 

## Bootstrap in practice - Numpy 
