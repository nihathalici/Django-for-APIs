Library Website / 2. Books App
========================================================

* Create the app books

```shell
python manage.py startapp books
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
    "books.apps.BooksConfig",
]
```

* Write the model
```python
# books/models.py
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=250)
    subtitle = models.CharField(max_length=250)
    author = models.CharField(max_length=100)
    isbn = models.CharField(max_length=13)

    def __str__(self):
        return self.title
```

* Sync the database
```shell
python manage.py makemigrations books
python manage.py migrate
```

* Create the superuser
```shell
python manage.py createsuperuser
```

* Update admin.py
```python
# books/admin.py
from django.contrib import admin

from .models import Book

admin.site.register(Book)
```

* Start up the local server
```shell
python manage.py runserver
```

* Check http://127.0.0.1:8000/admin . Make new book entries.

* Write the view
```python
# books/views.py

from django.views.generic import ListView

from .models import Book

class BookListView(ListView):
    model = Book
    template_name = "book_list.html"
```

* Modify the urls.py within the django_project
```python
# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", include("books.urls")),
]
```

* Create a new urls.py file within the books folder
```python
# books/urls.py

from django.urls import path

from .views import BookListView

urlpatterns = [
    path("", BookListView.as_view(), name="home"),
]
```

* Create a new directory named templates within the books folder. Within the templates folder create another directory named books
```shell
mkdir books/templates
mkdir books/templates/books
```

* Create the template book_list.html within books/templates/books/ folder.
```html
<!-- books/templates/books/book_list.html -->
<h1>All books</h1>

{% for book in book_list %}
<ul>
    <li>Title: {{ book.title }}</li>
    <li>Subtitle: {{ book.subtitle }}</li>
    <li>ISBN: {{ book.isbn }}</li>
</ul>    
{% endfor %}
```

* Start up the local server
```shell
python manage.py runserver
```

* Check http://127.0.0.1:8000/
