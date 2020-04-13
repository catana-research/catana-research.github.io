---
layout: post
title: TensorFlow
subtitle: 
photo: abstract-art-blur-bright-373543.jpg
photo-alt: TensorFlow
tags: [TensorFlow, Python]
category: [Analysis]
---

TensorFlow is popular in the machine learning sphere for providing a powerful framework for building neural networks.


### Restricting VRAM usage

By default TensorFlow will allocate all video memory when executing a calculation which can be problematic if you are trying to share the same GPU resource across multiple simulataneous calculations. You can however request that only the required amount of memory is allocated and allow growth as required. As explained in the [documentation](https://www.tensorflow.org/guide/gpu#limiting_gpu_memory_growth) this can be achieved by the following:

```
gpu_devices = tf.config.experimental.list_physical_devices('GPU')
for device in gpu_devices:
    tf.config.experimental.set_memory_growth(device, True)
````