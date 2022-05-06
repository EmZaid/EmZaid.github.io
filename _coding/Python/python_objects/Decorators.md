---
title: "Decorators"
permalink: /coding/python/python_objects/Decorators/
excerpt: ""
last_modified_at: 2022-05-05
sidebar:
    - title: "python"
      nav: python
published: false
---

### Decorators
Summary: Used to wrap functions in new functionality.

I don't totally get them yet.

Offical Documentation states:
```python
@property
def x(self):
    return self._x
```

is equivalent to

```python
def getx(self):
    return self._x
x = property(getx)
```

Basically it removes a few lines of typing. And you get to use the @ sign!






eg:

```python

# with @ symbol
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")

    return wrapper


@my_decorator
def say_whee():
    print("Whee!")


say_whee()

```
Basically it lets you dynamically move where your functions activate. 

There are a few built in decorators
```python

# Wrapped Classes: Circle Example


class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        """Get value of radius"""
        return self._radius

    @radius.setter
    def radius(self, value):
        """Set radius, raise error if negative"""
        if value >= 0:
            self._radius = value
        else:
            raise ValueError("Radius must be positive")

    @property
    def area(self):
        """Calculate area inside circle"""
        return self.pi() * self.radius**2

    def cylinder_volume(self, height):
        """Calculate volume of cylinder with circle as base"""
        return self.area * height

    @classmethod
    def unit_circle(cls):
        """Factory method creating a circle with radius 1"""
        return cls(1)

    @staticmethod
    def pi():
        """Value of π, could use math.pi instead though"""
        return 3.1415926535


c = Circle(5)
print(c.radius)

```


### References
- [realpython](https://realpython.com/primer-on-python-decorators/)
- [Python glossary](https://docs.python.org/3/glossary.html#term-decorator)
