# Django templates and views

From what we doing earlier, we're simply returing `HttpResponse` with some value in it which was basic and insufficient, because of that `Templates` comes into picture.

Django Templates are bit special, in case you can also call it a `Hybrid`, because we can write `python` code along with the `HTML`.

To create Django Templates, make sure to create them `templates` directory inside the `app` folder first and place your `HTML/templates` file over there. If you put somewhere else, Django may not be able to locate it due to configured default settings inside `settngs.py`.

## Example of Django Template

```html
{% if latest_list %}
    <ul>
    {% for question in latest_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.id }} | {{ question.question_text }} | {{question.pub_date}}</a></li>
    {% endfor %}
    {{ my_name }}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```
You may have noticed weird `{% %}` syntax. It is used to indicate that this is the `python` code. So whenever you're writing `Python` code make sure you follow the syntax.

Important thing is that whenever you write `if` and `for` loop make sure to close it with `endif` and `endfor` keyword, so that Django understands that we ended the loop here.

Also, when there is need to display something to the webpage, make sure you mention that in between ``{{  }}`` this syntax.

## Using Templates with Views

Now that we have seen the simple and basic `Templates`, it is time to use them with Django `Views`.

```python
# polls/views.py

from django.http import HttpResponse
from django.template import loader
from .models import Question

def show_question_list(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:2]
    my_name = 'Jack'
    template = loader.get_template('polls/index.html')
    context = {
        'latest_list': latest_question_list,
        'my_name': my_name
    }
    return HttpResponse(template.render(context, request))

```

## Explanation of the Code Snippet

To use template in view, you have to mention which template that you want to use. In order to do that, we've used `loader.get_template()` method, where we have mentioned the location of template. Note that this location is relative to the templates folder in your app.

This is my structure of template

```
polls
    - django files i.e urls.py models.py
    - templates - folder
        - polls - folder
            - index.html - template file

```

So that's why you'll find `polls/index.html` in the method parenthesis. You do not have to create a folder called polls or anything, you can also put your template file directly inside `templates` directory.

If you've noticed in `Django Template example` above, you'll find the variable `latest_list`. That variable is passed as context to the template so that we can use it's value inside template. 