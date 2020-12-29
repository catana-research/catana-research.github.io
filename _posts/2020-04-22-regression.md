---
layout: post
title: Regression
subtitle: Quantification models
photo: time-lapse-photo-of-stars-on-night-924824.jpg
photo-alt: Regression models
tags: [Regression, Python]
category: [Analysis]
---

Overview of basic regression models.

Supervised learning algorithm where the target is a numeric value.

A general model can be expressed in terms of a hypothesis function $h_{\theta}$, with parameters $\theta$, that maps a feature vector $\underline{x}$ by:

$\caret{y} = h_{\theta}(\underbar{x})$

Where $x_{i}$ is the value of the ith feature.

The construction of the hypothesis function is dependent on the model.

## Linear regression

A linear model, as the name suggests, is linear in the input feature data.
$h_{\theta}(\underbar{x}) = \theta_{0} + \theta_{1}x_{1} + ... + \theta_{n}x_{n} = \underline{\theta}^{T} \cdot \underline{x}$

There is a closed form solution, the _normal equation_, which provides $\caret{\theta}}$, the value of $\theta$ that minimises the cost function:

$\caret{\underline{\theta}} = (X^{T} \cdot X)^{-1}\cdot X^{T} \cdot y$

As the inversion operation of a matrix is typically O(n^{2}), for large n the inversion of the (n x n) matrix (X^{T} \cdot X) can be calculationally prohibitive. THe algorithm however scales linearly with the number of training instances, m.

The Linear model can also be solved with gradient descent. As the cost function is convex, convergence to the single global minimum is efficient. 

Loss function: $J(\theta) = MSE(\theta)$


### Polynomial regression

Linear regression can only model linear relationships for the features. Non-linearities can be fitted by a transformation of the features to contain high order represenations such as $x_{1}^{2}$. This also includes all cross-terms $x_{i}\cdotx_{2}$ etc. This however blows up quite quickly, as $n$ features undergoing a polynomial expansion of degree $d$ will produce $\frac{(n + d)!}{n!d!}$ terms.   

### Regularised models

Regularised models seek to reduce the effect of overfitting by the addition of terms to the cost function which penalise model complexity.

#### Ridge regression

Ridge regression adds a new penalty hyperparameter $\alpha$, the regularisation strength, that penalises model weights with the addition of a term that is the $l_{2}$-norm of the weight vector to the loss function:

Loss function: 

