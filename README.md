# Django-React-Tutorial

- Creating a demo project using Django and React following a tutorial
- [Video 1 of the Tutorial Series](https://www.youtube.com/watch?v=JD-age0BPVo&list=PLzMcBGfZo4-kCLWnGmK0jUBmGLaJxvi4j)

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
 
# 2.1 Create Database Model

- Django Models:
  - A model acts like a table
  - Constraints on the field in the Django documentation
  - Put most logic in the model. Fat model, thin view
- Open Models file: api > `models.py`
- Set `models.py` to codeblock:
- **Have to make the migrations and apply them**
  - Migrate CMD: `python manage.py makemigrations`
  - Apply CMD: `python manage.py migrate`
```
from django.db import models
import string
import random

# generates a random code that is K length
# only contains the uppercase ascii characters
# continues to generate random codes until it finds one not already in the database
def generate_unique_code():
  length = 6
  while True:
    code = ''.join(random.choices(string.ascii_uppercase, k=length))
    if Room.objects.filter(code=code).count() == 0:
      break
  return code

# Create your models here.
class Room(models.Model):
  # Max length required for char field
  # holds a bunch of characters
  # Creating the constraints of the field
  code = models.CharField(max_length=8, default="", unique=True)
  host = models.CharField(max_length=50, unique=True)
  guest_can_pause = models.BooleanField(null=False, default=False)
  votes_to_skip = models.IntegerField(null=False, default=1)
  created_at = models.DateTimeField(auto_now_add=True)
```

# 2.2 Create API View

- In the API project, create file: `serializers.py`
  - this takes the model (room) with the python related code
  - translate the python into a json response
  - takes the keys, turns them into string, and assigns the value
- Create a class for the serializer
  - Should have fields that match the model
- Code Block:
```
from rest_framework import serializers
from .models import Room

class RoomSerializer(serializers.ModelSerializer):
  class Meta:
    model = Room
    fields = ('id', 'code', 'host', 'guest_can_pause', 'votes_to_skip', 'created_at')
```

# 2.3 Create API View

- File: api > `views.py`
- Lets us view a list of all the different rooms
- Accesses the serializer class made in previous step
- Code block:
```
from django.shortcuts import render
from rest_framework import generics
from .serializers import RoomSerializer
from .models import Room

# Create your views here.
class RoomView(generics.CreateAPIView):
  queryset = Room.objects.all()
  serializer_class = RoomSerializer
```

# Set URL for Room View

- File: api > `urls.py`
```
from django.urls import path
from .views import RoomView

urlpatterns = [
  path("room", RoomView.as_view())
]
```