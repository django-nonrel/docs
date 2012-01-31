Django-nonrel
=============

Django-nonrel adds support for NoSQL databases such as MongoDB_ or
`Google App Engine`_ to Django_.

There is Django-nonrel the software and then there is Django-nonrel
the project. Django-nonrel the software is vanilla Django with a
series of patches applied to it (~200 lines of code in total).
Django-nonrel the project is an umbrella project for various kinds of
NoSQL database backends and their related bits:

* Common parts of the Django-nonrel project (the bits independent of
  the database backend):

  - Django-nonrel the software is what we get if we apply :doc:`a
    series of patches <differences>` to Django 1.3.1.

  - :doc:`djangotoolbox/index` is a set of utilities for developing
    applications with Django-nonrel e.g. it provides model fields such
    as :class:`ListField` and :class:`EmbeddedModelField` which are
    used by :doc:`mongodb-engine/index`.

* Django-aware wrappers around the actual database drivers:

  - :doc:`djangoappengine/index` is the glue between Django-nonrel and
    `Google App Engine`_.
  - :doc:`mongodb-engine/index` is the wrapper around PyMongo_ which in
    turn is the Python driver for MongoDB_.

* Related bits and pieces:

  - :doc:`django-dbindexer/index` abstracts away the differences
    between relational and NoSQL databases at the cost of (storage)
    efficiency. Examples of what it does is denormalizing data to
    emulate JOINs, or store additional lower-cased versions of strings
    to allow for case-insensitive comparison.
  - django-filetransfers_ extends Django's file storage API in order
    to fully support file uploads and downloads on any cloud storage
    provider (App Engine, Amazon S3, ...)
  - :doc:`mongodb-cache/index` is a cache backend similar to Django's
    built-in database cache backend, but for MongoDB rather than for
    SQL databases.

Contents
--------
.. toctree::
   :maxdepth: 2

   installation
   schema-design
   troubleshooting
   porting
   differences

   djangotoolbox/index

   mongodb-engine/index
   djangoappengine/index

   django-dbindexer/index
   mongodb-cache/index

   contributing

.. _Django: http://djangoproject.com
.. _Google App Engine: http://code.google.com/appengine
.. _MongoDB: http://mongodb.org
.. _PyMongo: http://api.mongodb.org/python/current/
.. _Django MongoDB Engine: http://django-mongodb.org
