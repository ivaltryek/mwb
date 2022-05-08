# Create App for your Django Project.

Here `App` could also be considered as an module for your project, for example a module could be Login or Register for your website. Another example for it is Cart for you shopping website.

## Creating an App 
```bash
python -m django startapp myapp
```

That’ll create a directory `myapp`, which is laid out like this:
```bash
myapp/
    __init__.py # An empty file that tells Python that this directory should be considered a Python package.
    admin.py # The admin.py file is used to display your models in the Django admin panel. You can also customize your admin panel.
    apps.py # This file is created to help the user include any application configuration for the app. Using this, you can configure some of the attributes of the application.
    migrations/ # These are for Database migrations.
        __init__.py
    models.py # This file is created to store Database models (Tables) configurations.
    tests.py # This file is created to write testcases for the app.
    views.py # This file is created to write application logic.

```

## Writing the views
Views can be written inside `views.py` file.
Here's an example of view that simply returns HTTP Response
```python
# myapp/views.py

# Django utility to send Http response
from django.http import HttpResponse

# HttpReponse: Typical usage is to pass the contents of the page, as a string, bytestring, or memoryview, to the HttpResponse constructor.

def index(request):
    return HttpResponse("This is my first Django App!")
```

> To be able to call this view, we need to map it to the URl, for that we need to create `urls.py`.

### Updated App Directory structure.

```bash
myapp/
    __init__.py # An empty file that tells Python that this directory should be considered a Python package.
    admin.py # The admin.py file is used to display your models in the Django admin panel. You can also customize your admin panel.
    apps.py # This file is created to help the user include any application configuration for the app. Using this, you can configure some of the attributes of the application.
    migrations/ # These are for Database migrations.
        __init__.py
    models.py # This file is created to store Database models (Tables) configurations.
    tests.py # This file is created to write testcases for the app.
    views.py # This file is created to write application logic.
    urls.py # This file is created to map views to URLs.

```
### Mapping Views to URL
```python
# myapp/urls.py

# Django Utility to map index with URL.
from django.urls import path

'''
   path(route, view, kwargs=None, name=None)
    
   Parameters:
   route: Define the route to show the view, i.e index, about etc
   view: View to map with the route. 
'''
# Import views from `views.py`
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]

```

The next step is to point our all module/app URLs to the Project's `urls.py` file.
```python
# webapp/urls.py

from django.contrib import admin # This is for admin site urls.
from django.urls import include, path

'''
   include(module, namespace=None)
   include(pattern_list)
   include((pattern_list, app_namespace), namespace=None)

   A function that takes a full Python import path to another URLconf module that should be “included” in this place. Optionally, the application namespace and instance namespace where the entries will be included into can also be specified.

   Usually, the application namespace should be specified by the included module. If an application namespace is set, the namespace argument can be used to set a different instance namespace. 

   include() also accepts as an argument either an iterable that returns URL patterns or a 2-tuple containing such iterable plus the names of the application namespaces.

   Parameters:
   module – URLconf module (or module name)
   namespace (str) – Instance namespace for the URL entries being included
   pattern_list – Iterable of path() and/or re_path() instances.
   app_namespace (str) – Application namespace for the URL entries being included

💡 When to use include():
   You should always use include() when you include other URL patterns. admin.site.urls is the only exception to this.

'''

urlpatterns = [
    path('myapp/', include('polls.urls')), 
    path('admin/', admin.site.urls),
]

```

## Letting Django know that `myapp` is exists in the Directory

```python
# webapp/settings.py

# Find the below block and mention your `app` name.


# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp.apps.MyAppConfig',
    ]

```


Now if you start your server using ```python -m django manage.py runserver```, Browse ```127.0.0.1/myapp```, You'll see the output whatever you have written in your view.



## Diagram for more Information
![DjangoURLs drawio](https://user-images.githubusercontent.com/31511537/167290430-2f4f2144-21f1-4892-8821-d818c21a421e.png)