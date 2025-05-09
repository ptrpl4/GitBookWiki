# 🐘 Django

## Quick Start

```bash
# create venv
$ python3 -m venv env
# activate env
$ source env/bin/activate
# install Django
$ pip install Django
# create project
$ django-admin startproject project_name
# create DB
$ python manage.py migrate
# run project
$ python project/manage.py runserver
```

## Add App

```bash
# create new app in project
$ python manage.py startapp app_name
```

### Create Modules 

Open app_name/modules.py

```bash
from django.db import models

# Create your models here.
class Topic(models.Model):
    """User Topic"""
    text = models.CharField(max_length=200)
    date_added = models.DateTimeField(auto_now_add=True)
    def __str__(self):
        """Return String"""
        return self.text
```

### Activate Modules

Open project_name/settings.py

Find `INSTALLED_APPS`

```python
INSTALLED_APPS = [
    ....
    'django.contrib.staticfiles',
    #My Apps
    'app_name',
]
```

 Update DB

```bash
# create migration for app app_name
$ python manage.py makemigrations app_name
# upadte DB for project
$ python manage.py migrate
```

### Register model

add in app_name/admin.py

```python
from learning_logs.models import Topic
admin.site.register(Topic)
```

## Create Superuser

```bash
$ python manage.py createsuperuser
```

## Create new URL+View

project/project/urls.py

```python
from django.contrib import admin
from django.urls import path, include

# add path
urlpatterns = [
    path('admin/', admin.site.urls),
    # if url contains 'blog/blabla/' it sends blabla to blog.urls
    path('', include('blog.urls')),
]
```

project/blog/urls.py

```python
from django.urls import path
from . import views

# add path inside app
urlpatterns = [
    path('', views.home, name='blog-home'),
    path('about/', views.about, name='blog-about'),
]
```

project/blog/views.py

```python
from django.shortcuts import render
from django.http import HttpResponse


# example how it works
def home(request):
    return HttpResponse('<h1>Blog Home</h1>')


# example with templates
# render use template from templates folder
def about(request):
    return render(request, 'blog/about.html')
```

## Templates for app

create folder & files

```bash
# create folder 'templates' in project/app
mkdir blog/templates
# create html file
echo '<!DOCTYPE html>' > blog/templates/blog.html 
```

Use content

```python
# project/blog/views.py
example_posts = [
    {
        'author': 'admin',
        'title': 'The beginning',
        'content': 'post content',
        'date_posted': ' Today'
    },
{
        'author': 'user',
        'title': 'The ending',
        'content': 'post content',
        'date_posted': ' Yesterday'
    }
]

def home(request):
    context = {
        'posts': example_posts
    }
    return render(request, 'blog/home.html', context)
```

In template file

```html
# project/blog/templates/blog/home.html
<body>
    #add for loop
    {% for post in posts %}
    #use data {{ dict_name.key_name}}
    <h1>{{ post.title }}</h1>
    <p>By {{ post.author }} on {{ post.date_posted}}</p>
    <p>{{ post.content }}</p>
    {% endfor %}
</body>
```
