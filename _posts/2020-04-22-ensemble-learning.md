---
layout: post
title: Ensemble learning
subtitle: Bagging and boosting 
photo: black-and-white-diamond-shape-wallpaper-1070345.jpg
photo-alt: Regression models
tags: [Regression, Python]
category: [Analysis]
---

https://towardsdatascience.com/https-medium-com-vishalmorde-xgboost-algorithm-long-she-may-rein-edd9f99be63d


Summarise: https://miro.medium.com/max/1400/1*QJZ6W-Pck_W7RlIDwUIN9Q.jpeg


## Bagging

Boostrap aggregation (Bagging) is a process of training an ensemble of predictors on an ensemble of data sets bootstrapped from the training set.


## Boosting

Ensemble method that combines several weak learners into a single strong learner. Predictors are sequentially added to the ensemble, trained to correct the mistakes of their predecessors and boost the overall learner performance. 


### Adaboost 

Adaptive boosting (Adaboost) is a classification boosting algorithm that adds predictors that are trained on a dataset that is reweighted by missclassifications of the model. 

Initially the training data are assigned a uniform weighting for all instances. As training progresses, training data points that are incorrectly classified are assigned a higher weighting and correctly classified points lower weightings. This ensures that the next predictor added to the ensemble focuses on learning where the ensemble performs the worst, until it learns the whole distribution.
  
Due to the fact the learning is sequential and reliant on prior iterations boosting cannot be parallelised, making scaling of the algorithm poor. 

### Gradient boosting


### XGBoost

Built on decision trees.

Better performance due to tree pruning na dbetter hardware management. Can also be parallelised.

https://towardsdatascience.com/https-medium-com-vishalmorde-xgboost-algorithm-long-she-may-rein-edd9f99be63d

https://machinelearningmastery.com/gentle-introduction-xgboost-applied-machine-learning/

## Stacking