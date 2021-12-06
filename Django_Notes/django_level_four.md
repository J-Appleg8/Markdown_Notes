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

<br>

# <span style="color:lightseagreen">Template Inheritance:</span>

## <span style="color:palegreen">Overview:</span>

Template inheritance saves a lot of repetitive work and makes it much easier to maintain the same base look and feel across the entire website

For Example: If we wanted a navbar at the top of our page, it wouldn't make sense to continually have the same navbar HTML code in each individual .html file

### <span style="color:lightyellow">The main steps for inheritance:</span>

1. Find the repetitive parts of your project
1. Create a base template of them
1. Set the tags in the base template
1. Extend and call those tags anywhere

`base.html`:

```html
<links to JS, CSS, Bootstrap></links>
<!-- <bunch of html like navbars> -->
<body>
  {% block body_block %} {% endblock %}
</body>
```

`other.html`

```html
<!DOCTYPE html>
{% extends "basic_app/base.html" %}
<p></p>
{% block body_block %}
<p>HTML specific to other.html</p>
{% endblock %}
```

<br>

# <span style="color:lightseagreen">Templates, Features & Filters:</span>

## <span style="color:palegreen">Template Filters:</span>

Filters allow you to make changes to the information before injecting it to the site page, like string operations or arithmetic

Luckily Django provides a ton of easy to implement filters that allow you to effect the inject before displaying it to the user

### <span style="color:lightyellow">The general form of a filter is:</span>

- Not all filters take in parameters though

```html
{{ value | filter }}
```

For Example:

```html
{{ django | title }}
```

with the context of:

```python
{'django': 'the web framework for perfectionists with deadlines'}
```

### <span style="color:lightyellow">Some filters also take in parameters:</span>

```html
{{ value | filter:"parameter" }}
```

For Example:

```html
{{ my_date | date:"Y-m-d" }}
```
