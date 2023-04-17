---
title: Django's design pattern
---

## Design pattern of Django
Software Degisn Pattern : Reusable solution for to solve common problems in process of software development

### MTV pattern
#### Model
Model is a code describing format of data that connects database and Django. Generally each model mapped to database table
- using python class, inherit all of django.db.models.Model class
- Each model attribution indicates database field
Default file name is models.py
```python
class DjangoModel(models.Model):
    name = models.CharField('name')
```

#### Template
Template : Code which we will return to web browser and represent format of reult provided to user. 

#### View
View : Code which have a logic handling user's request. We use function of python
Default file name is view.py
```python
def django_view(request):
    return HttpResponse("Django View")
```
