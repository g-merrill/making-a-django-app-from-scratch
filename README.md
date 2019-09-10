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
* after typing the following, check that the project is up and running on localhost:8000
```
$ python3 manage.py runserver
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
* set up **main_app**'s own **urls.py**
```
$ touch main_app/urls.py
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
* create your database
```
$ createdb <yourdatabasename>
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
* to begin rendering your data in your web app, you should set up your model first
* in the **main_app**'s **models.py** file, create a basic model for your data entity
```
class Yourmodel(models.Model):
    name = models.CharField(max_length=100)
    species = models.CharField(max_length=100)
    description = models.TextField(max_length=250)
    age = models.IntegerField()

    def __str__(self):
        return f'{self.name} ({self.id})'
```
* remember that anytime you add or change the model schema in this file, you have to make migrations and then migrate
* to do so for this first time set up, type the following in the CLI
```
$ python3 manage.py makemigrations
$ python3 manage.py migrate
```
* the easiest way to CRUD your data entities is as a Django admin!
```
$ python3 manage.py createsuperuser
```
* if you press enter, you can use the default username, and also skip the email address
* you must create and confirm a password (the CLI wont show the password characters as you type them)
* register the model that you just made, by going to the **main_app/admin.py** file and adding
```
from django.contrib import admin
# import your models here
from .models import Yourmodel

# Register your models here
admin.site.register(Yourmodel)
```
* go to localhost:8000/admin to log in to the awesome Django admin portal and add some data!
* once you've entered some data in to your database, start to create an index view 
* add a **View All Yourdataentities** link to the navigation bar in **base.html**
```
<li><a href="/about">About</a></li>
<!-- new markup below -->
<li><a href="/yourdataentities">View All Yourdataentities</a></li>
```
* add the new route to **main_app/urls.py**
```
urlpatterns = [
    path('', views.home, name='home'),
    path('about/', views.about, name='about'),
    # route for yourdataentities index
    path('yourdataentities/', views.yourdataentities_index, name='index'),
]
```
* import the Model you created into **views.py** and type the yourdataentities_index function
```
from django.shortcuts import render
# import the model here
from .models import Yourmodel

...

# define the index view
def yourdataentities_index(request):
    yourdataentities = Yourmodel.objects.all()
    return render(request, 'yourdataentities/index.html', { 'yourdataentities': yourdataentities })
```
* add the corresponding templates
```
$ mkdir main_app/templates/yourdataentities
$ touch main_app/templates/yourdataentities/index.html
```
* in **yourdataentities/index.html**, add
```
{% extends 'base.html' %}
{% block content %}

<h1>Yourdataentities List</h1>

{% for yourdataentity in yourdataentities %}
<div class="card">
    <a href="{% url 'detail' yourdataentity.id %}">
        <div class="card-content">
            <span class="card-title">{{ yourdataentity.name }}</span>
        </div>
    </a>
</div>
{% endfor %}

{% endblock %}
```
* now add the 'detail' route, since you preemptively referenced it in your index template 
* in **urls.py**, add
```
urlpatterns = [
    ...
    path('yourdataentities/<int:yourdataentity_id>/', views.yourdataentities_detail, name='detail'),
]
```
* now to define the detail function in **views.py**
```
...
def yourdataentities_detail(request, yourdataentity_id):
    yourdataentity = Yourmodel.objects.get(id=yourdataentity_id)
    return render(request, 'yourdataentities/detail.html', { 'yourdataentity': yourdataentity })
```
* create the template called **detail.html**
```
$ touch main_app/templates/yourdataentities/detail.html
```
* inside **yourdataentities/detail.html** add the following template code
```
{% extends 'base.html' %}
{% block content %}

<h1>Yourdataentity Details</h1>

<div class="card">
    <div class="card-content">
        <span class="card-title">{{ yourdataentity.name }}</span>
        <p>Species: {{ yourdataentity.species }}</p>
        <p>Description: {{ yourdataentity.description }}</p>
        <p>Age: {{ yourdataentity.age }}</p>
    </div>
</div>

{% endblock %}
```