---
layout: post
title: TensorFlow - Imbalanced data
subtitle: Classification with unbalanced data
photo: low-angle-shot-of-lumbers-3391128.jpg
photo-alt: Spark
tags: [Spark, Python]
category: [Systems]
---


https://www.tensorflow.org/tutorials/structured_data/imbalanced_data
https://colab.research.google.com/github/tensorflow/docs/blob/master/site/en/tutorials/structured_data/imbalanced_data.ipynb


Data that is highly imbalanced can make training a classification of an unbiased model difficult. This can occur in datasets where an outcome is rare, such as in fraud detection.

This issue can be resolved by many different techniques such as:

- Class weighting
- Oversampling and undersampling

## Metrics

In severe imbalances, the `accuracy` metric is no longer useful, as a model trained on a dataset that is composed 99% of one class will give a 99% accuracy if it always guesses that class.



## Mitigation methods

Two common methods are class weighting and under/over sampling.

## Oversampling and undersampling

- Undersampling should only be attempted if the number of events in the smaller class is approximately 100,000
