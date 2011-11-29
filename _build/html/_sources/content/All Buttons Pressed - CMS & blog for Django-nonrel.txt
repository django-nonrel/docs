.. toctree::
   :maxdepth: 2

All Buttons Pressed Blog Sample Project
================================================

This is the the website you're currently looking at. All Buttons Pressed provides a simple "CMS" with support for multiple independent blogs. It's compatible with Django-nonrel_ and the normal SQL-based Django, so you can use it with App Engine, MongoDB, MySQL, PostgreSQL, sqlite, and all other backends that work with either Django or Django-nonrel. Actually the project was started to demonstrate what's possible with Django-nonrel and to give you an impression of how to write code for non-relational databases.

.. raw:: html

   <ul class="project-links">
      <li><a href="#documentation">Documentation</a></li>
      <li><a href="http://bitbucket.org/wkornewald/allbuttonspressed/src">Source</a></li>
      <li><a href="http://bitbucket.org/wkornewald/allbuttonspressed/downloads/allbuttonspressed.zip">Download</a></li>
   </ul>

Documentation
------------------------------------

Note: Some parts of the code are explained in the `4 things to know for NoSQL Django coders`_ blog post.

Before you start you'll need to install Sass_ and Compass_. Then you can just download_ the zip file. Make sure that your App Engine SDK is at least version 1.5.0 prerelease.

Alternatively, if you want to clone the latest code you'll have to also install django-mediagenerator_. By default, the settings file is configured to use App Engine, so you'll also need djangoappengine_ and all of its dependencies (Django-nonrel_, djangotoolbox_, django-dbindexer_, and django-autoload_). In contrast, on SQL databases you won't need any of those dependencies except for django-mediagenerator_.

First, create an admin user with ``manage.py createsupeuser``. Then, run ``manage.py runserver`` and go to http://localhost:8000/admin/ and create a few pages and posts. Otherwise you'll only see a 404 error page.

All Buttons Pressed has a concept called "Block". Blocks can be created and edited via the admin UI. The sidebar's content is defined via a block called "sidebar".

The menu is defined via a "menu" block where each menu item is in the format ``<label> <url>``:

.. sourcecode:: text

    Some label     /url
    Other label    http://www.other.url/

.. _Sass: http://sass-lang.com/
.. _Compass: http://compass-style.org/
.. _4 things to know for NoSQL Django coders: /blog/django/2010/02/4-things-to-know-for-NoSQL-Django-coders
.. _Django-nonrel: /projects/django-nonrel
.. _django-mediagenerator: /projects/django-mediagenerator
.. _download: http://bitbucket.org/wkornewald/allbuttonspressed/downloads/allbuttonspressed.zip
.. _djangoappengine: /projects/djangoappengine
.. _djangotoolbox: /projects/djangotoolbox
.. _django-dbindexer: /projects/django-dbindexer
.. _django-autoload: /projects/django-autoload
