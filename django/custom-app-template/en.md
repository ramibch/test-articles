# Create a django custom app template

## Intro

I was building my own Django project template and I asked myself if it would be possible to create a custom app template for my apps when running the command

```sh
python manage.py startapp
```

It turned out, obviously, it was possible.

The goal is to get the following file structure for a new created django app:

```sh
myapp/
├── admin.py
├── apps.py
├── __init__.py
├── migrations
│   └── __init__.py
├── models.py
├── signals.py
├── templates
├── tests.py
├── urls.py
└── views.py
```

How can we do that? Let me explain it here shortly.

## Copy first

Django has its own app*template located at \_django/conf/app_template*.

So just copy that _app_template_ folder into your working directory.

```sh
cp -r venv/lib/python3.8/site-packages/django/conf/app_template/ app_template/
```

In my case, I am using a Linux distro (Ubuntu 22.04). This means I can use the copy command `cp -r` to copy the folder from my virtual environment _venv_ to my current working directory.

## Custom files or folders

Once you have your _app_template_ folder in your working directory, you could:

- Modify the content of the _\*.py-tpl_
- Add more files, such as _signals.py-tpl_ or _urls.py-tpl_
- Add folders. For instance _app_template/templates_ for your HTML templates or _app_template/static_ for your static files (JS, css, images).

I prefer deleting what I don't need than adding what I need. Therefore, I like to add some files to the _app_template_ folder that normally I need when a new app is created.

Here are some.

Adding _urls.py-tpl_ with the imports and the variable `urlpatterns` will save some seconds, or it will keep you away from frustration because of repetitive tasks.

```python
# urls.py-tpl

from __future__ import annotations

from django.urls import path

urlpatterns = [

]

```

And I also created the _signals.py-tpl_ file with a modification on the file _apps.py-tpl_

```python
# signals.py-tpl

from __future__ import annotations
```

```python
# apps.py-tpl
from __future__ import annotations

from django.apps import AppConfig

class {{ camel_case_app_name }}Config(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = '{{ app_name }}'

    def ready(self):
        from . import signals  # noqa
```

An example of an _app_template_ folder structure:

```sh
app_template/
├── admin.py-tpl
├── apps.py-tpl
├── __init__.py-tpl
├── migrations
│   └── __init__.py-tpl
├── models.py-tpl
├── signals.py-tpl
├── templates
├── tests.py-tpl
├── urls.py-tpl
└── views.py-tpl
```

## Run the command now

Now just run the command but point out to your custom _app_template_ folder specifying it on the `--template` argument:

```sh
python manage.py startapp myapp --template=app_template
```

Check out your app now!

```sh
myapp/
├── admin.py
├── apps.py
├── __init__.py
├── migrations
│   └── __init__.py
├── models.py
├── signals.py
├── templates
├── tests.py
├── urls.py
└── views.py
```

## Source code

You can find the whole source code under my GitHub account:

💻 [github.com/ramiboutas/my-django-project-template/](https://github.com/ramiboutas/my-django-project-template/)
