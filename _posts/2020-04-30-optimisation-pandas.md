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

Before optimising, it is good to understand the underlying data to identify where slowdowns may arise.

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


### Datatypes

Sometimes performance can be improved by changing the underlying datatype being stored.


#### Categorical data

If your data contains commonly repeated data in categories, it may be worth storing the data as [categorical](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Categorical.html). In which case Pandas stores data as one of a fixed number of categories, greatly reducing storage requirements and computionations.

Consider a portfolio that contains one million trades of a range of credit ratings: 
    ```
    ratings = pd.DataFrame(data={"Data": np.random.choice(["AAA", "AA", "A", "BBB", "BB", "B", "C", "D"], 1000000)})
    ```

If we want to extract only trades that are in the default state:
```
    # 60.407 ms
    with Timer():
        ratings[ratings["Data"] == "D"]
```

We can make this a bit faster using numpy:
```
    #  25.969 ms
    with Timer():
        ratings[ratings["Data"].to_numpy() == "D"]
```

It's easy to convert existing data to categories with the `astype` method:
```
    ratings_cat = ratings.copy()
    ratings_cat["Data"] = ratings["Data"].astype("category")
```
The categories can be accessed by:
```python
>>> ratings_cat["Data"].cat.categories
Index(['A', 'AA', 'AAA', 'B', 'BB', 'BBB', 'C', 'D'], dtype='object')
```
The size of the DataFrame is now 7.8x smaller (977.1 KB vs 7.6 MB) and doing the selection with Pandas is now much faster:
```
# 6.793 ms
ratings_cat[ratings_cat["Data"] == 'D']
```

Be careful however as a naive cast to numpy actually slower. This is because the data is being cast back to a string type: 
```
# 35.147 ms
ratings_cat[ratings_cat["Data"].to_numpy() == 'D']
>> ratings_cat["Data"].to_numpy()
array(['BB', 'AAA', 'BBB', ..., 'D', 'AAA', 'D'], dtype=object)
```

Using the category codes we get the same performance as the native Pandas categorical selection:
```
# 6.493 ms
ratings_cat[ratings_cat["Data"].cat.codes.to_numpy() == ratings_cat["Data"].cat.categories.get_loc('D')]
``` 
*Summary:* The Categorical datatype is useful if you have data that conforms to common categories, not only improving computational performance, but also reducing memory utilisation. Be careful to check that your optimisation performs as you expect and is not performing an intermediate transformation.



### Data lookups

A common task with Pandas objects is to search for a particular entry or set of entries. The API provides a simple way of performing this operation using the `__getitem__` operator `[]`, for example a common method to determine the price for the listing with `id=3309` would be:
```python
listings[listings['id'] == 3309]['price'] 
```
Timing this operation we find this takes a little over a millisecond:
```python
1.56 ms ± 23.6 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
```
This might seem small but in a loop where there are multiple lookups this can quickly add up. We can improve performance by setting the index to the `id` column and using the `loc` operator:
```python
listings.loc[3]['price']
```
Which is 6x faster:
```python
253 µs ± 30.9 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
```
This however is still has an inefficiency as it includes a redundant intermediate step where the data is returned as a DataFrame object after selection. To avoid this you should always include the column name(s) during the operation to ensure this is handled internally:
```python
listings.loc[3, 'price']
```
Which is over 171x faster than the original solution:
```python
9.1 µs ± 310 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```
The `loc` operator is useful as it can return single or multiple values for a selection. If however you only need a single return value (a scalar) the `at` operator can be used to further speed up the operation:
```python
listings.at[3, 'price']
```
Which is 290x faster than the original solution:
```python
5.38 µs ± 70.3 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```
In summary, when accessing data where possible filter on the index using `loc` or `at` and ensure that the returned columns are specified within the call.

### Multiple data lookups

Although it may appear that the singular lookup operation of ~5µs is pretty quick, when this operation is occuring inside a loop with a large number of iterations this can quickly become a problem. For instance looping over 10 million times (easy to do for big datasets or multiple dimensions) this will take ~50 seconds.

Another way at approaching the lookup problem is to instead rephrase it as a `join` operation, where we more efficiently combine two distinct datasets. Afterwhich we can perform more efficient vectorised operations. This scales much better than the linear scaling of the original singular lookup approach and is easier to read. 

For instance, consider looping through two DataFrames that contain the `Width` and `Length` of a set of shapes and trying to determine their area (we assume the indices of these DataFrames are already aligned). 

Let's generate a test dataset:
```pandas
import numpy as np
import pandas as pd

n_shapes = 100000
widths = pd.DataFrame(data={"Width": np.random.uniform(0, 1, n_shapes)})
lengths = pd.DataFrame(data={"Length": np.random.uniform(0, 1, n_shapes)})
```

Our naive singular lookup approach in this case takes 0.6s:
```python
areas = np.full(n_shapes, None)

for i, width in enumerate(widths.to_numpy()):
    areas[i] = widths.at[i, "Width"]
```

We can more efficiently performing the lookup of a single `join` operation of the two tables, followed by a vectorised operation. In this case the execution is ~243x faster and far more readable:
```python
shapes = widths.join(lengths)
areas = shapes["Width"].to_numpy() * shapes["Length"].to_numpy()
```

### Looping over data


Avoid `iterrows`, it's almost always a terrible solution. 

`vectorized` operations will normally provide the best performance.

If you have to apply a bespoke function `apply` can be fast.

Alternatively it might be worth reevaluating the problem. For example if the execution can be recast into a problem of joining to tables together `merge` can be extremely efficient.

### Inplace


### Processing large datasets

https://pandas.pydata.org/pandas-docs/stable/user_guide/scale.html

When using large datasets your may experience a decrease in performance and increase in memory.


listings.info(memory_usage='deep')

Reduce the data that is loaded and filter early.

Formats like `parquet` enable you to selectively load which data columns you need, `csv` will always load all the data.

Use space efficient dataformats. Pandas will use the largest dataformat available for each column. `csv` does not store the datatype so Pandas will assign a type that is compatible. `parquet` enables you to specify the datatype reducing space and memory utilisation.  

Split data from a monolithic block into smaller chunks. This enables IO and CPU utilisation to be balanced and makes it easier to use multithreaded processes, such as Dask.


### Dask

Dask is a framework that enables distributed computing.

Dask dataframe provides the same API for the majority of Pandas functionality however in a *lazy* way.

```python
import dask.dataframe as dd
ddf = dd.read_parquet("data/timeseries/ts*.parquet", engine="pyarrow")
ddf
```

Dask has some additional overhead than Pandas, so if the data is small enough to fit in memory its often better to use Pandas.  


### Numba

If an analysis of a profiling of your code shows it is spending a long time getting and setting Pandas objects it might be worth considering Numba. 



- Never pass Pandas or Python datatypes to Numba or Cython optimised functions. Instead use pass an `np.ndarray` using the `.to_numpy()` operator (use of `.values` is being deprecated). 
- If you are unsure whether the objects you are passing to numba are in a no Python format you can specify `nopython=True` in the decorator. This ensures numba runs without Python objects and if there are any such objects they can be easily identified as they will throw an error during execution.
- Ensure all that all `numba` functions with `nopython=True` also call functions with `nopython=True`. A function called without `nopython=True` will cause the rest to run in Python mode, losing the performance gains.



In summary:
- Filter data as early as possible to avoid redundant calculations and reduce memory and IO usage. 
- If your code is still too slow, consider computing the `nd.array` in Numpy, Cython or Numba.

