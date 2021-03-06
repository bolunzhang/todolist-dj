<p align="center">
  <img src="./static/media/logo-slim.svg" height="150">
</p>

2do1ist is a todolist app using Django framework. Demo [here](https://2do.pythonanywhere.com/). [![Build Status](https://travis-ci.org/balloonio/2do1ist.svg?branch=master)](https://travis-ci.org/balloonio/2do1ist)

This app covers the following topics and practice:

- Django basics (template, url, view and session)
- How to use Django build-in auth app to implement **login** and **logout**
- How to implement user **signup**
- How to solve **form resubmissions**
- How to use Bootstrap (modal and **popover**)
- How to use **Bootstrap grid system**
- How to **animate svg logo** with anime.js
- How to **animate progress bar** with progressbar.js
- How to send an **AJAX** request and read the response

Table of Content

- [Setup Environment](#setup-environment)
- [Create Django Project](#create-django-project)
- [Create Django App](#create-django-app)
- [URL, View and a Minimal Working Website](#url-view-and-a-minimal-working-website)
- [Bootstrap](#bootstrap)
- [HTML and Template](#html-and-template)
- [Database and Model](#database-and-model)
- [HTTP Form, GET and POST](#http-form-get-and-post)
- [Add/Complete/Delete a Todo](#addcompletedelete-a-todo)
- [Animate SVG Logo](#animate-svg-logo)
- [AJAX Request](#ajax-request)
- [About Deployment](#about-deployment)
- [Reference](#reference)

## Setup Environment

Create Python virtual environment:

```
python3 -m venv myvenv
```

Activate it:

```
source myvenv/bin/activate
```

Install Django:

```
pip3 install django
```

## Create Django Project

Create a Django project:

```
django-admin startproject mysite .
```

Now let’s look at what startproject created:

```
.
├── manage.py
└── mysite
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```

> - manage.py: A command-line utility that lets you interact with this Django project in various ways.
> - The inner mysite/ directory is the actual Python package for your project. Its name is the Python package name you’ll need to use to import anything inside it (e.g. mysite.urls).
> - mysite/__init__.py: An empty file that tells Python that this directory should be considered a Python package.
> - mysite/settings.py: Settings/configuration for this Django project. Django settings will tell you all about how settings work.
> - mysite/urls.py: The URL declarations for this Django project; a “table of contents” of your Django-powered site.
>- mysite/wsgi.py: An entry-point for WSGI-compatible web servers to serve your project.

Let’s verify our Django project works:

```
$ python manage.py runserver
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

February 01, 2019 - 15:50:53
Django version 2.1, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

> Note: Ignore the warning about unapplied database migrations for now.

Now that the server’s running, visit http://127.0.0.1:8000/ with a Web browser. We see a “Congratulations!” page, with a rocket taking off. It worked!

> By default, the runserver command starts the development server on the internal IP at port 8000.

## Create Django App

Now that our environment – a “project” – is set up, let's create our todolist app.

Each application we write in Django consists of a Python package that follows a certain convention. Django comes with a utility that automatically generates the basic directory structure of an app, so we can focus on writing code rather than creating directories.

> **Projects vs. apps**
>
> What’s the difference between a project and an app? An app is a Web application that does something – e.g., a Weblog system, a database of public records or a simple poll app. A project is a collection of configuration and apps for a particular website. A project can contain multiple apps. An app can be in multiple projects.

To create our todolist app, make sure we are in the same directory as manage.py and type this command:

```
python manage.py startapp todolist
```

Now our directory looks like this:

```
.
├── manage.py
├── mysite
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── todolist
    ├── __init__.py
    ├── admin.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    └── views.py

4 directories, 16 files
```

To include the app in our project, we need to add a reference to its configuration class in the INSTALLED_APPS setting:

```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'todolist'
)
```

## URL, View and a Minimal Working Website

As mentioned above, urls.py is like a "table of contents". It is a URLconf. Let's create a "table of contents" urls.py for todolist app. For now, let's copy the comment section from mysite/urls.py because the comment helps explain what we are doing here. Please read the comment yourself.

```python
# ./todolist/urls.py
"""mysite URL Configuration
 The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/2.1/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
```

Let's reference this app URLconf in our project URLconf based on the instructions in the comment:

```python
# ./mysite/urls.py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('todolist.urls')) # add this line (and any import if needed by include)
]
```

Before we complete todolist/urls.py, let's create a view inside todolist/views.py. 

What is a view?

> A view function, or view for short, is simply a Python function that takes a Web request and returns a Web response.

Good! It sounds like exactly what we need:

```python
# ./todolist/views.py
from django.http import HttpResponse
import datetime

def index(request):
    message = "Current Time: {}".format(datetime.datetime.now())
    return HttpResponse(message) 
```

Now it's time to complete todolist/urls.py:

```python
# ./todolist/urls.py
from django.conf.urls import url
from django.urls import path

from todolist import views

urlpatterns = [
    path('', views.index, name='index')
]
```

Let’s verify our code works:

```
python manage.py runserver
```

Now that the server’s running, visit http://127.0.0.1:8000/ with a Web browser. We can see the current time on the page!

## Bootstrap

You can either use Bootstrap CDN or downloard source files to apply Bootstrap. Here we downloard the source files into static directory:

```
.
├── manage.py
├── static
│   ├── css
│   │   └── bootstrap.css
│   ├── fonts
│   └── js
│       ├── bootstrap.js
│       └── jquery-3.3.1.js
├── mysite
│   └── ...
└── todolist
    └── ...
```

To apply Bootstrap, include this into the html file

```html
  <link rel="stylesheet" href="/static/css/bootstrap.css" />
  <script src="/static/js/jquery-3.3.1.js"></script>
  <script src="/static/js/bootstrap.js"></script>
```

> ⚠️ The order of include matters here! ⚠️  

## HTML and Template

Create an empty template directory. Inside it, create a basic html file with Bootstrap style:

```html
<!-- ./template/base.html -->
<!DOCTYPE html>
<html>
<head>
  <title>TodoList</title>
  <link rel="stylesheet" href="/static/css/bootstrap.css" />
  <script src="/static/js/jquery-3.3.1.js"></script>
  <script src="/static/js/bootstrap.js"></script>
</head>
<body>
  <div class="container">
    {% block content %}
    {% endblock %}
  </div>
</body>
</html>
```

> Being a web framework, Django needs a convenient way to generate HTML dynamically. The most common approach relies on templates. A template contains the static parts of the desired HTML output as well as some special syntax describing how dynamic content will be inserted.

As above show, we are using **block and extends** template syntax to quickly apply Bootstrap style to our index.html.

```html
<!-- ./template/index.html -->
{% extends 'base.html' %}
{% block content %}
  <div>
    <h1>TodoList</h1>
  </div>
{% endblock %}
```

For loop and if statement on a dynamic variable returned by backend:

```html
{% for todo in todos %}
    <tr>
        <td>
        {% if todo.completed %}
            <del>{{ todo.title }}</del>
        {% else %}
            {{ todo.title }}
        {% endif %}
        </td>
    </tr>
{% endfor %}
```

> ⚠️ All those will not work unless the templates engines are configured with the TEMPLATES setting. ⚠️  

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            # ... some options here ...
        },
    },
]
```

## Database and Model

> A model is the single, definitive source of information about your data. It contains the essential fields and behaviors of the data you’re storing. Generally, each model maps to a single database table.

Define the model, which is the database object. This step is similar to schema definition in some traditional database workflows.

```python
class Todo(models.Model):
    title = models.CharField(max_length=255)
    description = models.TextField(blank=True)
    completed = models.BooleanField(default=False)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        ordering = ('completed', '-updated_at',)
```

> Model metadata is “anything that’s not a field”, such as ordering options (`ordering`). For `ordering`, aach string is a field name with an optional "-" prefix, which indicates descending order. Fields without a leading "-" will be ordered ascending.

Keep in mind, everytime you modify a model, you need to regenerate migration scripts and migrate:

```
$ python3 manage.py makemigrations
...
$ python3 manage.py migrate
```

## HTTP Form, GET and POST

The `<form>` tag is used to create an HTML form for user input. Any applicable input tags inside the form will be sent over to server side.

A typical login submission form looks like this:

```html
<form action="/login" method="post">
  Name: <input type="text" name="name_field"><br>
  Password: <input type="password" name="password_field"><br>
  <button type="submit">Login</button>
</form>
```

`method` attribute specifies the HTTP method to send the form data:

- GET is used to request data from a specified resource:
    - GET requests can be cached
    - GET requests remain in the browser history
    - GET requests can be bookmarked
    - **GET requests should never be used when dealing with sensitive data**
    - GET requests have length restrictions
    - GET requests is only used to request data (not modify)
    - The query string (name/value pairs) is sent in the URL of a GET request:
        ```
        /test/demo_form.php?name1=value1&name2=value2
        ```

- POST is used to send data to a server to create/update a resource
    - POST requests are never cached
    - POST requests do not remain in the browser history
    - POST requests cannot be bookmarked
    - POST requests have no restrictions on data length
    - The data sent to the server with POST is stored in the request body of the HTTP request:
        ```
        POST /test/demo_form.php HTTP/1.1
        Host: w3schools.com
        name1=value1&name2=value2
        ```

## Add/Complete/Delete a Todo

To avoid form resubmission issue, we use POST/Redirect/GET pattern here. Therefore, each action is being posted to its corresponding url for server processing. Id is also posted for complete and delete actions.

```python
# ./todolist/urls.py
urlpatterns = [
    path('', views.todo_list, name='todo_home'),
    path('add/', views.add),
    path('complete/<int:todo_id>/', views.complete),
    path('delete/<int:todo_id>/', views.delete)
]
```

```python
# ./todolist/views.py
def add(request):
    todoname = request.POST.get('todoname')
    new_todo = Todo.objects.create(title=todoname, user_id=request.user.id)
    return HttpResponseRedirect('/')

def todo_list(request):
    if not request.user.is_authenticated:
        return HttpResponseRedirect('accounts/login/')
    todos = Todo.objects.filter(user_id=request.user.id)
    return render(request, 'index.html', locals())
```

> At a early version of this code, Add Todo action was posted to the same url as the index page. As a result, when you refresh the page right after a new todo item was added, a duplicate todo item would be added. This was caused by duplicate form resubmission. To solve this issue, Post/Redirect/Get was used. Instead of posting to the same url, Add Todo action posts to a dedicated url and server invokes a redirect to index page. Read more [here](https://en.wikipedia.org/wiki/Post/Redirect/Get) and [here](https://realpython.com/django-redirects/#django-redirects-a-super-simple-example).

## Animate SVG Logo

To include a SVG logo inside HTML, simply insert the SVG code, which you can get from Adobe Illustrator when save. It typically looks like this:

```html
<div>
    <?xml version="1.0" encoding="utf-8"?>
    <!-- Generator: Adobe Illustrator 17.1.0, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
    <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
    <svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
        viewBox="-174 126.3 3784 2536.5" enable-background="new -174 126.3 3784 2536.5" xml:space="preserve">
    <g enable-background="new    ">
        <path fill="#FFFFFF" stroke="#231F20" stroke-width="5px" d="M1650.4,1206.5h-2.5l-150.1,71.4l-30.3-138l208.2-96.9h152.6v786.9h-177.9L1650.4,1206.5L1650.4,1206.5z"/>
    </g>
    </svg>
</div>
```

Include `anime.js`:

```html
  <script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/2.2.0/anime.min.js"></script>
```

Animate:

```javascript
let paths = document.querySelectorAll('path');
paths.forEach(el => {
  let pLen = el.getTotalLength();
  el.setAttribute('stroke-dasharray', pLen + ' ' + pLen );
  anime({
    targets: el,
    strokeDashoffset: [anime.setDashoffset, 0],
    easing: 'easeInOutSine',
    duration: 1500,
    delay: function(el, i) { return i * 250 },
    direction: 'alternate',
    loop: true
  });
});
```

## AJAX Request

We have been redirecting users to other url for each todolist action, which is okay but not perfect. Users don't want to see their page being refreshed again and again. That's where AJAX comes into the picture. Let's rework our todolist page to have a textbox inside the list-group. Whenever users type inside the textbox and hit enter, an AJAX version Add action is sent to backend.

To send an AJAX request:

```javascript
let xhr = new XMLHttpRequest();
xhr.onload = function (){
  // this is the ajax callback function
  console.log("Ajax response received!" + xhr.responseText );
  let jsonResponse = JSON.parse(xhr.responseText);
};
xhr.open('POST','/add-ajax/', true );
var csrftoken = $("[name=csrfmiddlewaretoken]").val();
xhr.setRequestHeader("X-CSRFToken", csrftoken);
xhr.setRequestHeader('X-Requested-With', 'XMLHttpRequest');
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
todoTitle = encodeURIComponent(document.getElementsByName('todoname-ajax')[0].value);
xhr.send("todoname="+todoTitle);
```

> ⚠️ `encodeURIComponent()` is required to encode special characters. In addition, it encodes the following characters: `, / ? : @ & = + $ #`. Django requires csrf token to be sent along with POST request; otherwise, Django server will response 403 Forbidden. ⚠️

To process an AJAX request on server side and return a JSON response:

```python
from django.http import JsonResponse

def add_ajax(request):
    todoname = request.POST.get('todoname')
    new_todo = Todo.objects.create(title=todoname, user_id=request.user.id)
    data = { 'created_title' : new_todo.title, 'created_id': new_todo.id }
    return JsonResponse(data)
```

## About Deployment

Add the deployment domain to ALLOWED_HOSTS setting:

```python
ALLOWED_HOSTS = ['2do.pythonanywhere.com']
```

Serve the static/asset directory according to the host service provider's instructions. Usually, this might include running this command:

```
python3 manage.py collectstatic
```

## Reference

[Django Login/Logout Tutorial (Part 1)](https://wsvincent.com/django-user-authentication-tutorial-login-and-logout/)  
[How the Bootstrap 4 Grid Works](https://uxplanet.org/how-the-bootstrap-4-grid-works-a1b04703a3b7)  
[progressbar.js](https://kimmobrunfeldt.github.io/progressbar.js/)  
[anime.js](https://animejs.com/)  
[How SVG Line Animation Works](https://css-tricks.com/svg-line-animation-works/)  
[Metamorphosis: morphing SVGs](https://hackernoon.com/metamorphosis-morphing-svgs-378cf4f3aa58)  
[How to Work With AJAX Request With Django](https://simpleisbetterthancomplex.com/tutorial/2016/08/29/how-to-work-with-ajax-request-with-django.html)  
[AJAX - Send a Request To a Server](https://www.w3schools.com/js/js_ajax_http_send.asp)  
[Cross Site Request Forgery protection - Ajax](https://docs.djangoproject.com/en/dev/ref/csrf/#ajax)  