# Web App Checkpoint 5 - Twitter Bootstrap Layout

> FYI: [Reference Code](https://github.com/s2t2/daily-briefings-py/pull/1/commits/1991d4670b03a829390f206cc9892266fe0d7ad5)

Right now we have consistent navigation and footer, but our site could use some style upgrades. Let's leverage the capabilities of the Twitter Bootstrap front-end framework.

Instead of inheriting our views from the "layout.html" file, we're going to modify them to inherit from a new layout called "boostrap_layout.html".

Updated "web_app/templates/home.html":

```html
{% extends "bootstrap_layout.html" %}
{% set active_page = "home" %}

{% block content %}

    <h1>This is a heading</h1>

    <p>This is a paragraph text</p>

    <ul>
        <li>First list item</li>
        <li>Second list item</li>
        <li>Third list item</li>
    </ul>

{% endblock %}
```

Updated "web_app/templates/about.html":

```html
{% extends "bootstrap_layout.html" %}
{% set active_page = "about" %}

{% block content %}

    <h1>About Me</h1>

    <p>todo write some stuff here</p>

{% endblock %}
```

New layout called "web_app/templates/bootstrap_layout.html":

```html
<!doctype html>
<html>
  <head>
    {% block title %}
      <title>My Starter Web App | Helps students learn how to use the Flask Python package.</title>
    {% endblock %}

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
  </head>

  <body>

    <!-- FLASH MESSAGING -->
    <!-- see: http://flask.pocoo.org/docs/1.0/patterns/flashing/#flashing-with-categories -->
    <!-- see: https://getbootstrap.com/docs/4.3/components/alerts/ -->
    {% with messages = get_flashed_messages(with_categories=true) %}
      {% if messages %}
        {% for category, message in messages %}
          <div class="alert alert-{{ category }} alert-dismissible" role="alert" style="margin-bottom:0px;">
            <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
            {{ message }}
          </div>
        {% endfor %}
      {% endif %}
    {% endwith %}

    <!-- SITE NAVIGATION -->
    <!-- see: https://jinja.palletsprojects.com/en/2.11.x/tricks/ -->
    <!-- see: https://getbootstrap.com/docs/4.0/components/navbar/ -->
    {% set nav_links = [
      ('/about', 'about', 'About'),
      ('/users/new', 'new_user', 'Sign Up')
    ] -%}
    {% set active_page = active_page|default('index') -%}
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
      <a class="navbar-brand" href="/">My Web App</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <ul class="nav navbar-nav ml-auto">
          {% for href, id, link_text in nav_links %}
            {% if id == active_page %}
              {% set is_active = "active" -%}
            {% else %}
              {% set is_active = "" -%}
            {% endif %}
            <li class="nav-item">
              <a class="nav-link {{ is_active }}" href="{{href}}">{{link_text}}</a>
            </li>
          {% endfor %}
        </div>
      </ul>
    </nav>


    <div class="container" style="margin-top:2em;">
      <!-- PAGE CONTENTS -->
      <div id="content">
        {% block content %}
        {% endblock %}
      </div>

      <!-- FOOTER -->
      <div id="footer">
        <hr>
        &copy; Copyright 2019 Prof. MJ Rossetti |
        <a href="https://github.com/prof-rossetti/">source</a>
      </div>
    </div>

    <!-- JAVASCRIPT SECTION -->
    <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
    <script type="text/javascript">

        console.log("Thanks for the page visit!")

        // closes data-dismiss="alert" flash messages
        // see: https://getbootstrap.com/docs/4.3/components/alerts/#javascript-behavior
        //$().alert('close')

    </script>
  </body>
</html>

```

There's a lot going on in this file. But we still have the same "content" block which will allow the individual views to insert custom content.

Restart the server and view the app in the browser to see the new styles with a blue responsive navbar at the top!