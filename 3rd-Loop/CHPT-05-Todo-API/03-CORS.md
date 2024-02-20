Todo API / 3. Cross-Origin Resource Sharing (CORS) and CSRF 
========================================================

* Install django-cors-headers

```shell
python -m pip install django-cors-headers~=3.10.0
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
    "corsheaders",  # new
    # Local
    "todos.apps.TodosConfig",
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    "corsheaders.middleware.CorsMiddleware",  # new
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]


CORS_ALLOWED_ORIGINS = (      # new
    "http://localhost:3000",
    "http://localhost:8000",
)

#CSRF_TRUSTED_ORIGINS = ["localhost:3000"]
```