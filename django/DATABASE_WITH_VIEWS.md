# Database with Views

In earlier article, we have created the database models and now it's time to add some values in them and fetch from Django `views`.

To do that, we have to first insert values in Model, Easiest way would be directly entering them through Django Admin Panel.

Here are some snapshot that explains how to insert values using Django Admin Panel.

![image](https://user-images.githubusercontent.com/31511537/168472133-bdbe70a0-7e0d-4427-936c-53348e14132d.png)
<hr>

![image](https://user-images.githubusercontent.com/31511537/168472223-26b9afb2-a862-494c-9da8-08ec423e7abb.png)

Now that we have entered question, we can fetch it from `views`. 

## Fetch Database Details using Views
```python
# polls/views.py

def show_question_list(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:2]
    output = ', '.join([q.question_text for q in latest_question_list])
    return HttpResponse(output)
```

## Explanation of the Code Snippet

In above code snippet, we're fetching last 2 added questions based on `pub_date` column, also notice that `-` sign before `pub_date` that tells us that to fetch data in descending order and storing them inside `latest_question_list`. We can fetch Database Model's Object by: `<model_name>.objects.<method>`. You can fetch Database objects in many different ways, for more you can find more details from [here](https://docs.djangoproject.com/en/4.0/topics/db/queries/)