$J_{Ridge}(\theta) = MSE(\theta) + \frac{\alpha{2}\sum_{i=1}^{n}\theta_{i}^{2}$
This again has an analytic solution:
$\caret{\underline{\theta}} = (X^{T} \cdot X + \alpha {\mathbf A})^{-1}\cdot X^{T} \cdot y$
Where A is the $n \times n$ indenty matrix where the top left entry is zero.
Larger values of $\alpha$ add tighter constraints on the complexity of the model. Note that the linear regression model is just a special case where $\alpha = 0$, i.e. no regularisation.  

Lasso (Least Absolute Shrinkage and Selection Operator) regression also adds a penalty hyperparameter however this is a $l_{1}$ norm of the weight vector. The corresponding cost function is:
$J_{Lasso}(\theta) = MSE(\theta) + \alpha\sum_{i=1}^{n}|\theta_{i}|$

Lasso has a useful property of reducing the weights for features that have the least importance, enabling it to be used a method of feature selection.


## SVM

SVM (Support Vector Machine) works by splitting classes by a boundary which maximises their separation. In regression, the SVM seeks to find a bounary that minimises the data outside the street.  The vanilla algorithm only enables a linear separation of data which if non-linear leads to a poor model fit.  The _Kernel trick_ however can be utilised to extend SVM to support non-linear fitting, which enables an extension of high-order features without the combinatorial explosion of polynomial regression.



## Decision tree

Learns data by a series of true/false questions in a tree. Arranged in nodes which terminate at leaf nodes.



Pruning is act of removing nodes from the tree for which its child leaves have no statistical predictive power.

## Random forest

Random forests are built from an ensemble of decision trees. This ensemble learning averages the prediction of a large number of weak learners, improving performance and reducing overfitting. Random forests have two key features that improve their performance:
- **Bagging** - Bootstrap aggregation (bagging) randomly selects with replacement sub selections of the training data, building individual trees that only use the selected subset. This avoids overfitting as the different trees in the ensemble are trained on different parts of the data.
- **Random feature selection** - A downside of bagging is the resultant trees are still highly correlated with one another. To improve the generality of training, trees are only trained on a random subset of the features, ensuring that different trees in the forest learn different patterns in the data. 

- Like decision trees are scale invariant.



# Model training

- Train models on training data and evaluate best performant via cross-validation.
- Tune model hyperparameters using a grid search. Number of iterations = CV-folds * number of hyperparameter combinations.



## Learning curves

The performance of trained models can be impaired by an underfitting or overfitting of the training data. A common method to diagnose if this occuring is to take a look at the learning curves of the model. This compares the measured error in the training set, _training error_ and in the validation sets, _validation error_, as a function of the training set size. 

In general the measured error of the training set for low training set sizes will start low, as models will fit few data points well. As more data is added the performance will decrease, converging to the training error of the model. The validation error conversely will typically start high, as the few data points are likely very different from the training data, and will decrease to converge to the estimated valdation error of the sample. 

Bias-variance tradeoff - High bias, low variance models underfit. Low bias, high variance models overfit. 

The underlying noise and errors in the data will in addition lead to an irreducible error which is the fundamental minimum error.


### Underfitting

When underfitting adding more training data does little to improve the performance of the model as it is not sophistocated enough to incorporate all the features of the data. A common indication of underfitting is a learning curve where there training error plateaus at a high measured error, typically close to the measured validation error. 


### Overfitting

When overfitting, the training error is typically significantly lower than the validation error. This is because the data is learning the training set, resulting in a low training error, but it generalising poorly and hence has a large validation error, a measure of the generalisation error. 




## Hyperparameter optimisation

Be mindful of what each of the hyperparameters of the model achieves to determine which ranges will likely provide best performance. It can be more efficient to first perform a coarse grid search, then a finer grid search around the points of highest performance.



"""
After several models have been selected, fine-tune the hyperparameters to improve their performance.

GridSearchCV will iteratively train and evaluate the model over the list of specified hyperparameters
The below example will perform:
- 3 x 4 = 12 : n_estimators, max_features evaluations
- 1 x 2 x 3 = 6 : bootstrap, n_estimators, max_features evaluations
 => Total 18 evaluations
In addition the 5-fold CV will result in a total:
 18 x 5 = 90 rounds of training and evaluation  
"""

from sklearn.model_selection import GridSearchCV
# These are parameters specific to the RandomForestClass:
#   n_estimators : The number of trees in the forest.
#   max_features: The number of features to consider when looking for the best split:
#   bootstrap: Whether bootstrap samples are used when building trees.
param_grid = [
    {'n_estimators': [3, 10, 30], 'max_features': [2, 4, 6, 8]}, # With bootstrap on
    {'bootstrap': [False], 'n_estimators': [3, 10], 'max_features': [2, 3, 4]}, # With bootstrap off
]
forest_reg = RandomForestRegressor()
grid_search = GridSearchCV(forest_reg, param_grid, cv=5, n_jobs=4,
                           scoring='neg_mean_squared_error')
# Takes a long time, don't run unless you need to
grid_search.fit(housing_prepared, housing_labels)


# Get the best hyperparameter combination:
grid_search.best_params_
"""
Result for best params:
    {'max_features': 6, 'n_estimators': 30}
"""

# Get the corresponding model:
grid_search.best_estimator_
"""
RandomForestRegressor(bootstrap=True, criterion='mse', max_depth=None,
           max_features=6, max_leaf_nodes=None, min_impurity_decrease=0.0,
           min_impurity_split=None, min_samples_leaf=1,
           min_samples_split=2, min_weight_fraction_leaf=0.0,
           n_estimators=30, n_jobs=None, oob_score=False,
           random_state=None, verbose=0, warm_start=False)
"""

# Get all results:
pd.DataFrame(grid_search.cv_results_)