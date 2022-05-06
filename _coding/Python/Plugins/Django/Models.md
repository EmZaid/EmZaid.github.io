---
programs: []
date: 2022-04-19

---
### Title
Tags: 
Related to: [[Web Development]], [[Django Basic notes]]
See also: 
Previous:



# Models

A model is the single, definitive source of information about your data. It contains the essential fields and behaviors of the data you’re storing. Generally, each model maps to a single database table.

The basics:

-   Each model is a Python class that subclasses [`django.db.models.Model`](https://docs.djangoproject.com/en/4.0/ref/models/instances/#django.db.models.Model "django.db.models.Model").
-   Each attribute of the model represents a database field.
-   With all of this, Django gives you an automatically-generated database-access API; see [Making queries](https://docs.djangoproject.com/en/4.0/topics/db/queries/).


## Examples

```python
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```

# Using Models
Once you have defined your models, you need to tell Django you’re going to _use_ those models. Do this by editing your settings file and changing the [`INSTALLED_APPS`](https://docs.djangoproject.com/en/4.0/ref/settings/#std:setting-INSTALLED_APPS) setting to add the name of the module that contains your `models.py`.

For example, if the models for your application live in the module `myapp.models` (the package structure that is created for an application by the [`manage.py startapp`](https://docs.djangoproject.com/en/4.0/ref/django-admin/#django-admin-startapp) script), [`INSTALLED_APPS`](https://docs.djangoproject.com/en/4.0/ref/settings/#std:setting-INSTALLED_APPS) should read, in part:
```python
INSTALLED_APPS = [
    #...
    'myapp',
    #...
]
```


### References
- [[]]