# Django Databases

To use database in Django, we need to make configuration in `settings.py` file.

Here's the snippet of the default configuration that comes with initialized 

```python
# Database
# https://docs.djangoproject.com/en/4.0/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

```

Note that we have default database set to `sqlite3`, but we can use any other database such as `oracle`, `mysql`, `postgresql` etc

Here's the example of how to use `postgresql` with Django

```python

DATABASES = {
   'default': {
       'ENGINE': 'django.db.backends.postgresql',
       'NAME': '<database_name>',
       'USER': '<database_username>',
       'PASSWORD': '<password>',
       'HOST': '<database_hostname_or_ip>',
       'PORT': '<database_port>',
   }
}

```

Notice the change in `ENGINE`, it is set to postgresql. For any other DBs such as `mysql` or `oracle` the configuration looks the same but the `ENGINE` value changes.

`ENGINE` values for Different Databases:

    'django.db.backends.postgresql'
    'django.db.backends.mysql'
    'django.db.backends.sqlite3'
    'django.db.backends.oracle'


💡 To see the correct time for your database entries, make sure to enter your timezone. Here's the example:
```python
# settings.py

TIME_ZONE = 'Asia/Kolkata'
```

Now that we have configured our desired Databases, it's time to write some models(Tables) inside `models.py`.

Before that, we need to make `superuser` for Django Admin Panel. Django Admin Panel is something Django gives out of the box.

To configure `superuser` for `Django Admin Panel`, Run the following command,

```bash
python manage.py createsuperuser
```

Now that the Django Admin Panel user is created, you can login to the Django admin panel by entering following url:
`127.0.0.1/admin` 

you'll see something like below

![image](https://user-images.githubusercontent.com/31511537/168466858-97ad16d0-6608-4b9f-aad5-10d952b75cea.png)

Here `Groups` and `User` are default tables/models created by Django.

💡 You won't find these table if you did not run the following commands,
```bash
python manage.py makemigrations
python manage.py migrate
```