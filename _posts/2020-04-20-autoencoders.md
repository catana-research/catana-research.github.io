---
layout: post
title: Autoencoders
subtitle: Interpolation
photo: abstract-art-blur-circle-96381.jpg
photo-alt: Encoding and decoding data with neural networks
tags: [Autoencoder, Python]
category: [Analysis]
---


Good source: https://towardsdatascience.com/understanding-variational-autoencoders-vaes-f70510919f73

https://www.pyimagesearch.com/2020/02/17/autoencoders-with-keras-tensorflow-and-deep-learning/

https://towardsdatascience.com/understanding-variational-autoencoders-vaes-f70510919f73

https://www.tensorflow.org/tutorials/generative/cvae


Unlike PCA where dimensions are required to be orthogonal, autoencoders have no such constraints.

- Tradeoff between encoding data compression with information loss. 


## Autoencoders

Autoencoders work by transforming data from feature space to a compressed latent space. This can then be decoded back to the feature space.


## Variational Autoencoders

- A variational autoencoder (VAE) is a special type of regularised autoencoder that encodes data to a distribution, rather than a point. Because of this, and the special properties of the latent space, new data can be generated from a trained VAE by sampling the latent distribution.

- In practice, the encoded distributions are chosen to be normal so that the encoder can be trained to return the mean and the covariance matrix that describe these Gaussians. 

- A VAE can be defined as being an autoencoder whose training is regularised to avoid overfitting and ensure that the latent space has good properties that enable generative process. The latent space should exhibit _continuity_, that is that close points should be decoded to a similar value, and _completeness_, every point in the latent space should be mapped to the decoded space.

- To avoid overfitting, the covariance matrix and mean of the returned distributions returned by the encoder are regularised.

