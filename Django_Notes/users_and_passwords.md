# <span style="color:lightseagreen">User Authentication:</span>

<br>

## <span style="color:palegreen">Passwords:</span>

---

The first thing we need to take care of is set up our ability to authenticate a User. To do this, we need to use some built-in apps and make sure they are under the INSTALLED_APPS list in settings.py

The next thing we need to do is make sure we store our passwords safely

- We will begin by using the default PBKDF2 algorithm with an SHA256 hash that's built into Django

We will then use the bcrypt and Argon2 hash

- In your virtual environment:

```
pip install bcrypt
pip install django[argon2]
```

Depending on your Django version you may already have these installed

<br>

## <span style="color:palegreen">Set Up User Model:</span>

---

`models.py` file example:

```python
from django.contrib.auth.models import User

class UserProfileInfo(models.Model):

    user = models.OneToOneField(User)

    portfolio = models.URLField(blank=True)
    picture = models.ImageField(upload_to='profile_pics')

    def __str__(self):

        # username is a built-in attribute of django
        return self.user.username
```

Once you've created this model you will have to remember to register it in the `admin.py` file

```python
admin.site.register(UserProfileInfo)
```

In order to work with images with Python we will need to install the Python Imaging Library with:

```
pip install pillow
```

The code inside `forms.py` would look something like:

```python
from django import forms
from basic_app.models import UserProfileInfo

class UserProfileInfoForm(forms.ModelForm):
    portfolio = forms.URLField(required=False)
    picture = forms.ImageField(required=False)

    class Meta:
        model = UserProfileInfo
        exclude = ('user',)

```

<br>

## <span style="color:palegreen">User Login:</span>

---

This process involves:

- Setting up the login views
- Using built-in decorators for access
- Adding the LOGIN_URL in settings
- Creating the login.html
- Editing the `urls.py` file

<br>

### <span style="color:lightyellow">Full example from lecture of Registration page and Login page:</span>

<br>

Registration and Login/Logout in `views.py` Example:

```python
from django.shortcuts import render
from django.contrib.auth import authenticate, login, logout
from django.http import HttpResponseRedirect, HttpResponse
from django.urls import reverse
from django.contrib.auth.decorators import login_required
from basic_app.forms import UserForm, UserProfileInfoForm


def index(request):
    return render(request, "basic_app/index.html")


def register(request):

    registered = False

    if request.method == "POST":

        user_form = UserForm(data=request.POST)
        profile_form = UserProfileInfoForm(data=request.POST)

        if user_form.is_valid() and profile_form.is_valid():
            user = user_form.save()
            user.set_password(user.password)
            user.save()

            profile = profile_form.save(commit=False)
            profile.user = user

            if "profile_pic" in request.FILES:
                profile.profile_pic = request.FILES["profile_pic"]

            profile.save()

            registered = True

        else:
            print(user_form.errors, profile_form.errors)

    else:
        user_form = UserForm()
        profile_form = UserProfileInfoForm()

    return render(
        request,
        "basic_app/registration.html",
        context={"user_form": user_form, "profile_form": profile_form, "registered": registered},
    )


def user_login(request):

    if request.method == "POST":
        username = request.POST.get("username")
        password = request.POST.get("password")

        user = authenticate(username=username, password=password)

        if user:
            if user.is_active:
                login(request, user)
                return HttpResponseRedirect(reverse("index"))

            else:
                return HttpResponse("Account Not Active")

        else:
            print("Someone tried to log in and failed")
            print("Username: {} and password {}".format(username, password))
            return HttpResponse("invalid login details supplied")

    else:
        return render(request, "basic_app/login.html", {})


@login_required
def user_logout(request):
    logout(request)
    return HttpResponseRedirect(reverse("index"))
```

<br>

Registration form in `forms.py`:

```python
from django import forms
from django.contrib.auth.models import User
from basic_app.models import UserProfileInfo


class UserForm(forms.ModelForm):

    password = forms.CharField(widget=forms.PasswordInput())

    class Meta:
        model = User
        fields = ("username", "email", "password")


class UserProfileInfoForm(forms.ModelForm):
    class Meta:
        model = UserProfileInfo
        fields = ("portfolio_site", "profile_pic")
```

<br>

Example of `models.py`:

```python
from django.db import models
from django.contrib.auth.models import User


class UserProfileInfo(models.Model):

    user = models.OneToOneField(User, on_delete=models.CASCADE)
    portfolio_site = models.URLField(blank=True)
    profile_pic = models.ImageField(upload_to="profile_pics", blank=True)

    def __str__(self):
        return self.user.username
```

<br>

Registration template in `registration.html`:

```html
{% block body_block %}
<div class="jumbotron">
  {% if registered %}
  <h1>Thank you for registering!</h1>

  {% else %}
  <h1>Register Here!</h1>
  <h3>Fill out the form:</h3>

  <form method="post" enctype="multipart/form-data">
    {% csrf_token %} {{ user_form.as_p }} {{ profile_form.as_p }}
    <input type="submit" value="Register" />
  </form>

  {% endif %}
</div>

{% endblock %}
```

<br>

Login template in `login.html`:

```html
{% block body_block %}
<div class="jumbotron">
  <h1>Please Log In:</h1>

  <form action="{% url 'basic_app:user_login' %}" method="post">
    {% csrf_token %}

    <label for="username">Username:</label>
    <input type="text" name="username" placeholder="Enter Username" />

    <label for="password">Password:</label>
    <input type="password" name="password" />

    <input type="submit" value="Login" />
  </form>
</div>
{% endblock %}
```
