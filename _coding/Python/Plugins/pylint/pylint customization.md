

# pylint customization
Tags: 
Related to: [[pylint]]
See also: 
Previous:

disable annoying pylint errors that are not useful. For example when building very simple programs for learning adding a docstring isn't really needed. It is good practice if I am going to release something, but otherwise isn't really needed.
```python
[MASTER]
disable=
    C0114, # missing-module-docstring
```

edit this to use shorter variable names
```pylint
# Good variable names which should always be accepted, separated by a comma.
good-names=i,
           j,
           k,
           ex,
           x,
           Run,
           _


```