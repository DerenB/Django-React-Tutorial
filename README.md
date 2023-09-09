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


