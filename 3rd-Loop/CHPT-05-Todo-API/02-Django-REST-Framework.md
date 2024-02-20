Todo API / 2. Django REST Framework
========================================================

* Install Django REST Framework

```shell
python -m pip install djangorestframework~=3.13.0
```

* Update the settings
```python
# django_project/settings.py

# Application definition

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
    "todos.apps.TodosConfig",
]

REST_FRAMEWORK = {
    "DEFAULT_PERMISSION_CLASSES": [
        "rest_framework.permissions.AllowAny",
    ],
}

```

* Modify the urls.py within the django_project folder
```python
# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("api/", include("todos.urls")),
]
```

* Create another urls.py file within the todos directory
```python
# todos/urls.py

from django.urls import path

from .views import ListTodo, DetailTodo

urlpatterns = [
    path("<int:pk>/", DetailTodo.as_view(), name="todo_detail"),
    path("", ListTodo.as_view(), name="todo_list"),
]
```

* Create serializers.py file within the todos directory
```python
# todos/serializers.py

from rest_framework import serializers

from .models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = (
            "id",
            "title",
            "body",
        )
```

* Update the views
```python
# todos/views.py

from rest_framework import generics

from .models import Todo
from .serializers import TodoSerializer

class ListTodo(generics.ListAPIView):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer

class DetailTodo(generics.RetrieveAPIView):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer
```

* Make new entries using admin. 

* Check the list view http://127.0.0.1:8000/api/

* Check the detail view http://127.0.0.1:8000/api/1/