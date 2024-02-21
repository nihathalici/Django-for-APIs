Blog API / 3. Posts App
========================================================

* Create the app posts

```shell
python manage.py startapp posts
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
    "accounts.apps.AccountsConfig",
    "posts.apps.PostsConfig",
]
```

* Write the model
```python
# posts/models.py

from django.conf import settings
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=50)
    body = models.TextField()
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
```

* Refresh the database
```shell
python manage.py makemigrations posts
python manage.py migrate
```

* Update admin
```python
# posts/admin.py

from django.contrib import admin

from .models import Post

admin.site.register(Post)
```

* Run local server
```shell
python manage.py runserver
```

* Check http://127.0.0.1:8000/admin/
* Go to Posts section. Make a new blog post.
