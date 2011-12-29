========
Features
========

Database backend
---------------------------------------------

Field types
`````````````````````````````````````````````
All Django field types are fully supported except for the following:

* ``ImageField``
* ``ManyToManyField``

The following Django field options have no effect on App Engine:

* ``unique``
* ``unique_for_date``
* ``unique_for_month``
* ``unique_for_year``

Additionally :doc:`/djangotoolbox/index` provides non-Django field types in ``djangotoolbox.fields`` which you can use on App Engine or other non-relational databases. These are

* ``ListField``
* ``BlobField``

The following App Engine properties can be emulated by using a ``CharField`` in Django-nonrel:

* ``CategoryProperty``
* ``LinkProperty``
* ``EmailProperty``
* ``IMProperty``
* ``PhoneNumberProperty``
* ``PostalAddressProperty``

QuerySet methods
`````````````````````````````````````````````
You can use the following field lookup types on all Fields except on ``TextField`` (unless you use indexes_) and ``BlobField``

* ``__exact`` equal to (the default)
* ``__lt`` less than
* ``__lte`` less than or equal to
* ``__gt`` greater than
* ``__gte`` greater than or equal to
* ``__in`` (up to 500 values on primary keys and 30 on other fields)
* ``__range`` inclusive on both boundaries
* ``__startswith`` needs a composite index if combined with other filters 
* ``__year``
* ``__isnull`` requires :doc:`/django-dbindexer/index` to work correctly on ``ForeignKey`` (you don't have to define any indexes for this to work)

Using :doc:`/django-dbindexer/index` all remaining lookup types will automatically work too!

Additionally, you can use

* ``QuerySet.exclude()``
* ``Queryset.values()`` (only efficient on primary keys)
* ``Q``-objects
* ``QuerySet.count()``
* ``QuerySet.reverse()``
* ...

In all cases you have to keep general App Engine restrictions in mind.

Model inheritance only works with `abstract base classes`_:

.. sourcecode:: python

    class MyModel(models.Model):
        # ... fields ...
        class Meta:
            abstract = True # important!

    class ChildModel(MyModel):
        # works

In contrast, `multi-table inheritance`_ (i.e. inheritance from non-abstract models) will result in query errors. That's because multi-table inheritance, as the name implies, creates separate tables for each model in the inheritance hierarchy, so it requires JOINs to merge the results. This is not the same as `multiple inheritance`_ which is supported as long as you use abstract parent models.

Many advanced Django features are not supported at the moment. A few of them are:

* JOINs (with django-dbindexer simple JOINs will work)
* many-to-many relations
* aggregates
* transactions (but you can use ``run_in_transaction()`` from App Engine's SDK)
* ``QuerySet.select_related()``

Indexes
`````````````````````````````````````````````
It's possible to specify which fields should be indexed and which not. This also includes the possibility to convert a ``TextField`` into an indexed field like ``CharField``. You can read more about this feature in our blog post `Managing per-field indexes on App Engine`_.

By default, djangoappengine installs ``__iexact`` indexes on ``User.username`` and ``User.email``.

Other
`````````````````````````````````````````````
Additionally, the following features from App Engine are not supported:

* entity groups (we don't yet have a ``GAEPKField``, but it should be trivial to add)
* batch puts (it's technically possible, but nobody found the time/need to implement it, yet)

Email handling
---------------------------------------------
You can (and should) use Django's mail API instead of App Engine's mail API. The App Engine email backend is already enabled in the default settings (``from djangoappengine.settings_base import *``). By default, emails will be deferred to a background task on the production server.

Cache API
---------------------------------------------
You can (and should) use Django's cache API instead of App Engine's memcache module. The memcache backend is already enabled in the default settings.

Sessions
---------------------------------------------
You can use Django's session API in your code. The ``cached_db`` session backend is already enabled in the default settings.

Authentication
---------------------------------------------
You can (and probably should) use ``django.contrib.auth`` directly in your code. We don't recommend to use App Engine's Google Accounts API. This will lock you into App Engine unnecessarily. Use Django's auth API, instead. If you want to support Google Accounts you can do so via OpenID. Django has several apps which provide OpenID support via Django's auth API. This also allows you to support Yahoo and other login options in the future and you're independent of App Engine. Take a look at `Google OpenID Sample Store`_ to see an example of what OpenID login for Google Accounts looks like.

Note that username uniqueness is only checked at the form level (and by Django's model validation API if you explicitly use that). Since App Engine doesn't support uniqueness constraints at the DB level it's possible, though very unlikely, that two users register the same username at exactly the same time. Your registration confirmation/activation mechanism (i.e., user receives mail to activate his account) must handle such cases correctly. For example, the activation model could store the username as its primary key, so you can be sure that only one of the created users is activated.

File uploads/downloads
---------------------------------------------
See :doc:`/django-filetransfers/index` for an abstract file upload/download API for ``FileField`` which works with the Blobstore_ and X-Sendfile and other solutions. The required backends for the App Engine Blobstore are already enabled in the default settings.

Background tasks
---------------------------------------------
**Contributors:** We've started an experimental API for abstracting background tasks, so the same code can work with App Engine and Celery and others. Please help us finish and improve the API here: https://bitbucket.org/wkornewald/django-defer

Make sure that your ``app.yaml`` specifies the correct ``deferred`` handler. It should be:

.. sourcecode:: yaml

    - url: /_ah/queue/deferred
      script: djangoappengine/deferred/handler.py
      login: admin

This custom handler initializes ``djangoappengine`` before it passes the request to App Engine's internal ``deferred`` handler.

App Engine for Business
-------------------------------------------------------------
In order to use ``manage.py remote`` with the ``googleplex.com`` domain you need to add the following to the top of your ``settings.py``:

.. sourcecode:: python

    from djangoappengine.settings_base import *
    DATABASES['default']['DOMAIN'] = 'googleplex.com'

Checking whether you're on the production server
------------------------------------------------------------------------------------------

.. sourcecode:: python

    from djangoappengine.utils import on_production_server, have_appserver

When you're running on the production server ``on_production_server`` is ``True``. When you're running either the development or production server ``have_appserver`` is ``True`` and for any other ``manage.py`` command it's ``False``.

.. _App Engine SDK: http://code.google.com/appengine/downloads.html
.. _abstract base classes: http://docs.djangoproject.com/en/dev/topics/db/models/#abstract-base-classes
.. _multi-table inheritance: http://docs.djangoproject.com/en/dev/topics/db/models/#multi-table-inheritance
.. _multiple inheritance: http://docs.djangoproject.com/en/dev/topics/db/models/#multiple-inheritance
.. _Managing per-field indexes on App Engine: http://www.allbuttonspressed.com/blog/django/2010/07/Managing-per-field-indexes-on-App-Engine
.. _Google OpenID Sample Store: https://sites.google.com/site/oauthgoog/Home/openidsamplesite
.. _Blobstore: http://code.google.com/appengine/docs/python/blobstore/
