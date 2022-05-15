# Django Parametered View

Earlier we saw that we can display data to webpage using Django `views` which basically contains business logic.

## Examples of Django Parametered View

### Get Parameter value from URL to inside of view.
```python
# polls/views.py
def question_by_id(request, question):
    return HttpResponse('This is question: %s ' % question)
```
Now that the view is created, we need to map it to the `urls.py`
```python
# polls/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('<int:question>/', views.question_by_id, name='question_by_id')
]
```

If you notice the change we have weird path: `<int:question>/`, here `int` defines that the value should be an integer and `:question` is the parameter that we mentioned in our `polls/views.py`. Make sure that in both files parameter should be same. For example this view will only be invoked if you surf the URL which is something like this: `127.0.0.1/polls/6` or `127.0.0.1/polls/7`.