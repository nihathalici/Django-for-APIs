Blog API / 4. DjangoRESTFramework
========================================================

* Install djangorestframework

```shell
pip install djangorestframework~=3.13.0
```

* Update settings
```python
# django_project/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    #3rd-party apps
    "rest_framework"
    # Local
    "accounts.apps.AccountsConfig",
    "posts.apps.PostsConfig",
]

REST_FRAMEWORK = {
    "DEFAULT_PERMISSION": [
        "rest_framework.permissions.AllowAny",
    ],
}
```

* Edit the urls.py within the django_project folder
```python
# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("api/v1/", include("posts.urls")),
]
```

* Create urls.py file within the posts folder
```python
# posts/urls.py

from django.urls import path

from .views import PostList, PostDetail

urlpatterns = [
    path("<int:pk>/", PostDetail.as_view(), name="post_detail"),
    path("", PostList.as_view(), name="post_list"),
]
```

* Create the serializers.py within the posts folder
```python
# post/serializers.py

from rest_framework import serializers

from .models import Post

class PostSerializer(serializers.ModelSerializer):
    class Meta:
        fields = (
            "id",
            "author",
            "title",
            "body",
            "created_at",
        )
        model = Post
```

* Update the views
```python
# posts/views.py

from rest_framework import generics

from .models import Post
from .serializers import PostSerializer

class PostList(generics.ListCreateAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer

class PostDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
```

* Run the local server
```shell
python manage.py runserver
```

* Check http://127.0.0.1:8000/api/v1/