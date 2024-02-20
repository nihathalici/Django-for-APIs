Todo API / 1. Set-Up
========================================================

* Make a new directory

```shell
mkdir todo
```

* Create a virtual environment within this new directory. 

```shell
python -m venv .venv
```

* Activate the virtual environment called .venv
```shell
source .venv/bin/activate
```

* Install Django
```shell
pip install django~=4.0.0
```

* Create a new Django project called django_project. 
```shell
django-admin startproject django_project .
```



* Create the app named todos. Sync the database.
```shell
python manage.py startapp todos
python manage.py migrate
```

* Update the settings
```python
# django_project/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Local
    "todos.apps.TodosConfig",
]
```

* Run the server and check http://127.0.0.1:8000/
```shell
python manage.py runserver
```

* Stop the server with Ctrl-C . Create the .gitignore file
```.gitignore
.venv/
```

* Set up the Git repo
```shell
git status
git add -A
git commit -m "initial commit"
```

* Write the model
```python
# todos/models.py
from django.db import models

class Todo(models.Model):
    title = models.CharField(max_length=200)
    body = models.TextField()

    def __str__(self):
        return self.title
```

* Refresh the database
```shell
python manage.py makemigrations todos
python manage.py migrate
```

* Update the admin
```python
# todos/admin.py

from django.contrib import admin

from .models import Todo

class TodoAdmin(admin.ModelAdmin):
    list_display = (
        "title",
        "body",
    )

admin.site.register(Todo, TodoAdmin)
```

* Create the superadmin
```shell
python manage.py createsuperuser
```

* Run server and log in http://127.0.0.1:8000/admin/
```shell
python manage.py
```

* Create new todo items

* Stop the server with Ctrl-C .