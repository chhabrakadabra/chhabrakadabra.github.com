---
layout: post
category: Software
tagline: Instructions on integrating Loggly with your Django app
tags: [loggly, Django, software, development]
---

## Using Loggly with Django ##
I recently decided to beef up the logging functionality at uberlearner.com. And I ended up deciding to use Loggly. But I wasn't very happy with the loggly documentation on how to do so. So here is how I did it:

### Install Hoover ###
Hoover is the library that loggly provides for python. It can be installed very simply using pip.

{% highlight bash %}
pip install Hoover
{% endhighlight %}

### Change Django settings ###
Now add a handler in the LOGGING dictionary of your Django settings file to use hoover:

{% highlight python %}
LOGGING = {
    'version': 1,
    'disable_existing_loggers': True,
    'formatters': {
        'json': {
            '()': 'jsonlogger.JsonFormatter',
            'format': """
                %(threadName)s %(name)s %(thread)d %(created)f 
                %(process)d %(processName)s %(relativeCreated)f 
                %(module)s %(funcName)s %(levelno)d %(msecs)f
                %(pathname)s %(lineno)d %(asctime)s %(message)s 
                %(filename)s %(levelname)s
            """
        }
    },
    'handlers': {
        'loggly': {
            'level': 'ERROR',
            'class': 'hoover.LogglyHttpHandler',
            'token': LOGGLY_TOKEN
        }
    },
    'loggers': {
        'django.request': {
            'handlers': ['loggly'],
            'level': 'ERROR',
            'propagate': True,
        },
    },
}
{% endhighlight %}

**Note**: as can be seen, I have sneaked in another python library called python-json-logger to format my log messages as json packets. This will make the log messages easily searchable in loggly and is the recommended format for messages.
Log the Django way!

You can now log just like Django suggests. In the example above, all error logging in django (relevant to requests) will automatically be caught by the logger 'django.request'. In case you wanted to have a "catch-all" type logger, you could replace 'django.request' with 'django' in the loggers dictionary. 

In the example configuration I have suggested, even log messages by libraries like Tastypie (which logs on 'django.request.tastypie' will be logged to loggly automatically.

*Happy logging!*