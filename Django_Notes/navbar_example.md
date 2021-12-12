# <span style="color:lightseagreen">Bootstrap Navbar:</span>

<br>

## <span style="color:palegreen">Example:</span>

---

Can be placed in `base.html` for Django:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link
      rel="stylesheet"
      href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
      integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T"
      crossorigin="anonymous"
    />
    <title>Document</title>
  </head>

  <body>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
        <ul class="nav navbar-nav">
          <li class="nav-item">
            <a class="navbar-brand" href="{% url 'admin:index' %}">Admin</a>
          </li>
          <li class="nav-item">
            <a class="navbar-brand" href="{% url 'index' %}">BRAND</a>
          </li>
          <li class="nav-item">
            <a class="navbar-brand" href="{% url 'basic_app:register' %}"
              >Registration</a
            >
          </li>

          {% if user.is_authenticated %}
          <li class="nav-item">
            <a class="navbar-brand" href="{% url 'logout' %}">Logout</a>
          </li>
          {% else %}
          <li class="nav-item">
            <a class="navbar-brand" href="{% url 'basic_app:user_login' %}"
              >Login</a
            >
          </li>
          {% endif %}
        </ul>
      </div>
    </nav>

    <div class="container">{% block body_block %} {% endblock %}</div>
  </body>
</html>
```
