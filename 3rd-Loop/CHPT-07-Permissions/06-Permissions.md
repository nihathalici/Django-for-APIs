Blog API / 6. Permissions
========================================================


* Update settings
```python
# django_project/settings.py

REST_FRAMEWORK = {
    "DEFAULT_PERMISSION": [
        "rest_framework.permissions.IsAuthenticated",
    ],
}
```

* Run local server
```shell
python manage.py runserver
```
* Go to http://127.0.0.1:8000/admin/ Create a new user.


Blog API / Add Log In and Log Out
========================================================

* Update urls.py file within the django_project folder
```python
# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("api/v1/", include("posts.urls")),
    path("api-auth/", include("rest_framework.urls")),
]
```

* Check  http://127.0.0.1:8000/api/v1/ Login with the new test account.

Blog API / View-Level Permissions
========================================================

* Update the views within the posts folder
```python
# posts/views.py

from rest_framework import generics, permissions

from .models import Post
from .serializers import PostSerializer

class PostList(generics.ListCreateAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer

class PostDetail(generics.RetrieveUpdateDestroyAPIView):
    permission_classes = (permissions.IsAdminUser, )
    queryset = Post.objects.all()
    serializer_class = PostSerializer
```

* Run local server
```shell
python manage.py runserver
```

* Check  http://127.0.0.1:8000/api/v1/1/ Try to login with the test account.

Blog API / Custom Permissions
========================================================

* Create the permissions.py file within the posts folder

```python
# posts/permissions.py

from rest_framework import permissions

class IsAuthorOrReadOnly(permissions.BasePermission):
    def has_permission(self, request, view):
        # Authenticated users only can see list view
        if request.user.is_authenticated:
            return True
        return False
    
    def has_object_permission(self, request, view, obj):
        # Read permissions are allowed to any request
        # Always allow GET, HEAD, or OPTIONS requests
        if request.method in permissions.SAFE_METHODS:
            return True
        
        # Write only allowed to the author of a post 
        return obj.author == request.user        
```

* Update the views

```python
# posts/views.py

from rest_framework import generics, permissions

from .models import Post
from .permissions import IsAuthorOrReadOnly
from .serializers import PostSerializer

class PostList(generics.ListCreateAPIView):
    permission_classes = (IsAuthorOrReadOnly, )
    queryset = Post.objects.all()
    serializer_class = PostSerializer

class PostDetail(generics.RetrieveUpdateDestroyAPIView):
    permission_classes = (IsAuthorOrReadOnly, )
    queryset = Post.objects.all()
    serializer_class = PostSerializer
```

* Run local server
```shell
python manage.py runserver
```

* Login with the test account. Make a new entry.
