---
layout: post
title: Keras TensorFlow
subtitle: 
photo: shapes-and-pattern-2346594.jpg
photo-alt: Keras, TensorFlow
tags: [Keras, TensorFlow, Python]
category: [Analysis]
---

TensorFlow is popular in the machine learning sphere for providing a powerful framework for building neural networks.


### The Keras API

Keras is a high-level API that enables users to use a variety of machine learning backends with a common, clean interface. This provided a popular interface to quickly building TensorFlow models without having to get dirty in its low-level API. With the release of TensorFlow v2 Keras has now been incorporated as a native high-level API. Existing users of Keras, are [recommended to use the new `tf.keras` interface](https://twitter.com/fchollet/status/1174018651449544704?s=19), as new developments with not be ported back to Keras.

```
tensor
```


## Parameters


### Compile

- `optimizer`: Function used in gradient descent.
- `loss`: 