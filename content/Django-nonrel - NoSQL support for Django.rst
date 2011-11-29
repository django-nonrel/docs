Django-nonrel - NoSQL support for Django
=================================================

Django-nonrel is an independent branch of Django that adds NoSQL database support to the ORM. The long-term goal is to add NoSQL support to the official Django release. Take a look at the documentation below for more information.

.. raw:: html

   <ul class="project-links">
      <li><a href="#documentation">Documentation</a></li>
      <li><a href="http://bitbucket.org/wkornewald/django-nonrel/src">Source</a></li>
      <li><a href="http://bitbucket.org/wkornewald/django-nonrel/get/tip.zip">Download</a></li>
      <li><a href="http://groups.google.com/group/django-non-relational">Discussion group</a></li>
      <li><a href="/">Blog</a></li>
   </ul>

Why Django on NoSQL
--------------------------------------
We believe that development on non-relational databases is unnecessarily unproductive:

* you can't reuse code written for other non-relational DBs even if they're very similar
* you can't reuse code written for SQL DBs even if it could actually run unmodified on a non-relational DB
* you have to manually maintain indexes with hand-written code (denormalization, etc.)
* you have to manually write code for merging the results of multiple queries (JOINs, ``select_related()``, etc.)
* some databases are coupled to a specific provider (App Engine, SimpleDB), so you're locked into their hosting solution and can't easily move away if at some point you need something that's impossible on their platform

How can we fix the situation? Basically, we need a powerful abstraction that automates all the manual indexing work and provides an advanced query API which simplifies common nonrel development patterns. The Django ORM is such an abstraction and it fits our goals pretty well. It even works with SQL and thus allows to reuse existing code written for SQL DBs.

Django-nonrel is in fact only a small patch to Django that fixes a few assumptions (e.g.: ``AutoField`` might not be an integer). The real work is done by django-dbindexer_. The dbindexer will automatically take care of denormalization, JOINs, and other important features in the future too. Currently, it extends the NoSQL DB's native query language with support for filters like ``__month`` and ``__icontains`` and adds support for simple JOINs. If you want to stay up-to-date with our progress you should subscribe_ to the `Django-nonrel blog`_ for the latest news and tutorials.

This page hosts general information that is independent of the backend. You should also take a look at the `4 things to know for NoSQL Django coders`_ blog post for practical examples.

The App Engine documentation is hosted on the djangoappengine_ project site. We've also published some information in the `Django on App Engine`_ blog post.

The MongoDB backend is introduced in the `MongoDB backend for Django-nonrel`_ blog post.

Backends for ElasticSearch_ and Cassandra_ are also in development.

Documentation
----------------------------------
Differences to Django
---------------------------------------------------
Django should mostly behave as described in the `Django documentation`_. You can run Django-nonrel in combined SQL and NoSQL multi-database setups without any problems. Whenever possible, we'll try to reuse existing Django APIs or at least provide our own APIs that abstract away the platform-specific API, so you can write portable, database-independent code. There might be a few exceptions and you should consult the respective backend documentation (e.g., djangoappengine_) for more details. Here are a few general rules which can help you write better code:

Some backends (e.g., MongoDB) use a string instead of an integer for ``AutoField``. If you want to be on the safe side always write code and urlpatterns that would work with both strings and integers. Note that ``QuerySet`` automatically converts strings to integers where necessary, so you don't need to deal with that in your code.

There is no integrity guarantee. When deleting a model instance the related objects will **not** be deleted. This had to be done because such a deletion process can take too much time. We might solve this in a later release.

JOINs don't work unless you use django-dbindexer_, but even then they're very limited at the moment. Without dbindexer, queries can only be executed on one single model (filters can't span relationships like ``user__username=...``).

You can't use Django's transactions API. If your particular DB supports a special kind of transaction (e.g., ``run_in_transaction()`` on App Engine) you have to use the platform-specific functions.

Not all DBs provide strong consistency. If you want your code to be fully portable you should assume an eventual consistency model. For example, recently App Engine introduced an eventually-consistent high-replication datastore which is now the preferred DB type because it provides very high availability. Strongly consistent operations should be implemented via ``QuerySet.update()`` instead of ``Model.save()`` (unless you use ``run_in_transaction()`` on App Engine, of course).

Workarounds
-------------------------------------------------
You can only edit users in the admin interface if you add "djangotoolbox" to your ``INSTALLED_APPS``. Otherwise you'll get an exception about an unsupported query which requires JOINs.

Florian Hahn has also written an `authentication backend`_ which provides permission support on non-relational backends. You should use that backend if you want to use Django's permission system.

Writing a backend
-------------------------------------------------
The `backend template`_ provides a simple starting point with simple code samples, so you understand what each function does.

Sample projects
-------------------------------------------------
You can use allbuttonspressed_ or django-testapp_ as starting points to see what a nonrel-compatible project looks like.

Internals
-------------------------------------------------
Django-nonrel is based on Django 1.3. The modifications to Django are minimal (maybe less than 100 lines). Backends can override the ``save()`` process, so no distinction between ``INSERT`` and ``UPDATE`` is made and so there is no unnecessary ``exists()`` check in ``save()``. Also, deletion of related objects is disabled on NoSQL because it conflicts with transaction support at least on App Engine. These are the most important changes we've made. The actual DB abstraction is handled by django-dbindexer_ and it will stay a separate component even after NoSQL support is added to the official Django release.

Contributors
-----------------------------------
* Alberto Paro
* Andi Albrecht
* Flavio Percoco Premoli
* Florian Hahn
* Jonas Haag
* Kyle Finley
* George Karpenkov
* Mariusz Kryński
* Matt Bierner
* Thomas Wanschik
* Waldemar Kornewald

.. _Django documentation: http://docs.djangoproject.com/
.. _4 things to know for NoSQL Django coders: /blog/django/2010/02/4-things-to-know-for-NoSQL-Django-coders
.. _Django on App Engine: /blog/django/2010/01/Native-Django-on-App-Engine
.. _MongoDB backend for Django-nonrel: /blog/django/2010/05/MongoDB-backend-for-Django-nonrel-released
.. _Django-nonrel blog: /blog/django
.. _djangoappengine: /projects/djangoappengine
.. _subscribe: http://feeds.feedburner.com/AllButtonsPressed
.. _backend template: http://www.allbuttonspressed.com/blog/django/2010/04/Writing-a-non-relational-Django-backend
.. _django-testapp: http://bitbucket.org/wkornewald/django-testapp
.. _allbuttonspressed: /projects/allbuttonspressed
.. _django-dbindexer: http://www.allbuttonspressed.com/projects/django-dbindexer
.. _authentication backend: http://bitbucket.org/fhahn/django-permission-backend-nonrel
.. _Cassandra: http://github.com/vaterlaus/django_cassandra_backend
.. _ElasticSearch: http://github.com/aparo/django-elasticsearch
