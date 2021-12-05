# <span style="color:lightseagreen">Templates & Template Tagging:</span>

## <span style="color:palegreen">Overview:</span>

1. We can use templates to have a "base" template and inherit that template in the .html files

   - This saves a lot of time and will help create a unified look and feel across every page of your website

1. Templates can also be used to solve issues with relative paths and working with variables

1. Templates can also help solve issues by avoiding hard-coded URL paths

1. Templates also come with a built-in filter capabilities so you can adjust variables on the actual individual page

<br>

## <span style="color:palegreen">Relative URLs with Templates:</span>

You can replace hard-coded URLs in a template a few different ways:

For Example:

```html
<a href="basicapp/thankyou">Thanks</a>
```

Can be changed to:

- name="thanku" would be in the `urls.py`

```html
<a href="{% url 'thanku' %}">Thanks</a>
```

However, the suggested (and most future-proof) method to do this involves namespacing in the `urls.py` file

- Inside the `urls.py` file you can add the variable: app_name
- You then set this variable equal to a string that is the same as your app name

This is the best way to use URL templates:

```html
<a href="basicapp/thankyou">Thanks</a>
```

Can be changed to:

- name="thanku" would be in the `urls.py`

```html
<a href="{% url 'basicapp:thanku' %}">Thanks</a>
```
