---
layout: post
title: Storing Extra Data in Django Join Tables
---

I've had some people ask me about the <code>through</code> argument supported by Django's <code>ManyToManyField</code> class. This option supports a very simple use case: when you want to store additional data in a join table.

Imagine, for instance, that we're building a simple course registration tool. Let's call the Django app <code>courses</code>. We'll define the following models:

{% highlight python %}
from django.db import models

class Course(models.Model):
    """A course offered at our school."""
    name = models.CharField(max_length=100)
    code = models.CharField(max_length=5, unique=True)
    description = models.TextField()

    def __unicode__(self):
        return u'%s: %s' % (self.code, self.name)


class Student(models.Model):
    """A student registered with our school."""
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    number = models.CharField(max_length=10, unique=True)
    courses = models.ManyToManyField(Course, null=True)
    registered_on = models.DateTimeField(auto_now_add=True)

    def __unicode__(self):
        return u'%s -> %s, %s' % (
            self.number, self.last_name, self.first_name)
{% endhighlight %}

When we run <code>syncdb</code> or generate a migration for these
models, Django will automatically create the following tables:
<code>courses_course, courses_student, and
courses_student_courses</code>. The first two should be
self-explanatory. The third table will store information about our
many-to-many relationship. Each row will associate a student id with a
course id.

Now let's say we want to store the date and time whenever a student
registers for a course. We'd want something like the following model:

{% highlight python %}
class Registration(models.Model):
    """Student registration for a course."""
    student = models.ForeignKey(Student)
    course = models.ForeignKey(Course)
    registered_on = models.DateTimeField(auto_now_add=True)
{% endhighlight %}

In order to get Django to use our model as a join table, we can use
<code>through</code> by changing a single line in our student model:

{% highlight python %}
class Student(models.Model):
    ...
    courses = models.ManyToManyField(Course, null=True,
        through='Registration')
    ...
{% endhighlight %}

And that's it. Now, Django will automatically generate a <code>courses_registration</code>
table and use it instead of <code>courses_student_courses</code>, allowing us to store
extra data about the registration.