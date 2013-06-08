---
layout: post
title: "Django and Google App Engine Tutorial"
date: 2009-02-17 11:45
comments: true
categories:
- coding
- django
- google app engine
- howto
- python
- tech
- tutorial
---
Hi have been playing around with Google App Engine for a while now, and wanted to learn Django on a new application I am building. The two articles I started with are:

- [Using the Google App Engine Helper for Django](http://code.google.com/appengine/articles/appengine_helper_for_django.html)
- [Using Django 1.0 on App Engine with Zipimport](http://code.google.com/appengine/articles/django10_zipimport.html)

Both Django and GAE are being developed as I write this, so although these instructions are kind of recent, they are already out of date, or rely on you having knowledge of Django. Since there are a lot of others with no Python or Django experince wanting to learn, I thought I would make a tutorial that works as of today, but who knows a month from now or even tomorrow.

Note: This tutorial is written for Linux. Mac/Windows users will have to translate :-)

## Create django.zip

Django comes as a .tar.gz file, but we want a zip file to take advantage of the Zipimport library, so some conversion is needed.

1. [Download the latest Django](http://www.djangoproject.com/download/) and untar the file (in this case, it is 1.0.2)
1. `cd Django-1.0.2-final`
1. Create the zip file, but without the admin section since App Enging supplies it's own
   `zip -r ~/django.zip django -x 'django/contrib/admin*'`

## Get the Helper

The App Engine Helper or Django is an open source bootstrapper for getting Django started on App Engine. Downloading it from the website will currently give you quite an old version (r52) which will not work in this tutorial. Instead, use subversion to get the latest (r74).

``` bash
cd ~
svn export http://google-app-engine-django.googlecode.com/svn/trunk/ gae-django-tutorial
cd gae-django-tutorial
mv ~/django.zip .
```

## Setup App Engine

As you would with any GAE application, edit the app.yaml file to refer to your application ID. The Helper also needs to know where your Google App Engine SDK is, since it is going to change how you start the development server, so create a link to it:

``` bash
cd gae-django-tutorial
ln -s /path/to/google_appengine .google_appengine
```

## Start the development server

Django has a different way of running the development server. Instead of using dev_appserver.py to start the dev server, do the following:

``` bash
python manage.py runserver
```

If everything is running correctly, you should see something like:

```
INFO:root:Server: appengine.google.com
INFO:root:Checking for updates to the SDK.
INFO:root:The SDK is up to date.
INFO:root:Running application google-app-engine-django on port 8000: http://localhost:8000
```

However, if you are like me and saw an error like the following:

```
ImportError: No module named antlr3
```

Then you will need to install the antlr3 python module. Luckily this is easy.

1. Go to http://www.antlr.org/download/Python and download the latest zip
1. `unzip antlr_python_runtime-3.1.zip`
1. `cd antlr_python_runtime-3.1`
1. `sudo python setup.py install`

Lets see the site! When you go to http://localhost:8000/ you should see a page saying "It worked! Congratulations on your first Django-powered page."
Pretty (un)impressive huh?

Ok, now lets start doing something. Kill the server by pressing Ctrl-C. The Django tutorial is the next stop, which involves creating the Polls Django app. You can read through there to get a full understanding. For simplicity, I am only what I did to get it working.

``` bash
python manage.py startapp polls
cd polls
Edit models.py
```

``` python
from appengine_django.models import BaseModel
from google.appengine.ext import db

class Poll(BaseModel):
  question = db.StringProperty()
  pub_date = db.DateTimeProperty('date published')

class Choice(BaseModel):
  poll = db.ReferenceProperty(Poll)
  choice = db.StringProperty()
  votes = db.IntegerProperty()
```

1. Edit views.py (notice that order_by() from the Django tutorial is just order() here)

``` python
from django.template import Context, loader
from polls.models import Poll
from django.http import HttpResponse

def index(request):
  latest_poll_list = Poll.objects.all().order('-pub_date')[:5]
  t = loader.get_template('polls/index.html')
  c = Context({
    'latest_poll_list': latest_poll_list,
  })
  return HttpResponse(t.render(c))
```

1. cd ..
1. Edit urls.py and add to urlpatterns
   `(r'^polls/', 'polls.views.index'),`
1. mkdir -p templates/polls
1. cd templates/polls
1. Create index.html
``` irb
{% if latest_poll_list %}
<ul>
{% for poll in latest_poll_list %}
  <li>{{ poll.question }}</li>
{% endfor %}
</ul>
{% else %}
  <p>No polls are available.</p>
{% endif %}
```

1. cd ../..
1. Edit settings.py and add to INSTALLED_APPS
   'polls',
1. Now you can run the dev server again:
   `python manage.py runserver`
1. And go to:
   http://localhost:8000/polls/
1. You should see the message "No polls are available."

## Adding some data

You can go through the rest of the Django Tutorial to fill out the rest of the views. One last thing to know is the admin site. To view it, go to:

http://localhost:8000/_ah/admin

Notice that the Datastore Viewer is empty - it doesn't even know about our Poll or Choice Models. Don't panic. Go back to your terminal, and terminate the dev server. Now run:

``` bash
python manage.py shell
```

This brings up a special python shell with the Django environment set up for you. Now you can run:
```
>>> from polls.models import Poll, Choice
>>> import datetime
>>> p = Poll(question="What's up?", pub_date=datetime.datetime.now())
>>> p.save()
```

You have just created a new Poll. To end the shell, press Ctrl-D.

If you start the dev server again (or if you never stopped it in the first place), you should be able to go to:

http://localhost:8000/polls/

to see you new Poll, and go to the admin site:

http://localhost:8000/_ah/admin/datastore

to see and create new Polls

Easy huh? :-)