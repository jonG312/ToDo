# Django Todo App

## Set-Up Virtual Environment.

Open the terminal in the IDE, and Make a Directory

```
mkdir todo_app
```
move in that directory by running the following command.

```
cd todo_app
```

Install the `virtualenv` package

within the directory, create a Python virtual environment by typing:

```
virtualenv myenv
```
then activate the virtual environment

```
cd myenv
Scripts/activate
```

## Set-Up Django

Now, inside this virtual environment, we are going to install Django and other requirements.

```
cd myenv
pip freeze > requirements.txt
```

Now, inside the file requirements.txt write the requirements

`requirements.txt:`

```
django
djangorestframework
```

Then, run the requirements file from the terminal

```
pip install -r requirements.txt
```

## Create a new app

Now, go to terminal and write the next line

```
cd todo_app
```
```
django-admin startproject task
```

## Register the task app and the djangorestframework in the todo_app project settings file

`settings.py:`

```
INSTALLED_APPS = [
    .
    .
    .
    'tasks.apps.TasksConfig',
    'rest_framework',
]
```

Then, migrate the database

```
cd todo_app
```
```
python manage.py makemigrations
python manage.py migrate
```

**Create super user**

```
python manage.py createsuperuser
```
```
Username (leave blank to use 'Shivansh'): francisco
Email address: francisco@123.com
Password: 12345678
Password (again): 12345678
Superuser created successfully.
```
Then, run the server

```
python manage.py runserver
```

## Create a model for the database that the Django ORM will manage.

`models.py:`

```
from django.db import models

# Create your models here.
class Task(models.Model):
    title = models.CharField(max_length=35)
    description = models.TextField(null=True, blank=True)
    due_date = models.DateField()
    due_time = models.TimeField()
    completed = models.BooleanField(default=False)

    def __str__(self) -> str:
        return f'{self.title}'
```
## Serialize the model data 

`serializers.py:`

```
from rest_framework import serializers
from .models import Task

class TaskSerializer(serializers.ModelSerializer):
    class Meta:
        model = Task
        fields = ['id', 'title', 'description', 'due_date', 'due_time', 'completed']
```

## Display API Overview

Adding the endpoints to the file urls.py

`task/urls.py:`

```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
    path('completed', views.completed, name='completed'),
    path('remaining', views.remaining, name='remaining'),
    path('add_task', views.add_task, name='add_task'),
    path('delete_task/<str:task_id>', views.delete_task, name='delete_task'),  # Corregido el nombre de la ruta
    path('task_detail/<str:task_id>', views.task_detail, name='task_detail'),
    path('toggle_complete/<str:task_id>', views.toggle_complete, name='toggle_complete'),
    path('remove_task/<str:task_id>', views.remove_task, name='remove_task'),
]
```
`task/views.py:`

Manage the CRUD operations and redirect the data to the templates.

```
from django.http import JsonResponse
from django.shortcuts import get_object_or_404, render, redirect
from .models import Task

# Create your views here.
def home(request):
    tasks = Task.objects.all()

    return render(request, 'index.html', {
        'tasks': tasks,
    })

def completed(request):
    completed_tasks = Task.objects.filter(completed=True)

    return render(request, 'completed.html', {
        'tasks': completed_tasks,
    })

def remaining(request):
    remaining_tasks = Task.objects.filter(completed=False)
    return render(request, 'remaining.html', {
        'tasks': remaining_tasks,
    })

def add_task(request):
    if request.method == "POST":
        title = request.POST.get('title')
        description = request.POST.get('description')
        due_date = request.POST.get('due_date')
        due_time = request.POST.get('due_time')
        completed = False

        if title != "" and due_date != "" and due_time !="":
            task = Task(
                title=title,
                description=description,
                due_date=due_date,
                due_time=due_time,
                completed=completed
            )
            task.save()
            return redirect('home')
    else:
        return render(request, 'add_task.html') 
    return render(request, 'add_task.html')

def delete_task(request, task_id):
    task = get_object_or_404(Task, id=task_id)
    
    if request.method == 'POST':
        task.delete()
        return redirect('home')
    
    return render(request, 'delete.html', {"task": task})

def task_detail(request, task_id):
     
     if request.method == 'GET':
        task = Task.objects.get(id=task_id)
        return render(request, 'task_detail.html', {            
        "task": task,    
    })

def toggle_complete(request, task_id):
    if request.method == 'GET':
        task = Task.objects.get(id=task_id)
        if task:
            task.completed = not task.completed
            task.save() 
            return redirect('home')

def remove_task(request, task_id):
    if request.method == 'POST':
        task = Task.objects.get(id=task_id)
        task.delete()
        return redirect('home')
```

## Functions and Endpoints
```
_________________________________________________________
|    Function       |      Endpoint                      |
|___________________|____________________________________|
|    home           |   /                                |
|___________________|____________________________________|
|    completed      |   /completed                       |
|___________________|____________________________________|
|    remaining      |   /remaining                       |
|___________________|____________________________________|
|    add_task       |   /delete_task/<str:task_id>       |
|___________________|____________________________________|
|   delete_task     |   /task_detail/<str:task_id>       |
|___________________|____________________________________|
|   task_detail     |   /task_detail/<str:task_id>       |
|___________________|____________________________________|
|   toggle_complete |   /toggle_complete/<str:task_id>   |
|___________________|____________________________________|
|   remove_task     |   /remove_task/<str:task_id>       |
|___________________|____________________________________|
```

## How to run

- open the todo_app folder in the IDE -> Then

`run local:`

```
python manage.py runserver
```
Then, open ```http://127.0.0.1:8000/``` in the Browser.

`run the dockerfile:`

```
docker run -d -p 8000:8000 gjonasg/to-do:latest
```
