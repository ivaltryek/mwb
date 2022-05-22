# Register and Login in Django Views

This example shows you how to basically fetch the database and store the values using `views` and `templates`.

- Create the app called `authentication` 
  
    ```bash 
     python manage.py startapp authentication 
     
    ```

- `Register` the app in `settings.py`, so that Django could be aware that app named `authentication` exists inside the project
    ```python
    # settings.py

    # Application definition

    INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'authentication.apps.AuthenticationConfig'
    ]
   ```

- Make changes inside `authentication`'s `models.py` file. I have created basic `User` Model.

    ```python
    # authentication/models.py

    from django.db import models

    # Create your models here.

    class User(models.Model):
        user_name = models.CharField(max_length=20, unique=True)
        user_email = models.EmailField()
        user_password = models.CharField(max_length=50)

        def __str__(self) -> str:
            return self.user_name

    ```

- Now that `model` is defined, `run` the `migrations`

    ```bash
    python manage.py makemigrations
    python manage.py migrate authentication
    ```

- Create `templates` folder inside `authentication` and create file `register.html` inside `authentication/templates` folder.

    ```html
    <!-- authentication/templates/register.html -->

    <form action="" method="post">
    <!-- To avoid XSS attacks -->
    {% csrf_token %}
    <!-- Display the form -->
    <!-- as_p: display the form as paragraph. -->
    {{ form.as_p }}
    <input type="submit" value="Submit">
    </form>
    ```

- Now you might have noticed, that what is the `form` here, it is `context` passed from the view. But to create forms, we'll have to create file `forms.py`

    ```python
    # authentication/forms.py

    from django import forms
    from .models import User

    class RegisterUser(forms.ModelForm):
        # Makes password written in hidden i.e ******
        # If you don't mention widget=forms.PasswordInput(), password will be visible. i.e mypassword
        user_password = forms.CharField(widget=forms.PasswordInput())

        # Meta class used to defined the metadata of the model, such as it's field and which model to use in this form.
        class Meta:
            model = User
            fields = ('user_name', 'user_email', 'user_password')
        
    ```

- Now that all the required componets are configured, It is time to make a `view`.

    ```python
    # authentication/views.py
    from django.http import HttpResponse
    from django.shortcuts import get_object_or_404, redirect, render
    from .models import User
    from .forms import RegisterUser, LoginUser
    # Create your views here.

    def register(request):
        # Get the form to display in the webpage.
        # request.POST or none: is that the submission should be in POST request. If it is not then it should be considered as a none.
        form = RegisterUser(request.POST or None)

        # If request is something else than POST, don't execute the code further.

        if request.method == 'POST':
            
            if form.is_valid():
                # It'll store the data entered into a form in Database.
                form.save()
        # Mentioning which HTML we want to display, passing the form as a context so that we can use inside HTML page.
        return(render(request, 'register.html', {'form': form}))

    ```

- All the components are in place, we now only need to configure routes for our view.

    ```python
    # authentication/urls.py

    from unicodedata import name
    from django.urls import path

    from . import views

    urlpatterns = [
        path('register', views.register, name='register'),
    ]
    ```

    ```python
    # project/urls.py

    from django.contrib import admin
    from django.urls import path, include


    urlpatterns = [
        path('admin/', admin.site.urls),
        path('auth/',include('authentication.urls')),
      ]
    ```


## Creating Login View

- Create file inside `authentication/templates` called login.html.

    ```html
    <!-- authentication/templates/login.html -->
    <form action="" method="post">
    <!-- To avoid XSS attacks -->
    {% csrf_token %}
    <!-- Display the form -->
    <!-- as_p: display the form as paragraph. -->
    {{ form.as_p }}
    <input type="submit" value="Submit">
    </form>

    ```

- Create `form` for the Login inside `forms.py`

    ```diff
    # authentication/forms.py

    from django import forms
    from .models import User
    
    class RegisterUser(forms.ModelForm):
        # Makes password written in hidden i.e ******
        # If you don't mention widget=forms.PasswordInput(), password will be visible. i.e mypassword
        user_password = forms.CharField(widget=forms.PasswordInput())
        class Meta:
            model = User
            fields = ('user_name', 'user_email', 'user_password')

    +class LoginUser(forms.Form):
    +
    +    user_name = forms.CharField(max_length=50)
    +    user_password = forms.CharField(widget=forms.PasswordInput())
    ```

- *Notice* the `forms.Form` instead of `forms.ModelForm`, because we're not creating `form` based on the `model`. It is a `custom` form that is why we have used `forms.Form`.


- Create a `view` for login inside `authentication/views.py`

    ```diff

    from django.http import HttpResponse
    from django.shortcuts import get_object_or_404, redirect, render
    from .models import User
    from .forms import RegisterUser, LoginUser
    # Create your views here.

    def register(request):
        # Get the form to display in the webpage.
        # request.POST or none: is that the submission should be in POST request. If it is not then it should be considered as a none.
        form = RegisterUser(request.POST or None)

        # If request is something else than POST, don't execute the code further.

        if request.method == 'POST':
            
            if form.is_valid():
                # It'll store the data entered into a form in Database.
                form.save()
        # Mentioning which HTML we want to display, passing the form as a context so that we can use inside HTML page.
        return(render(request, 'register.html', {'form': form}))


    +def login(request):
    +    form = LoginUser(request.POST or None)
    +
    +    if request.method == 'POST':
    +  
    +        if form.is_valid():
    +            user_name = form.cleaned_data.get('user_name')
    +            user_password = form.cleaned_data.get('user_password')
    +            print('Username: {0}, password: {1}'.format(user_name, user_password))
    +            try:
    +                user = get_object_or_404(User, user_name=user_name, user_password=user_password)
    +                # it's set the session.
    +                request.session['user_name'] = user.user_name
    +                return redirect('user_page')
    +            except Exception:
    +                return HttpResponse('User does not exist')
    +    return(render(request, 'login.html', {'form': form}))
    +
    +def user_page(request):
    +    user_name = request.session['user_name']
    +    return(render(request, 'welcome.html', {'user': user_name}))

    ```

- Here in the `login`, we're fetching the user entered data by using `cleaned_data.get()` method, and we're checking that data against our Database, if the data exists user should be login else it'll show a custom error message.

- Apart from that, we've also created another view called `user_page` in which we've simply displaying the message, welcome {user}. Here's the template file for that.

```html
<!-- authentication/templates/welcome.html -->
<h1>Welcome user: {{user}}</h1>
```

- Also if you have noticed the keyword, `request.session`. This is the Django Session which is used to manage User' session.

- And, finally here's the `urls.py` that routes all the views.
  
  ```python
    # authentication/urls.py
    from django.urls import path

    from . import views

    urlpatterns = [
        path('register', views.register, name='register'),
        path('login', views.login, name='login'),
        path('welcome', views.user_page, name='user_page'),
    ]
  ```