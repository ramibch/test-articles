# Group your Django apps in a directory

The way you structure your Django project is important for maintainability, specially when the number of apps are growing and they get mixed with other directories such as the media and static development dirs, any development tool folders, the scripting one...


When I have more than 3/4 apps in a project I use to create a common folder for them and group them as follows:


```sh
apps/
  ├── app1/
  │   ├── migrations
  │   ├── __init__.py
  │   ├── apps.py
  │   ├── models.py
  │   ├── views.py
  │   └── tests.py
  ├── app2/
  ├── app3/
  ├── app4/
  └── __init__.py
```


The advantage of doing that is it makes it easier to browse to a app's code. At least for me.

When doing that there some stuff to consider.

__1. First__

Make sure you create the  `__init__.py`, so that the `apps` folder is treated as a python module.


__2. Second thing__

The app label for django needs to be specified like in the snippet below:

```python
# apps/app1/apps.py
# or apps/app1/__init__.py

class App1Config(AppConfig):
    name = "apps.app1"
```

> Note that you can actually create your AppConfig subclass under your app's `__init__.py` file



__3. Last__

As you guessed it the installed app setting needs to be a little different

```python
# config/settings.py

INSTALLED_APPS = [
    ...
    # My apps
    "apps.app1.apps.App1Config", # or apps.app1
    "apps.app2.apps.App2Config", # or apps.app2
    "apps.app3.apps.App3Config", # or apps.app3
    "apps.app4.apps.App4Config", # or apps.app4
    ...
]
```

I have never wrote many time the word apps!
