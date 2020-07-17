---
layout: post
title: Architecture
subtitle: Python software design
photo: architectural-architecture-building-city-273750.jpg
photo-alt: Clustering data
tags: [Design, Python]
category: [Systems]
---

- Use layers categorised by function
- Clean design is a spherical architecture lower layers being encompassed and hidden by higher layers
- The deeper the layer, the more abstract the concepts

- Communication between elements in the same layer is unrestricted, communication outward in layers must be through an interface.
  

## Layers

Main layers: 
- Entities: Base classes and methods (innermost layer), know of each other and can interact. Do not know of higher level objects. 
- Use cases: Proceses that happen in the application, where domain models work on data. E.g. user logging on. Should be as small as possible. Use cases know the enitites and can instantiate and directly use them.
- External systems: Implements interfaces in the use case layer.

API is a fixed collection of entry points (methods or objects).
It is important to be strict about dataflow unless there is a good reason, such as performance. For example if an interface to the storage layer is slow, then directly using the database interface may be preferrable. This however has the issue that any changes to the database or API will break the outer layers. 
If data flow is broken consistently, it might be work combining two layers to a single layer of abstraction.



## Example pg 85

- Domain model:
- Use cases: Contains the logic that interacts with the higher layers.
- Repository: Data storage abstraction that can persist data and can return domain models. This can be an in-memory container, or persistent storage such as a file on a file system or a database. 
- Serializer: Returns the model as a result of a web API call. Typically uses a JSON format for HTML.

A clean API will perform validation of data input and provide diagnostic errors.

### Web API
- HTTP API: Typically returns data as JSON.
- Endpoint: Functions that are run when a user sends a request to a certain URL.
- Python web applications have a common interface known as the Web Serve Gateway Interface (WSGI). This is typically used to initialise the web interface.


*References:* **Clean Architectures in Python - Ch 2**



## Error management

- Error management should be robust.
- Main process performs creation and execution of usecases and is therefore the main cause of errors.

- Error management can be divided in requests, input data for the use cases, and responses, the use case output data.
- Request objects are created from incoming API calls. They need to validate incorrect or missing values and formats.  


*References:* **Clean Architectures in Python - Ch 3**


## Framework overview

- domain
- repository
- request_objects
    - Helper classes:
        - ValidRequest(object)
        - InvalidRequest(object)
    - Requests object for each use_case:
        - Derived from ValidRequest object
        - Implements logic for checks for that use_case
        - Takes repo in constructor
        - execute:
            - Applies request, if successful, returns object from repo with selections applied. Failures return the InvalidRequest object containing all the errors. 
        
- response_objects
    - Helper classes:
        - ResponseSuccess(object)
        - ResponseFailure(object)
            - Errors: Resource, parameters, system
            - Can build different error types and format an input str or exception into an error.
        - Both have class member variables for predefined success/errors.
        
- rest (Web)
- serializers
- use_cases:
    - init()
        input data from repo
    - execute()
        input: request from API
        internal: get data from repo using request
        output: response(data)






## Mastering object oriented Python






## Appendix

### DataStore

Pattern for a storage interface that is agnostic to the underlying repository.


### Mixin 

A class that contains methods that can be included in other classes without inheritence. Mixin classes are not designed to be standalone. Mixins are evaulated in reverse order.

```
class Mixin1(object):
    def test(self):
        print "Mixin1"

class Mixin2(object):
    def test(self):
        print "Mixin2"

class MyClass(BaseClass, Mixin1, Mixin2):
    pass

obj = MyClass()
>>> obj.test()
Mixin2
```

### Python project setup
- Create a venv for the project and link to it:
```cmd
mkvirtualenv PROJECT_NAME
setprojectdir . 
```
- If starting a new project use cookie cutter:
```pip install cookiecutter
    cookiecutter https://github.com/catana-research/cookiecutter-pypackage  # Pick an appropriate template
``` 
- Otherwise checkout from GitHub.
- Install the requirements:
```pip install -r requirements/dev.txt```