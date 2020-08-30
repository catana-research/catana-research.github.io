---
layout: post
title: Pandas
subtitle: Improving execution of Panel data
photo: position-of-figures-3678799.jpg
photo-alt: Pandas
tags: [Optimisation, Pandas, Python]
category: [Systems]
---

### Filter and memory


### Inplace


### Numba

If your code is spending a long time getting and setting Pandas objects it might be worth considering Numba.


In summary:
- Filter data as early as possible to avoid redundant calculations and reduce memory and IO usage. 
- Never pass Pandas or Python datatypes to Numba or Cython optimised functions. Instead use pass an `np.ndarray` using the `.to_numpy()` operator (use of `.values` is being deprecated). 
- If you are unsure whether the objects you are passing to numba are in a no Python format you can specify `nopython=True` in the decorator. This ensures numba runs without Python objects and if there are any such objects they can be easily identified as they will throw an error during execution.
- Ensure all that all `numba` functions with `nopython=True` also call functions with `nopython=True`. A function called without `nopython=True` will cause the rest to run in Python mode, losing the performance gains.


