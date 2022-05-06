---
title: "Name Spaces"
permalink: /coding/python/python_objects/name_spaces/
excerpt: ""
last_modified_at: 2022-05-05
sidebar:
    - title: "python"
      nav: python
published: false
---

### Name Spaces

Summary:

A class supports two kinds of operations: attribute references and instantiation

Instantiation objects in turn only understand attribute references.

But there are two kinds of valid attribute names: data attributes and methods.

A method is a function that “belongs to” an object.





# official documentation
>
A _scope_ is a textual region of a Python program where a namespace is directly accessible. “Directly accessible” here means that an unqualified reference to a name attempts to find the name in the namespace.
Although scopes are determined statically, they are used dynamically. At any time during execution, there are 3 or 4 nested scopes whose namespaces are directly accessible:
-   the innermost scope, which is searched first, contains the local names
-   the scopes of any enclosing functions, which are searched starting with the nearest enclosing scope, contains non-local, but also non-global names
-   the next-to-last scope contains the current module’s global names
-   the outermost scope (searched last) is the namespace containing built-in names




A simple example from documentation:

```python
def scope_test():
    def do_local():
        spam = "local spam"

    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)
```


# official documentation
>
When a class definition is entered, a new namespace is created, and used as the local scope — thus, all assignments to local variables go into this new namespace. 
>
In particular class objects support two kinds of operations: attribute references and instantiation.


Attributes are anything after a `.`

```python
import string
string.ascii_lowercase
```
the `.ascii_lowercase` is the attribute. 


## A simple Example

```python
class MyClass:
    """A simple example class"""

    i = 12345

    def f(self):
        return "hello world"


x = MyClass()

xf = x.f
while True:
    print(xf())
```

This function prints out hello world forever.
MyClass.i and MyClass.f are both valid attribute references

The line `x = MyClass()` makes a new instance of the class and assigns `x` to it. 


### References

