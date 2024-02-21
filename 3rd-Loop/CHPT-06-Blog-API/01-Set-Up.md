Blog API / 1. Set-Up
========================================================

* Make a new directory

```shell
mkdir blogapi
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


* Run the Django development server
```shell
python manage.py runserver
```

* Stop the local server by typing Control+c

* Initialize a new Git repo
```shell
git init
```

* Create the .gitignore file
```.gitignore
.venv/
```

* Build the Git repo
```shell
git status
git add -A
git commit -m "initial commit"
```