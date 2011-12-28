Django-nonrel
=============

Django-nonrel is a fork of Django_ that adds support for NoSQL databases.

The Django-nonrel project is split into several sub-projects:

* Django-nonrel "itself" --- a version of Django 1.3.1 with
  :doc:`some small changes <nonrel-differences>` to make things NoSQL compatible
* :doc:`djangotoolbox` --- a set of utilities for developing applications against
  Django-nonrel -- most notably, it provides model fields like :class:`ListField`
  and :class:`EmbeddedModelField`.
* Our database backends, currently being

  - djangoappengine_, the backend for `Google App Engine`_, and
  - `Django MongoDB Engine`_, the backend for MongoDB_.


Other related tools are:

* :doc:`django-dbindexer` --- abstracts away the differences between relational
  and non-relational databases at the cost of (storage) efficiency,
  e.g. by using denormalization to emulate JOINs or by storing an additional
  lower-cased version of each string to allow for case-insensitive comparison
* django-filetransfers_ --- extends Django's file storage API in order to fully
  support file uploads and downloads on any cloud storage provider (App Engine,
  Amazon S3, ...)


Contents
--------
.. toctree::
   :maxdepth: 2

   installation
   schema-design
   troubleshooting
   porting

   djangotoolbox
   django-dbindexer

   contributing
   

(Old TOC:)

.. toctree::
   :maxdepth: 1

   /content/Django-nonrel - NoSQL support for Django
   /content/All Buttons Pressed - CMS & blog for Django-nonrel
   /content/djangoappengine - Django App Engine backends (DB, email, etc.)
   /content/django-autoload
   /content/django-dbindexer - Expressive NoSQL
   /content/django-filetransfers - File upload-download abstraction
   /content/django-mediagenerator asset manager
   /content/djangotoolbox
   /content/HTML5 offline manifests with django-mediagenerator
   /content/Managing per-field indexes on App Engine
   /content/Writing a non-relational Django backend

.. _Django: http://djangoproject.com
.. _Google App Engine: http://code.google.com/appengine
.. _MongoDB: http://mongodb.org
.. _Django MongoDB Engine: http://django-mongodb.org
