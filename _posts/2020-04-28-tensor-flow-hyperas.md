---
layout: post
title: TensorFlow - Hyperas
subtitle: Automated hyperparameter optimisation 
photo: spiral-staircase-with-blue-glass-window-3676619.jpg
photo-alt: Spark
tags: [Spark, Python]
category: [Systems]
---


Automated hyperparameter optimisation.

`GridSearchCV` is typically used for hyperparameter optimisation however it cannot exploit the parallelisation of GPU processing. `Hyperas` is designed to exploit parallelisation, which can make it 10x faster.



- Hyperas uses a random search.
- Works for dense networks, not good for CNNs.

- https://towardsdatascience.com/keras-hyperparameter-tuning-in-google-colab-using-hyperas-624fa4bbf673

- https://maxpumperla.com/hyperas/

CNNs
- https://towardsdatascience.com/a-guide-to-an-efficient-way-to-build-neural-network-architectures-part-i-hyper-parameter-8129009f131b
- https://towardsdatascience.com/a-guide-to-an-efficient-way-to-build-neural-network-architectures-part-ii-hyper-parameter-42efca01e5d7


## Performance optimisations

https://www.tensorflow.org/guide/data_performance



### Over and underfitting

For every model there is a bias-variance trade-off. The model model should aim for an optimal complexity that minimises the total bias and variance errors. A model with high bias is said to *underfit* and a model with high variance *overfits*. Whether a model under or over fits can be determined from analysing learning rate curves, the train and validation errors with the size of the training set/epochs. 

In each case, the following actions can be taken to counteract:

- Overfitting (High variance)
    - Increase number of training examples
    - Reduce number of features (dataset complexity)
    - Increase regularisation of the model to penalize complexity
    
- Underfitting (High bias)
    - Increase number of features
    - Enrich features for example products and powers
    - Decrease the regularisation of the model
    
Smaller NN tend to underfit, deeper NNs tend to overfit. Starting with a single layer and increasing in depth can help to find the optimal network depth that minises the bias-variance error.

## Model development process

The process of developing a machine learning model is not simply a case of throwing the latest and greatest architecture at a problem. Each problem is unique and requires a careful thought process to produce a model that has accurate predictions. An overview of this process is described in the following. 

- https://towardsdatascience.com/a-guide-to-an-efficient-way-to-build-neural-network-architectures-part-i-hyper-parameter-8129009f131b
- http://karpathy.github.io/2019/04/25/recipe/



### Analysis of data
- Spend a good deal of time looking at the data. Look for patterns that could be exploited, discover useful features of the data and potential data errors. This will give an indication of what architecture to employ. Sometimes there will be non-physical artifacts, such as the flooring or capping of values which should be removed or remedied. If the problem is a classification, are the classes equally populated? Are there duplicates, outliers or data biases? Could the data be reduced by removing features, a dimensionality reduction or downscaling?

### Preparation of data
- Select the correct `metrics` for the task and data. This does not affect the trainging of the model, only the `loss` function has influence on the gradient descent. Be careful selecting metrics, for example using `accuracy` for a classification task on deeply imbalanced data will lead the false intrepretation of the model under or over performing.
- Split data into `Train`, `Validation` and `Test` sets, typically split 60%-20%-20%. 
    - `Train`: Data you will train the model with. This should be  representative of the problem you are trying to solve as a difference in underlying distributions and correlations will result in a model that will operate poorly when used in practice.  
    - `Validation`: Data used to validate the model training with but never included in the training. The validation set is used to select the best model configuration, prior to and after hyperparameter optimisation. A comparison of the training and validation learning curves can be used to determine if the model is under or over fitting.
    - `Test`: Dataset used to estimate the generalisation error of the model. To avoid bias in the model development, this set should be set aside at the start and only used to measure the model error after the final model selection and hyperparameter training is completed. 
- Care must be taken at every step to never allow information from the validation and test sets to contaminate the training set.
- Feature scaling of the training data should be applied to increase the speed and effectiveness of training. Normalisation/Standardisation should be applied to ensure each feature has comparable weighting, reducing bias of features that have numerically larger values. Distributions with tails should be rescaled with transformations, such as a log-scaling, to make them more Gaussian-like, as models generalise poorly on distributions with long tails. Remember to learn the transform from the training set and apply it to all datasets. For example use `fit_transform` on the training set and apply the learnt transform to the validation and test sets using `transform`.
- Data should be cleaned to remove spurious or irrelevant data. Feature enrichment can be used to combine underlying features into more predictive data. 
- Data can be analysed to determine underlying correlations and their relative importance. Redundant or unimportant data can be removed to reduce training time and improve training performance.
- In general the more data the model has, the better its performance and the smaller the validation error. If possible, increase the amount of data for training. The time spent labelling additional data could be outweighed by savings in training time. However remember, an underfitting model will not see any benefits from additional data.
 
### Developing model prototypes
- Develop a series of basic models and experiment with different configurations. Draw upon existing model architectures and simple models and experiment to see which works best. 
- Keep the model simple, complexity should only be considered much later. If possible remove complexity in the model at this stage. Adding complexity without research cause is to be avoided as it will unnecessarily increase the training time and cause an explosion in the hyperparameter optimisation space.
- Fix the random number seeds with `np.random.seed()` and `tf.random.set_seed()`. This ensures reproducibility of the results.

