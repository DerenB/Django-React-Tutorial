# Django-React-Tutorial
Creating a demo project using Django and React following a tutorial

# 1.1: Installs

### Code Needs

- Python
- npm
- Node.js

### VSCode Extensions

- Python
- Django
- ES7+ React/Reduc/React
- JavaScript (ES6)

### Install Python Packages

- Open terminal in project folder
- Install Djano
    - CMD: `pip install django djangorestframework`

# 1.2: Setup Project

### Create Django project

- CMD: `django-admin startproject APPNAME`
- CMD: `django-admin startproject music_controller`
- "music_controller" is the project name, user choice
- run command in the directory where the project should be located

### Create API

- `CD` into the project folder created in previous step (music_controller)
- CMD: `django-admin startapp APPNAME`
- CMD: `django-admin startapp api`

### Add reference to API

- Open music_controller > music_controller > `settings.py`
- Scroll to "INSTALLED_APPS" (near line 40 by default)
- Add line to the end: `"api.apps.ApiConfig",`
  - "api" references the name for the API project you created
  - accessing the api > `apps.py` file
  - Calling the ApiConfig class
- Add the line after previous line: `"rest_framework"`

# 1.3: Create Views

- Where we write the "end points"
  - A location on the web server after the "/"
- Open file: api > `views.py`
- Add this code block:
```
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def main(request):
    return HttpResponse("Hello")
```

### Configure api URL file

- Within the API project, create file: `urls.py`
- Add code block:
```
from django.urls import path
from .views import main

urlpatterns = [
    path("", main)
]
```

### Configure music_controller URL file

- Open music_controller URL file
  - music_controller > `urls.py`
- The `urlpatterns` section does the directing for the URL
- import "include"
- add the path for the API url file
- Should look like this:
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("api.urls"))
]
```

# 1.4: Migrations

- Run this command since this is the first time running the app
  - Django automatically creates a database 
- CMD: `python manage.py makemigrations`
  - Need to run this every time:
    - There is a change to the model
    - There is a change to the database
- CMD: `python manage.py migrate`

# 1.5: Run Server

- CMD: `python manage.py runserver`
- Hot Reload Compatible 
  - Whenever you save, the server will reload
  - Can change python files, save, and see changes
  - 
