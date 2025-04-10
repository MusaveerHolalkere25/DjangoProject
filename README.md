This repo Create a Django app, and set up models, views, and templates for a basic CRUD operation.

Steps

Step 1: Install Django

pip install django

Step 2: Create a Django Project

django-admin startproject myproject

cd myproject

Step 3: Create a Django App

python manage.py startapp myapp

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

Step 4: Create a Model (models.py)

Inside myapp/models.py, define a simple model for a Task:

from django.db import models

class Task(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    completed = models.BooleanField(default=False)

    def __str__(self):
        return self.title


Step 5: Apply Migrations

Run the following commands:

python manage.py makemigrations

python manage.py migrate

Step 6: Create a Form (forms.py)

Inside myapp/forms.py, create a form for adding tasks:

from django import forms
from .models import Task

class TaskForm(forms.ModelForm):
    class Meta:
        model = Task
        fields = ['title', 'description', 'completed']


Step 7: Create Views (views.py)

Modify myapp/views.py to handle CRUD operations:

from django.shortcuts import render, redirect, get_object_or_404
from .models import Task
from .forms import TaskForm

# List all tasks
def task_list(request):
    tasks = Task.objects.all()
    return render(request, 'task_list.html', {'tasks': tasks})

# Create a new task
def task_create(request):
    if request.method == "POST":
        form = TaskForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('task_list')
    else:
        form = TaskForm()
    return render(request, 'task_form.html', {'form': form})

# Update an existing task
def task_update(request, pk):
    task = get_object_or_404(Task, pk=pk)
    if request.method == "POST":
        form = TaskForm(request.POST, instance=task)
        if form.is_valid():
            form.save()
            return redirect('task_list')
    else:
        form = TaskForm(instance=task)
    return render(request, 'task_form.html', {'form': form})

# Delete a task
def task_delete(request, pk):
    task = get_object_or_404(Task, pk=pk)
    if request.method == "POST":
        task.delete()
        return redirect('task_list')
    return render(request, 'task_confirm_delete.html', {'task': task})


Step 8: Create Templates (templates/)

Inside myapp/templates/, create HTML templates:

Task List (task_list.html)

<!DOCTYPE html>
<html>
<head><title>Task List</title></head>
<body>
    <h1>Task List</h1>
    <a href="{% url 'task_create' %}">Add Task</a>
    <ul>
        {% for task in tasks %}
            <li>
                {{ task.title }} - {{ task.description }}
                <a href="{% url 'task_update' task.id %}">Edit</a> |
                <a href="{% url 'task_delete' task.id %}">Delete</a>
            </li>
        {% endfor %}
    </ul>
</body>
</html>


Task Form (task_form.html)

<!DOCTYPE html>
<html>
<head><title>Task Form</title></head>
<body>
    <h1>{% if form.instance.pk %}Edit Task{% else %}New Task{% endif %}</h1>
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Save</button>
    </form>
    <a href="{% url 'task_list' %}">Back to list</a>
</body>
</html>


Task Delete (task_confirm_delete.html)

<!DOCTYPE html>
<html>
<head><title>Delete Task</title></head>
<body>
    <h1>Are you sure you want to delete "{{ task.title }}"?</h1>
    <form method="post">
        {% csrf_token %}
        <button type="submit">Yes, Delete</button>
    </form>
    <a href="{% url 'task_list' %}">Cancel</a>
</body>
</html>


Step 9: Configure URLs (urls.py)

Modify myapp/urls.py:

from django.urls import path
from .views import task_list, task_create, task_update, task_delete

urlpatterns = [
    path('', task_list, name='task_list'),
    path('create/', task_create, name='task_create'),
    path('update/<int:pk>/', task_update, name='task_update'),
    path('delete/<int:pk>/', task_delete, name='task_delete'),
]


Modify myproject/urls.py to include myapp routes:

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),
]


Step 10: Run the Django Server

python manage.py runserver

Open the app in a browser: http://127.0.0.1:8000/