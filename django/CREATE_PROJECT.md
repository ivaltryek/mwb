# Create Project in Django

## Run the following command to initialise the Django Project.
```bash
python -m django startproject webapp
```

## Details about files created by the `startproject` command.
```bash
webapp/
    manage.py # CLI Utility for a Django project tasks, i.e starting the server and running migrations etc. 
    webapp/
        __init__.py #  An empty file that tells Python that this directory should be considered a Python package.
        settings.py # settings/configuration for this Django project. Django settings will tell you all about how settings work.
        urls.py # The URL declarations for this Django project; a “table of contents” of your Django-powered site.
        asgi.py # An entry-point for ASGI-compatible web servers to serve your project.
        wsgi.py # An entry-point for WSGI-compatible web servers to serve your project. 

```

## Running the Django Development Server
```bash
python manage.py runserver
```

## Running the Django Development Server on Different Port.
```bash
python manage.py runserver 8080
```