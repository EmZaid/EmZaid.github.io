---
programs: []
date: 2022-04-19

---
### Django Migrations
Tags: 
Related to: 
See also: 
Previous:

## Migrations

They are basically a version control system.

They are Djano's way of propagating changes. 

Think of them as managing databases. You make changes to the [[Models]] and then those changes are migrated to the database. (eg. adding a column or row)

Migrations are very powerful and let you change your [[Models]] over time, as you develop your project, without the need to delete your database or tables and make new ones - it specializes in upgrading your database live, without losing data. We’ll cover them in more depth in a later part of the tutorial, but for now, remember the three-step guide to making model changes:

-   Change your [[Models]] (in `models.py`).
-   Run [`python manage.py makemigrations`](https://docs.djangoproject.com/en/4.0/ref/django-admin/#django-admin-makemigrations) to create migrations for those changes
-   Run [`python manage.py migrate`](https://docs.djangoproject.com/en/4.0/ref/django-admin/#django-admin-migrate) to apply those changes to the database.
### References
- [[]]