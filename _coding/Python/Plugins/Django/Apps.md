---
programs: []
date: 2022-04-19

---
### Title
Tags: 
Related to: [[Web Development]], [[Models]]
See also: 
Previous:

### Apps

#### Installing

To create a new app run:

```bash
python manage.py startapp <app-name>
```

`startapp` creates the framework for a new app.

### startapp

`django-admin startapp name [directory]`[¶](https://docs.djangoproject.com/en/4.0/ref/django-admin/#django-admin-startapp "Permalink to this definition")

Creates a Django app directory structure for the given app name in the current directory or the given destination.

By default, [the new directory](https://github.com/django/django/blob/main/django/conf/app_template) contains a `models.py` file and other app template files. If only the app name is given, the app directory will be created in the current working directory.

If the optional destination is provided, Django will use that existing directory rather than creating a new one. You can use ‘.’ to denote the current working directory.

For example:
```bash
django-admin startapp myapp /Users/jezdez/Code/myapp
```

### References
- [Oficial documentation](https://docs.djangoproject.com/en/4.0/ref/django-admin/)