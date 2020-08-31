---
layout: post
title: Pandas
subtitle: Improving execution of Panel data
photo: position-of-figures-3678799.jpg
photo-alt: Pandas
tags: [Optimisation, Pandas, Python]
category: [Systems]
---


Pandas is a popular library for handling and processing tabular data.

The core data types, the `Series` and `DataFrame`, are built upon the Numpy `ndarray`. These can be directly  accessed using the `.to_numpy()` operator, which replaces the `.values` method.


In the following we will explore optimisation of Pandas operations on the `listings` and `reviews` tables from the [Kaggle Berlin Airbnb Dataset](https://www.kaggle.com/brittabettendorf/berlin-airbnb-data#). 

### Dataset inspection 

Before optimising, it is good to understand the underlying data to indentify where slowdowns may arise.

Looking at the two DataFrames using the `.info()` operator we see they are both reasonably large.

The `listings` DataFrame is 2.8MB and has 22,552 entries and 16 columns of a mixture of datatypes.
```python
>>> listings.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 22552 entries, 0 to 22551
Data columns (total 16 columns):
id                                22552 non-null int64
name                              22493 non-null object
host_id                           22552 non-null int64
host_name                         22526 non-null object
neighbourhood_group               22552 non-null object
neighbourhood                     22552 non-null object
latitude                          22552 non-null float64
longitude                         22552 non-null float64
room_type                         22552 non-null object
price                             22552 non-null int64
minimum_nights                    22552 non-null int64
number_of_reviews                 22552 non-null int64
last_review                       18644 non-null object
reviews_per_month                 18638 non-null float64
calculated_host_listings_count    22552 non-null int64
availability_365                  22552 non-null int64
dtypes: float64(3), int64(7), object(6)
memory usage: 2.8+ MB
```

Reviews is even larger at 6.1MB, with 401,963 rows and 2 columns:
```python
>>> reviews.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 401963 entries, 0 to 401962
Data columns (total 2 columns):
listing_id    401963 non-null int64
date          401963 non-null object
dtypes: int64(1), object(1)
memory usage: 6.1+ MB
```


### Data lookups

A common task with Pandas objects is to search for a particular entry or set of entries. The API provides a simple way of performing this operation using the 



### Filter and memory



### Inplace


### Numba

If your code is spending a long time getting and setting Pandas objects it might be worth considering Numba.

- Never pass Pandas or Python datatypes to Numba or Cython optimised functions. Instead use pass an `np.ndarray` using the `.to_numpy()` operator (use of `.values` is being deprecated). 
- If you are unsure whether the objects you are passing to numba are in a no Python format you can specify `nopython=True` in the decorator. This ensures numba runs without Python objects and if there are any such objects they can be easily identified as they will throw an error during execution.
- Ensure all that all `numba` functions with `nopython=True` also call functions with `nopython=True`. A function called without `nopython=True` will cause the rest to run in Python mode, losing the performance gains.



In summary:
- Filter data as early as possible to avoid redundant calculations and reduce memory and IO usage. 
- If your code is still too slow, consider computing the `nd.array` in Numpy, Cython or Numba.

