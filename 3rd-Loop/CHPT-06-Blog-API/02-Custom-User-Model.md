Blog API / 2. Custom User Model
========================================================

* Create the app accounts

```shell
python manage.py startapp accounts
```

* Add it to the apps in the settings file

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
]
```

* Edit the models.py file within the accounts folder
```python
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    name = models.CharField(null=True, blank=True, max_length=100)
```

* Update the settings

```python
# django_project/settings.py
AUTH_USER_MODEL = "accounts.CustomUser"
```

* Apply the changes to the database
```shell
python manage.py makemigrations
python manage.py migrate
```

* Create a superuser
```shell
python manage.py createsuperuser
```

* Add the forms.py file to the accounts folder
```python
# accounts/forms.py

from django.contrib.auth.forms import UserCreationForm, UserChangeForm

from .models import CustomUser

class CustomUserCreationForm(UserCreationForm):
    class Meta(UserCreationForm):
        model = CustomUser
        fields = UserCreationForm.Meta.fields + ("name",)

class CustomUserChangeForm(UserChangeForm):
    class Meta(UserChangeForm):
        model = CustomUser
        fields = UserChangeForm.Meta.fields
```

* Update the admin.py file within the accounts directory
```python
# accounts/admin.py

from django.contrib import admin
from django.contrib.auth.admin import UserAdmin

from .forms import CustomUserCreationForm, CustomUserChangeForm
from .models import CustomUser

class CustomUserAdmin(UserAdmin):
    add_form = CustomUserCreationForm
    form = CustomUserChangeForm
    model = CustomUser
    list_display = [
        "email",
        "username",
        "name",
        "is_staff",
    ]
    fieldsets= UserAdmin.fieldsets + ((None, {"fields": ("name",)}), )
    add_fieldsets = UserAdmin.add_fieldsets + ((None, {"fields": ("name",)}), )

admin.site.register(CustomUser, CustomUserAdmin)
```

* Check http://127.0.0.1:8000/admin/