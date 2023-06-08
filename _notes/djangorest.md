---
title: RestFramework of django
---

- From [[django]]

## Why we use DRF(Django Rest Framework)
- Web browser API help our dev. easier
- Provide **Serialization**(DB data to Json) function
- Documentation and Community
- Seperate frontend and backend

## Process
### Installation

```
$ pip install djangorestframework
```

### Add to settings.py

```python
INSTALLED_APPS = [
    ...,
    'rest_framework',
]
```

### Django Superuser

```python
python manage.py createsuperuser
```

### Model

```python
from django.db import models

# Create your models here.
class Burger(models.Model):
    name = models.CharField(max_length=20)
    price = models.IntegerField(default=0)
    calories = models.IntegerField(default=0)

    def __str__(self):
        return self.name
```

#### django shell

```shell
python manage.py shell
```

<hr>

#### Django ORM
ORM(Object-relational mapping) is function which connect data to object. In django ORM, Models calss connect to database and object attribute of model class let use use SQL query
> You can see all object with "{model name}.objects.all" command
> {model name}.objects.get({attribute}=keyword)
-> get one specific object from the model
> {model name}.objects.filter()
-> get specific objects from the model
> len(model name)
-> get the number of objects of the model

### View
#### How to get data
1. Request data with unique URL
2. URLconf interprets received URL and execute corresponding view function
3. View function get data from database using model class 
4. View function deliever aquired data to Template
5. Template creates dynamic HTML
6. sends to customer

```python
from django.shortcuts import render
from {application name}.models import {model name}

# Create your views here.
def list(request):
    objects = Burger.objects.all()

    # deliever data to template
    context = {
        "objects" : objects,
    }

    return render(request, "list.html", context)
```

#### deliever data to View
> Use **?keyword={keywork}**

```python
request.GET     # delivered data from request
request.GET.get("{keyword}")    # get key value of keyword 

if keyword is not None:
    objects = {model name}.objects.filter(name__contains=keyword)   # find objects which containes keyword in the attribute
else:
    objects = {model name}.objedcts.none()

```

### Template
#### Variable
If you want to variable in template, put variable name in **{{variable name}}**

#### for statement

```html
{% for object in objects %}
    <div>{{object.attribute}}</div>
{% endfor %}
```

## DRF
### Serializer
Serializer let us filed value in Model 

```python
# app name/serializer.py
from rest_gramework import serializers
from .models import {model anem}

class {serializer name}(serializers.ModelSerializer):
    class Meta:
        model = {model name}
        filed = (fields that you want to use)
```

### Viewset
Model view set basically supports **Create, Retrieve, Update, Partial_Update, Destory, List**
Serializer_class : we will use previous declared serailizer class

```python
# views.py
from rest_framework import viewsets
from .serializers import {delared serializer}
from .models import {model name}

class {viewset name}(viewsets.ModelViewSet):
    queryset = {query set}
    serializer_class = {delared serializer name}
```

- Viewsets combines repeated logic to one class
- As using router, we do not need to handle URL setting

### Router

```python
# urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('{project name}.urls')) # use {project name}/urls.py
]
```

```python
from django.urls import include, path
from rest_gramework import routers
from . import views

router = routers.DefaultRouter()    # set DeafultRouter
router.register('router name', views.{viewset name})

urlpattenrs = [
    path('', include(router.urls)),
]
```

<hr>

## Conclustion
We could seperate backend and frontend using Serializer which convert DB data format to json