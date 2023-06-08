---
title: Simple application template of Django
---

- From [[django]]

## 2. Making Django project
```shell
django-admin startproject {project name} .
```

Activate server
```shell
python manage.py runserver
```

## 3. View
1. Create views.py
```python
from django.http import HttpResponse
def main(request):
    return HttpResponse("{content of response}")
```

```python
from django.shortcuts import render

def main(request):
    return render(request, "main.html")
```

## 4. URLconf
```python
# from django.contrib import admin
# from django.urls import path
from config.views import main

urlpatterns = [
    # path("admin/", admin.site.urls),
    path("", main),
]
```

## 5. Template
1. add template directory in **settings.py**
```python
# BASE_DIR = Path(__file__).resolve().paretn.parent
TEMPLATES_DIR = BASE_DIR / "templates"
```
```python
TEMPLATES = [
    {
        "DIRS" : [TEMPLATES_DIR],
    },
]
```

## 5. Database and Django
### 5-1. Application
```shell
> python manage.py startapp {application name}
```
1. Add new applicatio in "INSTALLED_APPS" list
2. Make new model
```python
class {model name}:
    name = models.IntegerField(default=0)
    ...
```
3. migration
```shell
> python3 manage.py migrate
```