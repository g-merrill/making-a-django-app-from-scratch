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

