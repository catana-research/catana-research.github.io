---
layout: post
title: Test-Driven Development
subtitle: 
photo: stones-wall-93417.jpg
photo-alt: Clustering data
tags: [Testing, Python]
category: [Systems]
---

{% comment %}Introduction{% endcomment %}
When developing a new feature the process of writing tests is often a secondary consideration to writing code. For Test-Driven development (TDD) the emphasis is inverted, with tests being written before any code. This is achieved this by framing the problem by a set goals that need to be achieved before the feature is deemed complete. Although this may seem restrictive, these goals are can be adjusted as development progresses and more information about the problem is learnt. 

{% comment %}Motivation{% endcomment %}
Done correctly, TDD ensures that there is always a comprehensive and up-to-date suite of tests that covers all functionality. This greatly reduces the chance of bugs creeping and enables safer refactoring and optimisiation of code. 

As TDD is primarily focused on unit testing, it is useful for ensuring an API works but not necessarily ensuring the end-to-end execution. These cases are better covered by integration and functional testing. 


## Dummy example

To see how this works in practice, let's consider adding new functionality to a Mathematical framework.

We have been asked to create a new function called `sqrt_above_ten`, which returns the sqrt of an input provided it is greater than ten. Before writing any code we can already define a series of tests to ensure it is correctly implemented, these should follow all the expected code paths and outcomes in addition to corner cases. An example in this case could be:

- Value above ten is square rooted: `sqrt_above_ten(400) = 20` 
- Value equal to ten is returned unchanged: `sqrt_above_ten(10) = 10`
- Value below ten is returned unchanged: `sqrt_above_ten(9) = 9`
- Negative values do not throw errors: `sqrt_above_ten(-1) = -1` 

Before writing any code, we can define the unit tests with pytest:
```python
# tests/analysis/test_analysis.py
from analysis.analysis import sqrt_above_ten

def test_sqrt_above_ten_large_number():
    res = sqrt_above_ten(400)
    assert res == 20

def test_sqrt_above_ten_equal_ten():
    res = sqrt_above_ten(10)
    assert res == 10

def test_sqrt_above_ten_below_ten():
    res = sqrt_above_ten(9)
    assert res == 9

def test_sqrt_above_ten_negative_number():
    res = sqrt_above_ten(-1)
    assert res == -1
```

Executing this test will result in a failure as this code does not exist: 
```python
Fail
```


In TTD, the aim should be to use the minimum amount of code to pass the tests. This is often counter to a developer's desire to provide the most elegant and efficient solution possible however this can be achieved later during refactoring, a changing of the underlying code without changing its behaviour.

## Red, Green, Refactor

Initially we created a series of unittests that failed, which we fixed by adding the most simple solution to resolve. With these tests inplace, we can now be confident in improving the code via refactoring. This is referred to as 'Red, Green, Refactor', an iterative process where tests are added for new functionality that fail (red), changes are made to add the functionality and the tests pass (green) and finally a refactoring takes place. As in TDD we have a comprehensive test suite in place, we can be confident that the refactoring is not going to introduce any bugs or unintended changes to behaviour.  Refactoring should be avoided if their isn't sufficient testing for the code being changed. 

Any new feature or bug fix requests should be handled in the same manner. Determine first the requirements, encode them as tests and develop until the tests pass, refactoring if required. Being able to produce a failing test demonstrates you understand the issue at hand and can replicate the failure of the code. Provided the existing test coverage is comprehensive, this ensures a bug fix does not introduce further bugs.

## Summary:

1. Test first, code later
2. Add the bare minimum amount of code you need to pass the tests
3. You shouldn’t have more than one failing test at a time
4. Write code that passes the test. Then refactor it.
5. A test should fail the first time you run it. If it doesn’t ask yourself why you are adding it.
6. Never refactor without tests.

*References:* **Clean Architectures in Python - Ch 1**

## Automated testing

### Unit tests

Key design principles:
- Tests should run fast, this enables quick iteration of executing tests to check a problem is resolved.
- Tests should leave the system unchanged and not be dependent on the order of execution (idempotent).
- Tests of specific functionality should be independent of the rest of the system (isolated). For example the processing of output from a database should not be dependent on the database extraction code, this is covered by integration tests. This can be isolated by `mocking`.
- Should test the smallest possible chunk of logic possible. If a test hits several potential points of failure, it may be difficult to diagnose what caused the issue. Similarly if several tests are exposed to the same points of failure.



