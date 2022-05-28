# Django Generic Create View

CreateView is A view that displays a form for creating an object, re-displaying the form with validation errors (if there are any) and saving the object. 

## Example

- Create app called `generics`
  
  ```bash
  python -m django startapp generics
  ```

- Create `model` in `models.py`
  ```python
  # generics/models.py

  from django.db import models
  from authentication.models import User

    class Blog(models.Model):
        created_by = models.ForeignKey(User, on_delete=models.CASCADE)
        title = models.CharField(max_length=150)
        description = models.TextField(max_length= 2000)
        image = models.ImageField(upload_to='uploads/')

        def __str__(self) -> str:
            return self.title

  ```

- Register the app in global `settings.py`
  ```python
  
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'generics.apps.GenericsConfig' # Registered app generics
    ]
  ```

- Run the migrations
  ```bash
  python manage.py makemigrations
  python manage.py migrate
  ```

- Create `templates` directory inside `generics` app directory.Finally, create `create-blog.html`
  
  ```html
    <!-- generics/templates/create-blog.html -->
    <form method="POST" enctype="multipart/form-data">

        <!-- Security token -->
        {% csrf_token %}

        <!-- Using the formset -->
        {{ form.as_p }}
        
        <input type="submit" value="Submit">
    </form>

  ```

- Write the view in `views.py` file.
  
  ```python
  # generics/views.py

  from django.http import HttpResponse
  from django.views.generic.edit import CreateView
  from .models import Blog
  from authentication.models import User
  # Create your views here.

  class CreateBlogView(CreateView):
      # Mention the model/table that you want to create a record for from the User.
      model = Blog
      # Mention the fields that you want user to enter the data.
      fields = ['title', 'description', 'image']

      # Mention the template that you want to use.
      template_name = 'generics/create-blog.html'

      # This is the init method when the webpage gets load.
      def dispatch(self, request, *args, **kwargs):
          try:
              # Checking if the user is logged in or not
              user = request.session['user_name']
              print(user)
          except Exception:
              # if user is not logged in, send user to login first
              return HttpResponse("Please login first. Click here to <a href='/auth/login'>Login</a>")
          return super(CreateBlogView, self).dispatch(request, *args, **kwargs)
      
      # This method will be executed only if form method is POST
      def post(self, request):
          # get the user entered form.
          form = self.get_form()
          # If the form is valid:
          if form.is_valid():
              # Get the form data first, and don't store it to database yet.
              obj = form.save(commit=False)
              # edit the form object property `created_by`, because we set it to Foreign key, it's value should be type of the object `User`, because it is linked to table `User`
              obj.created_by = User.objects.get(user_name=request.session['user_name'])
              # Save the form, It'll create entry inside the database.
              obj.save()
              # Return the response to user.
              return HttpResponse('Your blog has been saved.')

  ```

- Configure the routes
  
  ```python
  # generics/urls.py
  from django.urls import path
  from . import views
  urlpatterns = [
      # Note the `as_view()`, because we have used class based views, we need to mention it here.
      path('create', views.CreateBlogView.as_view(), name='createblog'),
  ]
  ```

  ```python
  # Project's main urls.py
  # webapp/urls.py

  from django.contrib import admin
  from django.urls import path, include


  urlpatterns = [
      path('admin/', admin.site.urls),
      path('polls/', include('polls.urls')),
      path('myapp/',include('demo.urls')),
      path('auth/',include(('authentication.urls', 'authentication'), namespace='authentication')),
      # Note the `namespace`, here namespace is used to point or redirect to any app's view. i.e {% url generics:createblog %}
      path('blog/',include(('generics.urls', 'generics' ), namespace="generics")),

  ]
  ```