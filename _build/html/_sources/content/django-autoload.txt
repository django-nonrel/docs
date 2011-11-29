django-autoload
========================================

django-autoload is a reuseable django app which allows for correct initialization of your django project by ensuring the loading of signal handlers or indexes before any request is being processed. 

.. raw:: html

   <ul class="project-links">
     <li><a href="https://bitbucket.org/twanschik/django-autoload/src">Source</a></li>
     <li><a href="/blog/django">Blog</a></li>
   </ul>




Installation
----------------------------------------------------

#. Add django-autoload/autoload to settings.INSTALLED_APPS
    .. sourcecode:: python

       INSTALLED_APPS = (
           'autoload',
       )

#. Add the ``autoload.middleware.AutoloadMiddleware`` before any other middelware
    .. sourcecode:: python
   
       MIDDLEWARE_CLASSES = ('autoload.middleware.AutoloadMiddleware', ) + \
                             MIDDLEWARE_CLASSES


How does django-autoload ensures correct initialization of my project?
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

django-autoload provides two mechanisms to allow for correct initializations:

#. The ``autoload.middleware.AutoloadMiddleware`` middleware will load all ``models.py`` from settings.INSTALLED_APPS as soon as the first request is being processed. This ensures the registration of signal handlers for example.

#. django-autoload provides a site configuration module loading mechanism. Therefore, you have to create a site configuration module. The module name has to be specified in your settings:

.. sourcecode:: python

   # settings.py:
   AUTOLOAD_SITECONF = 'indexes'

Now, django-autoload will load the module ``indexes.py`` before any request is being processed. An example module could look like this: 

.. sourcecode:: python

   # indexes.py:
   from dbindexer import autodiscover
   autodiscover()

This will ensure the registration of all database indexes specified in your project.


autoload.autodiscover(module_name)
__________________________________________

Some apps like django-dbindexer_ or nonrel-search_ provide an autodiscover-mechanism which tries to import index definitions from all apps in settings.INSTALLED_APPS. Since the autodiscover-mechanism is so common django-autoload provides an ``autodiscover`` function for these apps.

``autodiscover`` will search for ``[module_name].py`` in all ``settings.INSTALLED_APPS`` and load them. django-dbindexer's autodiscover function just looks like this for example:

.. sourcecode:: python

    def autodiscover():
        from autoload import autodiscover as auto_discover
        auto_discover('dbindexes')

.. _Get SQL features on NoSQL with django-dbindexer: /blog/django/2010/09/Get-SQL-features-on-NoSQL-with-django-dbindexer
.. _`JOINs for NoSQL databases via django-dbindexer - First steps`: http://www.allbuttonspressed.com/blog/django/joins-for-nosql-databases-via-django-dbindexer-first-steps
.. _`JOINs via denormalization for NoSQL coders, Part 1`: http://www.allbuttonspressed.com/blog/django/2010/09/JOINs-via-denormalization-for-NoSQL-coders-Part-1-Intro
.. _djangoappengine: http://www.allbuttonspressed.com/projects/djangoappengine
.. _django-mongodb-engine: http://github.com/aparo/django-mongodb-engine
.. _djangotoolbox: http://www.allbuttonspressed.com/projects/djangotoolbox
.. _django-dbindexer: http://www.allbuttonspressed.com/projects/django-dbindexer
.. _nonrel-search: http://www.allbuttonspressed.com/projects/nonrel-search