### Testing cases


Test cases can be categorised by whether they are a result of data flow in, internally or out: 
- Incoming (outside, self) should be tested. This includes calls to methods that extract data and modify the component.
- Private (self, self) can be tested. This includes internal logic that is not exposed externally. Arguably this should not be tested as it locks a particular implementation, making refactoring difficult however if this is acceptable and more robust testing is required this can be done.
- Outgoing (self, outside) should be mocked. 


Cases can be further split into two distinct instances:
- `Queries` extract information without changing the state of the component.
- `Commands` change the state without extracting information.

### Smoke tests
- Slower running

### Integration tests


*References:* **Clean Architectures in Python - Ch 2**


## Mocking

Useful when testing features that require the input from an external system.


```python
from unittest import mock
m = mock.Mock()

# Mocking a method
>>> m.my_method.return_value = 42
>>> m.my_method()
42
```

### side_effects

Accepts three different flavours of objects, callables, iterables, and exceptions.

Iterable:
```python
>>> m.some_attribute.side_effect = range(3)
>>> m.some_attribute()
0
>>> m.some_attribute()
1
```

Callable passes arguments to the function:
```python
>>> def print_number(num):
... print("Number:", num)
...
>>> m.some_attribute.side_effect = print_number
>>> m.some_attribute(5)
Number: 5
```


### Patching

Override the behaviour of external code. Below we patch `abspath` to a mock where we reassign the return_value. You can add more patched objects by adding more arguments nad patch decorators. Note that the decorators are evaluated from the bottom to the top of the decorator list.   
```python
from unittest.mock import patch

@patch('os.path.getsize')
@patch('os.path.abspath')
def test_get_info(abspath_mock, getsize_mock):
    filename = 'somefile.ext'
    original_path = '../{}'.format(filename)

    test_abspath = 'some/abs/path'
    abspath_mock.return_value = test_abspath

    test_size = 1234
    getsize_mock.return_value = test_size

    fi = FileInfo(original_path)
    assert fi.get_info() == (filename, original_path, test_abspath, test_size)
```

Note that some objects are immutable and cannot be patched. This can be circumvented by patching mutable copy of the object in the module that imports that object. For instance datetime calls in the module `fileinfo.logger` can be patched by:

```$xslt
from unittest.mock import patch

# @patch('datetime.datetime.now')  # This won't work
@patch('fileinfo.logger.datetime.datetime')
def test_log(mock_datetime):
    test_now = 123
    test_message = "A test message"
    mock_datetime.now.return_value = test_now
    test_logger = Logger()
    test_logger.log(test_message)
    assert test_logger.messages == [(test_now, test_message)]

``` 


*References:* **Clean Architectures in Python - Ch 3**


## Pytest

### Fixtures

https://docs.pytest.org/en/latest/fixture.html

Fixtures provide a fixed baseline so that tests execute reliably and produce consistent, repeatable, results.

Common functions that are defined in `conftest.py` will be automatically loaded when running pytest.



#### Examples

A standard fixture lets a predefined input be defined. In the example below, `response_value` is a function that returns a dictionary that can be used by any test that includes it as an argument. 

```
@pytest.fixture
def response_value():
    return {'key': ['value1', 'value2']}

def test_response_success_is_true(response_value):
    assert bool(res.ResponseSuccess(response_value)) is True
```


Parameterize enables a single test to be executed multiple times with differing test arguments:

```
@pytest.mark.parametrize(
'key',
['code__eq', 'price__eq', 'price__lt', 'price__gt']
)
def test_build_room_list_request_object_accepted_filters(key):
filters = {key: 1}
request = req.RoomListRequestObject.from_dict({'filters': filters})
assert request.filters == filters
assert bool(request) is True
```


### Marking

```python
@pytest.mark.complex
@pytest.mark.slow
def test_addition():
    [...]
```
Can selectively run marked tests:
```
pytest -svv -m slow
```

#### Integration tests

Add the following to a module to mark all contained tests as integration tests: 
```python
pytestmark = pytest.mark.integration
```
Run selectively with:
```cmd
py.test -svv -m integration
```

## Extensions

pytest-flask