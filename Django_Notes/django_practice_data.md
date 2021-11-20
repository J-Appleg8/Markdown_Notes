# <span style="color:lightseagreen">Example 1: Fill Models With Fake Company Data Using Faker</span>

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

<br>

# <span style="color:lightseagreen">Example 2: Fill Models With Fake User Data Using Faker</span>

<br>

## <span style="color:palegreen">Models:</span>

```python
from django.db import models
from django.db.models.deletion import CASCADE


class User(models.Model):
    first_name = CharField(max_length=264)
    last_name = CharField(max_length=264)
    email = EmailField(max_length=264)

```

<br>

## <span style="color:palegreen">Views:</span>

```python
from django.shortcuts import render
from django.http import HttpResponse
from .models import Topic, Webpage, AccessRecord, User


def index(request):
    webpages_list = AccessRecord.objects.order_by("date")
    date_dict = {"access_records": webpages_list}

    return render(request, "second_app/index.html", context=date_dict)


def user_data(request):
    userdata_list = User.objects.order_by("first_name")
    user_dict = {"user_list": userdata_list}

    return render(request, "second_app/users.html", context=user_dict)

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
    path("users/", views.user_data, name="user_data"),
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
from second_app.models import User
from faker import Faker

fakegen = Faker("en_GB")


def populate_users(n=5):
    for entry in range(n):

        # Create the fake data for that entry
        fake_first_name = fakegen.first_name()
        fake_last_name = fakegen.last_name()
        fake_email = fakegen.email()

        # Create new User entry
        new_user = User.objects.get_or_create(
            first_name=fake_first_name, last_name=fake_last_name, email=fake_email
        )[0]


if __name__ == "__main__":
    print("populating script!")
    populate_users(20)
    print("populating complete!")

```

<br>

## <span style="color:palegreen">HTML Page:</span>

```html
<!DOCTYPE html>
{% load static %}

<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>User List</title>
  </head>

  <body>
    <div class="container">
      <h1>Hi welcome to Django Level Two</h1>
      <h2>These are your user records:</h2>

      {% if user_list %}
      <table>
        <thead>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Email Address</th>
        </thead>

        {% for user in user_list %}
        <tr>
          <td>{{ user.first_name }}</td>
          <td>{{ user.last_name }}</td>
          <td>{{ user.email }}</td>
        </tr>
        {% endfor %}
      </table>
      {% else %}
      <p>No User Records Found</p>
      {% endif %}
    </div>
  </body>
</html>
```

<br>

## <span style="color:palegreen">Web Page:</span>

![alt text](images/userlist.jpg "Title")
