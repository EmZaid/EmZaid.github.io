```dataview

```



# Intro

Hashing is a one way process. It allows for simple checking of file diferences. It compares the hash of a file to the known hash. If there are differences the hash should differ. It is also often used to store passwords. 

Hashes can be cracked using things like hashcat

# Hashing functions

There are a ton. 

## md5
Very common. 

## argon2
Probably better?
This code is the boilerplate from [here](https://argon2-cffi.readthedocs.io/en/stable/api.html)

```python
from argon2 import PasswordHasher

ph = PasswordHasher()

hash = ph.hash("correct horse battery staple")

hash  
'$argon2id$v=19$m=65536,t=3,p=4$MIIRqgvgQbgj220jfp0MPA$YfwJSVjtjSU0zzV/P3S9nnQ/USre2wvJMjfCIjrTQbg'

ph.verify(hash, "correct horse battery staple")
True

ph.check_needs_rehash(hash)
False

ph.verify(hash, "Tr0ub4dor&3")
Traceback (most recent call last):
  ...
argon2.exceptions.VerifyMismatchError: The password does not match the supplied hash
```

