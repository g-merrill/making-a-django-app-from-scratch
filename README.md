* create your database
```
$ createdb <yourdatabasename>
```
* cd into the local containing folder where you wish to create your app
* initialize a Django project folder
```
$ django-admin startproject <yourprojectname>
```
* then
```
$ cd <yourprojectname>
$ code .
```
* still in the CLI, create a Django app that will implement the main functionality
```
$ python3 manage.py startapp main_app
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
* in the project's __*>yourprojectname<*/urls.py__
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
