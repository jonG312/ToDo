# Django Todo App

##Set-Up Virtual Environment.

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

##Set-Up Django

Now, inside this virtual environment, we are going to install Django and other requirements.

```
cd myenv
pip freeze > requirements.txt
```

Now, inside the file requirements.txt write de requirements

`requirements.txt:`

```
django
djangorestframework
```

Then, run the requirements file from the terminal

```
pip install -r requirements.txt
```

##Create a new app

Now, go to terminal and write the next line

```
cd todo_app
```

```
django-admin startproject task
```


##Create a model for the database that the Django ORM will manage.

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













##Set-Up Django Rest Framework


##Serialize the model data from step-3


##Create the URI endpoint to view the serialized data.