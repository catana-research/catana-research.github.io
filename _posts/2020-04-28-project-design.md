---
layout: post
title: Project design
subtitle: Numerical computing in Python
photo: photo-of-snake-3281127.jpg
photo-alt: SQL
tags: [SQL, Python]
category: [Systems]
---





## Conventions

- Use a linter and follow code conventions, for Python this is: [PEP8](https://www.python.org/dev/peps/pep-0008/).  



## Software Development Life cycle


## Environments

A great feature of Python is the rich ecosystem of libraries that can be readily installed using _pip_ or _conda_. A downside to this however is keeping a configuration of library versions that are compatible with one another. The resolution to this problem is to create a new virtual environment for each project. This is a self-contained system that enables a bespoke installation of packages for each project you develop, ensuring there is no pollution from packages installed in other projects.  

A list of required packages are stored in the project root in a `requirements.txt` file.

## Documentation

The majority of time spent in development lifecycle is the maintenance of existing code. Having comprehensive and up-to-date documentation is critical in ensuring this is easy as possible.

- All code should have descriptive docstrings.
- Extra documentation in the form of RST files can provide extra information. 
- Sphinx can be used to render the documentation into searchable HTML and PDF formats.
- Numpy/Google format.

## Automated testing

- Unittest, pytest, nose.
- All functionality developed needs to be unit tested, if a test does not exist.
- You should not need to test code from external libraries. 
- Unit tests and integration tests.


## Continuous integration

A core part of a agile development process is the use of continuous integration systems to   

- Jenkins, Travis CI

## Structure

The structure for your project _myproject_ will be as follows:

- _myproject/
- tests/
- docs/
- requirements.txt

## Cookiecutter

Cookiecutter removes the effort of setting up all the boilerplate code to setup and effective project. Pick your favourate template, answer the prompts and a fully working project is ready to go. There is a wide ecosystem of templates to choose from which can be customised to fit your needs.


## Appendix

Don't reinvent the wheel. Many problems have already been solved using efficient and well-tested implementations. If something already exists import the code and save effort in code maintenance.


