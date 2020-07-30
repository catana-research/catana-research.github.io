---
layout: post
title: SQL-Python
subtitle: Interfacing with SQL databases in Python
photo: shapes-and-column-on-black-surface-3779823.jpg
photo-alt: SQL
tags: [SQL]
category: [Systems]
---


## SQLAlchemy

SQLAlchemy is Object Relational Database (ORM) that can use multiple different backends, such as SQL server and Postgres to persist data. Unlike the table structure interface of an SQL database, an ORM API inputs and outputs data in the form of objects, reducing the overhead of unpacking tabular data into an Object Oriented format. 


### Example

The SQL mapping to our Python object is defined by creating a Python object that derives from the `declarative_base` object. The member variables of this object are then mapped to columns within the specified `__tablename__`

```python
from sqlalchemy import Column, Integer, String, Float
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class Room(Base):
    __tablename__ = 'room'
    
    id = Column(Integer, primary_key=True)
    
    code = Column(String(36), nullable=False)
    size = Column(Integer)
    price = Column(Integer)
    longitude = Column(Float)
    latitude = Column(Float)

```

Data can then be extracted and input by instances of this class.