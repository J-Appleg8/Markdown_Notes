# <span style="color:lightseagreen">Fill Models With Practice Data Using Faker</span>

## <span style="color:palegreen">Set Up:</span>

```
py -m pip install Faker
```

<br>

## <span style="color:palegreen">Models:</span>

```python
from django.db import models
from django.db.models.deletion import CASCADE


class Topic(models.Model):
    top_name = models.CharField(max_length=264, unique=True)

    def __str__(self):
        return self.top_name


class Webpage(models.Model):
    topic = models.ForeignKey(Topic, on_delete=CASCADE)
    name = models.CharField(max_length=264, unique=True)
    url = models.URLField(unique=True)

    def __str__(self):
        return self.name


class AccessRecord(models.Model):
    name = models.ForeignKey(Webpage, on_delete=CASCADE)
    date = models.DateField()

    def __str__(self):
        return str(self.date)

```

<br>

## <span style="color:palegreen">Views:</span>

```python
from django.shortcuts import render
from django.http import HttpResponse


def index(request):
    my_dict = {"insert_me": "Hello I am from the views.py!"}
    return render(request, "second_app/index.html", context=my_dict)


def help(request):
    help_dict = {"help_me": "Help Page!"}
    return render(request, "second_app/help.html", context=help_dict)

```

<br>

## <span style="color:palegreen">URLs:</span>

```python
from django.urls import path
from . import views


app_name = "second_app"

urlpatterns = [
    # Homepage View
    path("", views.index, name="index"),
    path("help/", views.help, name="help"),
]

```

<br>

## <span style="color:palegreen">Faker Script:</span>

```python
import os

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "project_two.settings")

import django

django.setup()


# Fake Pop Script
import random
from second_app.models import AccessRecord, Webpage, Topic
from faker import Faker

fakegen = Faker()
topics = ["Search", "Social", "Marketplace", "News", "Games"]


def add_topic():
    t = Topic.objects.get_or_create(top_name=random.choice(topics))[0]
    t.save()
    return t


def populate(n=5):
    for entry in range(n):

        # Get the topic for the entry
        top = add_topic()

        # Create the fake data for that entry
        fake_url = fakegen.url()
        fake_date = fakegen.date()
        fake_name = fakegen.company()

        # Create new Webpage entry
        webpg = Webpage.objects.get_or_create(topic=top, url=fake_url, name=fake_name)[0]

        # Create fake access record for that webpage
        acc_rec = AccessRecord.objects.get_or_create(name=webpg, date=fake_date)[0]


if __name__ == "__main__":
    print("populating script!")
    populate(20)
    print("populating complete!")

```
