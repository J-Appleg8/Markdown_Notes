# <span style="color:lightseagreen">Django Forms:</span>

## <span style="color:palegreen">Overview:</span>

Django Forms Advantages:

- Can quickly generate HTML form widgets
- Validate data and process it into a Python data strucure
- Create form versions of our Models, quickly update models from Forms

<br>  
The first thing we need to do is create a `forms.py` file inside the application.
After that we call Django's built in forms classes (looks very similar to creating models)

```python
from django import forms

class FormName(forms.Form):
    name = forms.CharField()
    email = forms.EmailField()
    text = forms.CharField(widgets=forms.Textarea)

```

<br>  
Now that we have the form created inside the application's `forms.py` file, we need to show it by using a view

Inside our `views.py` file we need to import the forms

- The . just indicates to import from the same directory as the current .py file

```python
from . import forms

```

<br>

We can then create a new view for the form

```python
def form_name_view(request):
    form = forms.FormName()
    return render(request, "form_name.html", {"form":form})

```

<br>

Then we just add the view to the app's urls either directly or with include()

```python
from basicapp import views

urlpatterns = [
    path("formpage/", views.form_name_view, name="form_name")
]

```

<br>

We can then create the templates folder along with the html file that will hold the template tagging for the form

- Remember to update the `settings.py` file to reflect the TEMPLATE_DIR variable

<br>

So now everything is set up for us to go into the form_name.html file inside templates/basicapp and add in the actual template tagging that will create the Django Form

<br>

There are several ways you can inject the form using template tagging. You can just pass in the key from the context dictionary:

```
{{ form }}
```

<br>

On your form_page.html it will look something like this:

- form.as_p lines the form up using `<p>`
- {% csrf_token %} is also added:
  - This is a Cross-Site Request Forgery (CSRF) token, which secures the HTTP POST action that is initiated on the subsequent submission of a form
  - In other words, when a user clicks submit, it protects your website from getting false data, and protects the user from accidentally sending data somewhere else
  - The Django framework requires the CSRF token to be present, if its not there then your form may not work
  - It works by using a "hidden input" which is a random code and checking that it matches the user's local site page

```python
<div class="container">
    <form method="POST">
        {{ form.as_p }}
        {% csrf_token %}
        <input type="submit" class="btn btn-primary" value="Submit">
    </form>
</div>

```

<br>

Right now, if we hit submit, nothing will happen. We need to inform the view that if we get a POST back, we should check if the data is valid and if so, grab that data. We can do this by editing the view

- Upon receiving a validated form, we can access a dictionary like attribute of the "cleaned_data"

```python
def form_name_view(request):
    form = forms.FormName()

    # Check to see if we get a POST back
    if request.method == "POST":

        # In which case we pass in that request
        form = forms.FormName(request.POST)

        # Check to see if form is valid
        if form.is_valid():
            # Do something
            print("Form Validation Success")
            print("Name"+form.cleaned_data["name"])
            print("Email"+form.cleaned_data["email"])
            print("Text"+form.cleaned_data["text"])

    return render(request, "basicapp/form_page.html", {"form":form})

```

<br>

### <span style="color:lightyellow">Side Discussion: HTTP - GET - POST</span>

HTTP:

- Stands for Hypertext Transfer Protocol
- Designed to enable communication between a client and a server
- The client submits a request, the server tehn responds
- The most commonly used methods for this request/response protocol are GET and POST

GET:

- Requests data from a source

POST:

- Submits data to be processed to a resource

<br>

# <span style="color:lightseagreen">Django Model Forms:</span>

## <span style="color:palegreen">Overview:</span>

Model forms allow you to accept form input and pass it to a model. Instead of inheriting from forms.Forms, we will inherit from forms.ModelForm

- This helper class allows us to create a form from a pre-existing model
- We then add an inline class called Meta, which provides information connecting the model to the form

```python
from django import forms
from myapp.models import MyModel

class MyNewForm(forms.ModelForm):

    # Form Fields go here (with validators params if you want validators)

    class Meta:
        model = MyModel
        fields = # Lets see the options!

```

<br>

Option 1: Set it to "`__all__`":

```python
from django import forms
from myapp.models import MyModel

class MyNewForm(forms.ModelForm):

    # Form Fields go here (with validators params if you want validators)

    class Meta:
        model = MyModel
        fields = "__all__"

```

<br>

Option 2: Exclude certain fields

```python
from django import forms
from myapp.models import MyModel

class MyNewForm(forms.ModelForm):

    # Form Fields go here (with validators params if you want validators)

    class Meta:
        model = MyModel
        exclude = ["field1", "field2"]

```

<br>

Option 3: List included fields

```python
from django import forms
from myapp.models import MyModel

class MyNewForm(forms.ModelForm):

    # Form Fields go here (with validators params if you want validators)

    class Meta:
        model = MyModel
        fields = ("field1", "field2")

```

<br>
