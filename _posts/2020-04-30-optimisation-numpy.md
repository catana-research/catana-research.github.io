---
layout: post
title: Caching
subtitle: Memoization with Python
photo: photo-of-cave-rock-formation-2537642.jpg
photo-alt: Caching
tags: [Cache, Python]
category: [Optimisation]
---

Caching is useful when expensive calculations are repeated. Using a cache, the first time the calculation procedes as normal, however every subsequent calculation uses a cached version of the result, saving the computational effort. This can only be applied to functions where the same input produces the same output.

## Caching a function

Functools provides a simple method for the caching of results via the last recently used cache. This stores the most recently called arguments and the corresponding function outputs to enable a fast mapping. To enable this lookup to be applied, there is a restriction that the arguments are a hashable type.

Caching can be applied to existing code by simply adding the `lru_cache` decorator.  

```python
@functools.lru_cache(maxsize=1000)
def f(x):
    return x
```

## Clearing the cache 

If the underlying data of the cached function has changed you need to ensure the cache is wiped to avoid using the stale cache. You can use the `cache_clear` function in the setter function of the underlying data to ensure this happens automatically.

```python
f.cache_clear()
```