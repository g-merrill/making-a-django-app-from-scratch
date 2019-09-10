* create your database
```
$ createdb <yourdatabasename>
```
* cd into the local containing folder where you wish to create your app
* initialize a Django project folder
```
$ django-admin startproject <yourprojectname>
```
* create a Django app that will implement the main functionality and then open the project in VS Code
```
$ cd <yourprojectname>
$ python3 manage.py startapp main_app
$ code .
```
* now, inside VS Code in **settings.py**, add main_app to the INSTALLED_APPS list
```
INSTALLED_APPS = [
	'main_app',
	'django.contrib.admin',
	'django.contrib.auth',
	'django.contrib.contenttypes',
	'django.contrib.sessions',
	'django.contrib.messages',
	'django.contrib.staticfiles',
]
```
* check that the project is up and running
```
$ python3 manage.py runserver
```
* in **settings.py**, change the DATABASES dictionary default ENGINE to postgreSQL and update NAME
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': '<yourdatabasename>',
    }
}
```
* the following will test the database connection and get rid of the red unapplied migration message
```
$ python3 manage.py migrate
```
* set up **main_app**'s own **urls.py**
```
$ touch main_app/urls.py
```
* in the project's __*yourprojectname*/urls.py__
```
from django.contrib import admin
# Add the include function to the import
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    # In this case '' represents the root route
    path('', include('main_app.urls')),
]
```
* in **main_app/urls.py**, add the following boilerplate for now
```
from django.urls import path
from . import views

urlpatterns = [

]
```
* to set up styling with CSS, do the following
```
$ mkdir main_app/static
$ mkdir main_app/static/css
$ touch main_app/static/css/style.css
```
* add the following boilerplate CSS to **style.css**
```
body {
    display: flex;
    min-height: 100vh;
    flex-direction: column;
}

main {
    flex: 1 0 auto;
}

footer {
    padding-top: 0;
    text-align: right;
}
```
* create the templates folder for main_app
```
$ mkdir main_app/templates
```
* add the **base.html** template
```
$ touch main_app/templates/base.html
```
* copy the following boilerplate Django Template Language (DTL) into **base.html**
```
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Your App Name</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">
    <link rel="stylesheet" type="text/css" href="{% static 'css/style.css' %}">
</head>
<body>
    <header class="navbar-fixed">
        <nav>
            <div class="nav-wrapper">
                <ul>
                    <li><a href="/" class="left brand-logo">&nbsp;&nbsp;YourAppName</a></li>
                </ul>
                <ul class="right">
                    <li><a href="/about">About</a></li>
                </ul>
            </div>
        </nav>
    </header>
    <main class="container">
        {% block content %}
        {% endblock %}
    </main>
    <footer class="page-footer">
        <div class="right">All Rights Reserved, &copy; 2019 Your App Name &nbsp;</div>
    </footer>
</body>
</html>
```
* add the **home.html** template
```
$ touch main_app/templates/home.html
```
* now add the following to **home.html**, making use of Django's template inheritance
```
{% extends 'base.html' %}
{% block content %}

<h1>Your App Name</h1>
<hr />
<p>Welcome to the Your App Name home page</p>

{% endblock %}
```
* in **main_app/urls.py**, start building your first route, which will be the home page
```
urlpatterns = [
    # Add the home page path
    path('', views.home, name='home')
]
```
* define the home function below your import line in **main_app/views.py**
```
# Define the home view
def home(request):
    return render(request, 'home.html')
```
* repeat the last few steps to create the About page
* add the **about.html** template
```
$ touch main_app/templates/about.html
```
* now add the following to **about.html**
```
{% extends 'base.html' %}
{% block content %}

<h1>About</h1>
<hr />
<p>Welcome to the Your App Name about page</p>

{% endblock %}
```
* in **main_app/urls.py**, start building the about page route
```
urlpatterns = [
    path('', views.home, name='home'),
    # Add the about page path
    path('about/', views.about, name='about'),
]
```
* define the about function below your import line in **main_app/views.py**
```
# Define the about view
def about(request):
    return render(request, 'about.html')
```
* to begin rendering your data in your web app, you should set up your model first

