# Django Official Docs Project

- [Django Official Docs Project](#django-official-docs-project)
  - [0. Quick Install](#0-quick-install)
    - [Install Python](#install-python)
    - [Set up a database](#set-up-a-database)
    - [Install Django](#install-django)
      - [Verifying](#verifying)
  - [1. Writing first Django app](#1-writing-first-django-app)
    - [Creating a project](#creating-a-project)
    - [Development Server](#development-server)
      - [Changing the port](#changing-the-port)
    - [Creating the Polls app](#creating-the-polls-app)
      - [Project vs apps](#project-vs-apps)
    - [Write your first view](#write-your-first-view)
      - [When to use include()](#when-to-use-include)
      - [path() argument: route](#path-argument-route)
      - [path() argument: view](#path-argument-view)
      - [path() argument: kwargs](#path-argument-kwargs)
      - [path() argument: name](#path-argument-name)
  - [2. Working with the database](#2-working-with-the-database)
    - [Database setup](#database-setup)
    - [Creating models](#creating-models)

## 0. Quick Install

Before you can use Django, you’ll need to get it installed.

### Install Python

- Being a Python web framework, Django requires Python.
- Python includes a lightweight database called SQLite so you won’t need to set up a database just yet.
- Get the latest version of Python at [python.org/downloads/](https://www.python.org/downloads/) or with your operating system’s package manager.

__What Python version should I use with Django?__

- Since newer versions of Python are often faster, have more features, and are better supported, the latest version of Python 3 is recommended.
- You don’t lose anything in Django by using an older release, but you don’t take advantage of the improvements and optimizations in newer Python releases. Third-party applications for use with Django are free to set their own version requirements.

You can verify that Python is installed by typing `python` from your shell;

```py
python
python -V
python --version
```
you should see something like:
```shell
Python 3.x.y
[GCC 4.x] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

### Set up a database

- This step is only necessary if you’d like to work with a “large” database engine like __PostgreSQL__, __MariaDB__, __MySQL__, or __Oracle__.

### Install Django

```shell
pip install django

# or

python -m pip install django
```

__pip version:__
```py
pip --version
```

#### Verifying

To verify that Django can be seen by Python, type __python__ from your shell. Then at the Python prompt, try to import Django:

```py
import django as dj
print(dj.get_version())

# or

python -m django --version
```

- If Django is installed, you should see the version of your installation. If it isn’t, you’ll get an error telling “No module named django”.

## 1. Writing first Django app

- Let's learn by creating a project.
- Throughout this tutorial, we’ll walk you through the creation of a basic poll application.
- It'll consist two parts:
  - A public site that lets people view polls and vote in them.
  - An admin site that lets you add, change, and delete polls.

### Creating a project

- If this is your first time using Django, you’ll have to take care of some initial setup. Namely, you’ll need to auto-generate some code that establishes a Django project – a collection of settings for an instance of Django, including database configuration, Django-specific options and application-specific settings.
- From the command line, __cd__ into a directory where you’d like to store your code, then run the following command:

```py
django-admin startproject mysite
```
- This will create a __mysite__ directory in your current directory.
- __Note:__ You’ll need to avoid naming projects after built-in Python or Django components. In particular, this means you should avoid using names like __django__ (which will conflict with Django itself) or __test__ (which conflicts with a built-in Python package).

Let’s look at what __startproject__ created:

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

There files are:

- The outer `mysite/` root directory is a container for your project. Its name doesn’t matter to Django; you can rename it to anything you like.
- __`manage.py`__: A command-line utility that lets you interact with this Django project in various ways.
- The inner __mysite/__ directory is the actual Python package for your project. Its name is the Python package name you’ll need to use to import anything inside it (e.g. `mysite.urls`).
- __`mysite/__init__.py`__: An empty file that tells Python that this directory should be considered a Python package.
- __mysite/settings.py__: Settings/configuration for this Django project. Django settings will tell you all about how settings work.
- __mysite/urls.py__: The URL declarations for this Django project; a “table of contents” of your Django-powered site.
- __mysite/asgi.py__: An entry-point for ASGI-compatible web servers to serve your project.
- __mysite/wsgi.py__: An entry-point for WSGI-compatible web servers to serve your project.

### Development Server

Let’s verify your Django project works. Change into the outer __mysite__ directory, if you haven’t already, and run the following commands:

```shell
python manage.py runserver
```

You’ll see the following output on the command line:

```
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

January 26, 2023 - 15:50:53
Django version 4.1, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
- Ignore the warning about unapplied database migrations for now; we’ll deal with the database shortly.
- You’ve started the Django development server, a lightweight web server written purely in Python. We’ve included this with Django so you can develop things rapidly, without having to deal with configuring a production server – such as Apache – until you’re ready for production.
- Now’s a good time to note: __don’t__ use this server in anything resembling a production environment. It’s intended only for use while developing. (We’re in the business of making web frameworks, not web servers.)

Now that the server’s running, visit [http://127.0.0.1:8000/](http://127.0.0.1:8000/) with your web browser. You’ll see a “Congratulations!” page, with a rocket taking off. It worked!

#### Changing the port

- By default, the __runserver__ command starts the development server on the internal IP at port `8000`.
- If you want to change the server’s port, pass it as a command-line argument. For instance, this command starts the server on port 8080:

```shell
python manage.py runserver 8080
```

If you want to change the server’s IP, pass it along with the port. For example, to listen on all available public IPs (which is useful if you are running Vagrant or want to show off your work on other computers on the network), use:

```shell
py manage.py runserver 0.0.0.0:8000
```

__Automatic reloading of runserver:__
- The development server automatically reloads Python code for each request as needed. You don’t need to restart the server for code changes to take effect. However, some actions like adding files don’t trigger a restart, so you’ll have to restart the server in these cases. For Ex: in changing setting you need to restart runserver.

### Creating the Polls app

Now that your environment – a “project” – is set up, you’re set to start doing work.

- Each application you write in Django consists of a Python package that follows a certain convention. Django comes with a utility that automatically generates the basic directory structure of an app, so you can focus on writing code rather than creating directories.

#### Project vs apps

What’s the difference between a project and an app?

- An app is a web application that does something – e.g., a blog system, a database of public records or a small poll app.
- A project is a collection of configuration and apps for a particular website. A project can contain multiple apps. An app can be in multiple projects.

Your apps can live anywhere on your Python path. In this tutorial, we’ll create our poll app in the same directory as your __manage.py__ file so that it can be imported as its own top-level module, rather than a submodule of __mysite__.

To create your app, make sure you’re in the same directory as __manage.py__ and type this command:


```shell
python manage.py startapp polls
```
- That’ll create a directory __polls__, which is laid out like this:

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

- This directory structure will house the poll application.

### Write your first view

Let’s write the first view. Open the file __`polls/views.py`__ and put the following Python code in it:

```py
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

- This is the simplest view possible in Django. To call the view, we need to map it to a URL - and for this we need a URLconf.

To create a URLconf in the polls directory, create a file called __`urls.py`__. Your app directory should now look like:

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```

In the __`polls/urls.py`__ file include the following code:

```py
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

The next step is to point the root URLconf at the __`polls.urls`__ module. In __`mysite/urls.py`__, add an import for `django.urls.include` and insert an `include()` in the __urlpatterns__ list, so you have:

```py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

- The __`include()`__ function allows referencing other URLconfs. Whenever Django encounters __`include()`__, it chops off whatever part of the URL matched up to that point and sends the remaining string to the included URLconf for further processing.
- The idea behind __`include()`__ is to make it easy to plug-and-play URLs. Since polls are in their own URLconf (polls/urls.py), they can be placed under “/polls/”, or under “/fun_polls/”, or under “/content/polls/”, or any other path root, and the app will still work.

#### When to use include()

You should always use `include()` when you include other URL patterns. __`admin.site.urls`__ is the only exception to this.

You have now wired an index view into the URLconf. Verify it’s working with the following command:

```shell
py manage.py runserver
```
- Go to [http://localhost:8000/polls/](http://localhost:8000/polls/) in your browser, and you should see the text “`Hello, world. You’re at the polls index.`”, which you defined in the __index__ view.

Page not found?
- If you get an error page here, check that you’re going to [http://localhost:8000/polls/](http://localhost:8000/polls/) and not `http://localhost:8000/`.

The __path()__ function is passed four arguments, two required: __route__ and __view__, and two optional: __kwargs__, and __name__. At this point, it’s worth reviewing what these arguments are for.

#### path() argument: route

__route__ is a string that contains a URL pattern. When processing a request, Django starts at the first pattern in __urlpatterns__ and makes its way down the list, comparing the requested URL against each pattern until it finds one that matches.

Patterns don’t search GET and POST parameters, or the domain name. For example, in a request to
- __`https://www.example.com/myapp/`__, the URLconf will look for __myapp/__. In a request to
- __`https://www.example.com/myapp/?page=3`__, the URLconf will also look for __myapp/__.

#### path() argument: view

When Django finds a matching pattern, it calls the specified view function with an __HttpRequest__ object as the first argument and any “captured” values from the route as keyword arguments. We’ll give an example of this in a bit.

#### path() argument: kwargs

Arbitrary keyword arguments can be passed in a dictionary to the target view. We aren’t going to use this feature of Django in the tutorial.

#### path() argument: name

- Naming your URL lets you refer to it unambiguously from elsewhere in Django, especially from within templates. This powerful feature allows you to make global changes to the URL patterns of your project while only touching a single file.

## 2. Working with the database

We’ll set up the database, create your first model, and get a quick introduction to Django’s automatically-generated admin site.

### Database setup

Now, open up __`mysite/settings.py`__. It’s a normal Python module with module-level variables representing Django settings.

- By default, the configuration uses __SQLite__. If you’re new to databases, or you’re just interested in trying Django, this is the easiest choice. SQLite is included in Python, so you won’t need to install anything else to support your database. When starting your first real project, however, you may want to use a more scalable database like PostgreSQL, to avoid database-switching headaches down the road.

- While you’re editing __`mysite/settings.py`__, set __TIME_ZONE__ to your time zone.
- Also, note the __INSTALLED_APPS__ setting at the top of the file. That holds the names of all Django applications that are activated in this Django instance. Apps can be used in multiple projects, and you can package and distribute them for use by others in their projects.

By default, __INSTALLED_APPS__ contains the following apps, all of which come with Django:

- __`django.contrib.admin`__ – The admin site. You’ll use it shortly.
- __`django.contrib.auth`__ – An authentication system.
- __`django.contrib.contenttypes`__ – A framework for content types.
- __`django.contrib.sessions`__ – A session framework.
- __`django.contrib.messages`__ – A messaging framework.
- __`django.contrib.staticfiles`__ – A framework for managing static files.

These applications are included by default as a convenience for the common case.

Some of these applications make use of at least one database table, though, so we need to create the tables in the database before we can use them. To do that, run the following command:

```shell
python manage.py migrate
```

- The __migrate__ command looks at the __INSTALLED_APPS__ setting and creates any necessary database tables according to the database settings in your __`mysite/settings.py`__ file and the database migrations shipped with the app (we’ll cover those later). You’ll see a message for each migration it applies.
- If you’re interested, run the command-line client for your database and type `.tables` (SQLite) to display the tables Django created.

__For the minimalists:__

Like we said above, the default applications are included for the common case, but not everybody needs them. If you don’t need any or all of them, feel free to comment-out or delete the appropriate line(s) from __INSTALLED_APPS__ before running __migrate__. The __migrate__ command will only run migrations for apps in __INSTALLED_APPS__.

### Creating models

Now we’ll define your models – essentially, your database layout, with additional metadata.




