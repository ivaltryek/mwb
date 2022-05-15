# Django Models

Django `models` are way to write definition of tables as Python code. You can write these definitions in app's `models.py` file.

Here are some examples of it.

```python

# polls/models.py

from django.db import models

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    def __str__(self) -> str:
        return self.question_text


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self) -> str:
        return self.choice_text

```
Here `class` Question defines the Model name, meaning that Table/Model name will be `Question`. Inside the `class` it has `properties` which could be also referenced as `Columns` of `Table`

## Explanation of Snippet

We have two models one is `Question` and another one is `Choice`. Here `Question` has 2 `Columns` in it, `question_text` which is `Character Field` meaning it can store `AlphaNumeric` Values. `pub_date` is `DateTime` Field, meaning it can only store Dates with Time.

`Choice` model has 3 `Columns`, `question` which is a `ForeignKey` meaning It is being referenced to `Question` model and also it has property `on_delete=models.CASCADE`, which tells us that if `Question` is deleted then `Choice`s related to that `Question` will also get deleted. You can use other than `CASCADE` value based on your use case, you can find more about it [here](https://docs.djangoproject.com/en/4.0/ref/models/fields/#:~:text=models.ForeignKey(%27self%27%2C-,on_delete,-%3Dmodels.CASCADE)).

`choice_text` is again `AlphaNumeric` field. And, `votes` is an `IntegerField` meaning that it can only store `Interger` Values.

At the end of the `model`, you'll find `method` `__str__`, using this method you can customize `Model Objects` look in Admin panel.

## Running Migrations

Now that we have written our models, it is time to apply these changes in our Database, To do that run the following command,

```bash
python manage.py makemigrations polls
python manage.py migrate
```
Here make a note that, at the end of the command I've mentioned the `App` name, so that it only runs the defined `App` model's migrations. IF you want to run all `Apps` migration you don't have to defined anything `at the end` of the command.

Now that if you open the Django Admin Panel, you'll find new models under your `App` section, here my `App` name is `Polls`.

![image](https://user-images.githubusercontent.com/31511537/168468072-24ff0bf5-f2fc-4ec9-adef-ba945610d48b.png)

Django Models has many types of `Field` types, Each `field` in your model should be an instance of the appropriate `Field` class. Django uses the field class types to determine a few things:
- The column type, which tells the database what kind of data to store (e.g. INTEGER, VARCHAR, TEXT).
- The default HTML widget to use when rendering a form field
- The minimal validation requirements, used in Django’s admin and in automatically-generated forms.
  
Follow the `Django Model Field` [docs](https://docs.djangoproject.com/en/4.0/ref/models/fields/#model-field-types) for many different `Field` types you can in your `models`.
