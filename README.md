# This repo Create a Django app, and set up models, views, and templates for a basic CRUD operation.

## Steps

***Step 1: Install Django***

```python
pip install django
```

***Step 2: Create a Django Project***

```python
django-admin startproject myproject

cd myproject
```

***Step 3: Create a Django App***

```
python3 manage.py startapp myapp

Add myapp to INSTALLED_APPS in myproject/settings.py:

INSTALLED_APPS = [

    'django.contrib.admin',

    'django.contrib.auth',

    'django.contrib.contenttypes',

    'django.contrib.sessions',

    'django.contrib.messages',

    'django.contrib.staticfiles',

    'myapp',  # Add this line

]
```

***Step 4: Create a Model (models.py)***

```
Inside myapp/models.py, define a simple model for a Task:
```


***Step 5: Apply Migrations***

```
Run the following commands:

python3 manage.py makemigrations

python3 manage.py migrate
```

***Step 6: Create a Form (forms.py)***

```
Inside myapp/forms.py, create a form for adding tasks:
```

***Step 7: Create Views (views.py)***

```
Modify myapp/views.py to handle CRUD operations:
```


***Step 8: Create Templates (templates/)***

```
Inside myapp/templates/, create HTML templates:

Task List (task_list.html)

Task Form (task_form.html)

Task Delete (task_confirm_delete.html)
```


***Step 9: Configure URLs (urls.py)***

```
Modify myapp/urls.py:

Modify myproject/urls.py to include myapp routes:

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),
]
```

***Step 10: Run the Django Server***
```
python manage.py runserver

Open the app in a browser: http://127.0.0.1:8000/
```