- Initialise values with sensible parameters. If regressing against a value, set the initial bias close to that value. This cuts down training time by avoiding the model learning the underlying bias.
- Validate the model is setup correctly with sanity checks. If the losses make sense, the loss for the final softmax layer for a classification should be `-log(1/n_classes)`.
- Validate that the loss decreases with increased complexity, i.e. more layers, as one would expect.
- Validate inputs by visualising the input data right before it enters the model. Is it distributed as you expect?
- Visualise the model predictions on a fixed training batch. This help determine when the model is learning well and when it gets stuck. This can also help to set the learning rate.  
- Only when you are confident the model and data pipelines are setup correctly should you move onto further developing your model.


### Further developing the model
At this stage you should have a better understanding of the problem and have several simple models we can build upon and use as benchmarks to evaluate performance. 


- For developing a more complex models look at common architectures that are used in your problem space. Utilise the most simple of its class, do not implement an overly complex architecture. 
- Train the model beyond the point of overfitting to ensure the training loss can be reduced below your desired threshold. If the model cannot be overfit to such a degree it may indicate as issue with implementation that should be resolved before proceeding.

### Regularisation
- Now that you have confidence in the model implementation, you can look into regularisation to combat the overfitting and decrease the validation error.

- One of the most effective methods of improving a model, and to reduce overfitting, is to increase the size of the training set. A little work in getting more data can greatly increase training performance. Remember, a simple model with more data will often outperform a more advanced model with fewer data.
- Data can be further expanded using *data augmentation*, multiplying the size of your original dataset by performing transforms. For example, images can be rotated, flipped and scaled to create addition training  instances, enabling the model to better generalise. Experiment with a range of different transforms.   
- More advanced techniques can be used to create entirely new synthetic data from your existing training set. This includes purely Monte Carlo simulation, combining simulated with data and more advanced techniques such as *generative adversarial networks (GANs)* and *automated domain randomisation (ADR)*.
- Remove data features that may lead to false signals or does not contribute significantly to the predictive power.
- Decrease the model complexity. For example remove layers or change fully connected layers into pooling layers.
- Decreasing the batch size can increase variation in the batch fit, effectively adding additional regularisation.
- Dropout can be effective to ensure learnt behaviour generalises better. For CNNs SpatialDrop2D is more effective as it deals better with the correlation between pixels in an image. 
- Use early stopping to pick the optimal training epoch before overfitting occurs.
- Visualise the early layers of the network and check they have structure rather than being random.


- Manually inspect model errors, e.g. misclassifications, and analyse common trends in these examples. This error analysis can be used to determine which features of the data can be better utilised, through data enrichment, to reduce the chance of these errors occuring.

- Select the best performing model(s) for hyperparameter optimisation.


### Hyperparameter optimisation

With a better understanding of the data and models their hyperparamters can be explored.
Hyperparameter optimisation is an important and difficult aspect of model training. It does not matter if you spend lots of effort in developing the dataset and model as a poor selection of hyperparameters can cause a model to underperform. A highly-tuned simple model can outperform a more advanced model if it is poorly tuned.  

Models typically have multiple hyperparameters which each can possess a large range of values. This leads to a large parameter space, making finding an optimal selection difficult. 
- Hyperparameter optimisation is typically performed by a grid search or random search of the hyperparameter space. As the influence of individual hyperparamters are not equal, it is more efficient to use a random search rather than a grid search, with the latter more likely to sample more points of phase space that are not important to the model.

- Using a learning rate schedule can help to escape from local minima and inflection points. If using a scheduler, be careful that the learning rate decay is appropriate for the dataset and is not being reduced too much prematurely. Alternatively, use an optimiser such as Adam and RMSProp that adaptively scale with the gradient of the loss function.

### Additional performance enhancements

- Boosting can be used to combine an ensemble of weak learners into a more powerful model.
- Train the model for longer. Go beyond where the validation error appears to stop decreasing, the extra training could eventually push the model into a lower minima.

#### Dense network hyperparameters

- Number of layers:
    - Too low: Model may underfit (high bias).
    - Too high: Model may overfit (high variance), exploding/vanishing gradients.
- Number of nodes per layer:
    - Too low: Underfit
    - Too high: Overfit
- Activation function:
    - ReLU/LeakyReLU are popular as they are fast and reduce exploding gradients.
    - Sigmoid/Tanh only work for shallow networks (2 or fewer layers), have issue wih vanishing gradients for deeper networks.  
- Optimiser: 
    - SGD: Works well for shallow networks, can get stuck in local minima.
    - Adam and RMSProp work better and converge faster for more complex networks. Adam is generally more forgiving for poor hyperparamter configurations so may be safer when developing your model.

- Learning rate: 
    - Too high can diverge, too small can take too long to converge.
    - Recommended to try the powers of ten: 0.001, 0.01, 0.1, 1
- Weight initialisation scheme:
    - Use He-normal/Xavier-uniform for ReLU activation functions.
    - Use Xavier-glorot-normal/uniform for Sigmoid functions.
- Batch size:
    - Number of training examples passed through gradient descent before the weights are corrected.
    - The higher the batch size the better the convergence however the more memory training will take. 
    - It is recommended that the batch size is a power of 2 for optimal computations on the GPU.
- Number of epochs:
    - Number of times the training set passes through the training process.
    - Too few and the model may underfit, too many and it may overfit.
    - Anaylse the learning curves to determine the optimal stopping point.
- Dropout:
    - Regularisation parameter that randomly disables connections to nodes to avoid overfitting.
- l1/l2 regularisation:
    - Another regularisation parameter that penalises complexity.
    - Should be used if dropout is not fixing overfitting.