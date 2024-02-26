Blog API / 8. User Authentication
========================================================


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
    "rest_framework",
    "corsheaders",
    "rest_framework.authtoken",  # new
    # Local
    "accounts.apps.AccountsConfig",
    "posts.apps.PostsConfig",
]

REST_FRAMEWORK = {
    "DEFAULT_PERMISSION_CLASSES": [
        "rest_framework.permissions.IsAuthenticated",
    ],
    "DEFAULT_AUTHENTICATION_CLASSES": [  # new
        "rest_framework.authentication.SessionAuthentication",
        "rest_framework.authentication.TokenAuthentication",
    ],
}
```

* Apply the changes to the database
```shell
python manage.py migrate
```

* Start the local server
```shell
python manage.py runserver
```

* Login with your superuser account http://127.0.0.1:8000/admin/

Blog API / 7. User Authentication / dj-rest-auth
========================================================

* Stop the server with Control+c and install dj-rest-auth
```shell
pip install dj-rest-auth==2.1.11
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
    #3rd-party apps
    "rest_framework",
    "corsheaders",
    "rest_framework.authtoken",
    "dj_rest_auth",
    # Local
    "accounts.apps.AccountsConfig",
    "posts.apps.PostsConfig",
]

```

* Modify the urls.py file within the django_projects folder

```python
# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("api/v1/", include("posts.urls")),
    path("api-auth/", include("rest_framework.urls")),
    path("api/v1/dj-rest-auth/", include("dj_rest_auth.urls")),
]
```

* Start the local server
```shell
python manage.py runserver
```

* Check http://127.0.0.1:8000/api/v1/dj-rest-auth/login/

* and http://127.0.0.1:8000/api/v1/dj-rest-auth/logout/

* and http://127.0.0.1:8000/api/v1/dj-rest-auth/password/reset

* and http://127.0.0.1:8000/api/v1/dj-rest-auth/password/reset/confirm


Blog API / 7. User Authentication / User Registration
========================================================

* Stop the server with Control+c and install django-allauth
```shell
pip install django-allauth~=0.48.0
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
    "django.contrib.sites",
    #3rd-party apps
    "rest_framework",
    "corsheaders",
    "rest_framework.authtoken",
    "allauth",
    "allauth.account",
    "allauth.socialaccount",
    "dj_rest_auth",
    "dj_rest_auth.registration",
    # Local
    "accounts.apps.AccountsConfig",
    "posts.apps.PostsConfig",
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                "django.template.context_processors.request",
            ],
        },
    },
]

EMAIL_BACKEND = "django.core.mail.backends.console.EmailBackend"

SITE_ID = 1

```

* Apply the changes to the database
```shell
python manage.py migrate
```

* Modify the urls.py file within the django_projects folder
```python
# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("api/v1/", include("posts.urls")),
    path("api-auth/", include("rest_framework.urls")),
    path("api/v1/dj-rest-auth/", include("dj_rest_auth.urls")),
    path("api/v1/dj-rest-auth/registration/", include("dj_rest_auth.registration.urls")),
]
```

* Start the local server
```shell
python manage.py runserver
```

* Check http://127.0.0.1:8000/api/v1/dj-rest-auth/registration/

* Create a new user account
