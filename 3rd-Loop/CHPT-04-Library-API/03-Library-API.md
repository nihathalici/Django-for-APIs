Library Website / 2. Books App
========================================================

* Install Django REST Framework

```shell
python -m pip install djangorestframework~=3.13.0
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
    # 3rd party
    "rest_framework",
    # Local
    "books.apps.BooksConfig",
]
```

* Create the app named apis
```shell
python manage.py startapp apis
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
    # 3rd party
    "rest_framework",
    # Local
    "books.apps.BooksConfig",
    "apis.apps.ApisConfig",
]
```

* Modify the urls.py file within the django_project folder
```python
# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("api/", include("apis.urls")),
    path("", include("books.urls")),
]
```

* Create the urls.py within the apis folder
```python
# apis/urls.py

from django.urls import path

from .views import BookAPIView

urlpatterns = [
    path("", BookAPIView.as_view(), name="book_list"),
]
```

* Write the view
```python
from rest_framework import generics

from books.models import Book
from .serializers import BookSerializer

class BookAPIView(generics.ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

* Create the serializers.py file within the apis folder
```python
# apis/serializers.py

from rest_framework import serializers

from books.models import Book

class BookSerializers(serializers.ModelSerializer):
        class Meta:
               model = Book
               fields = ("title", "subtitle", "author", "isbn")

```

* Run server
```shell
python manage.py runserver
```

* Check http://127.0.0.1:8000/api/