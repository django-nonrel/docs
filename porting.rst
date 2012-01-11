Porting
=======

Django should mostly behave as described in the `Django documentation`_. You can run Django-nonrel in combined SQL and NoSQL multi-database setups without any problems. Whenever possible, we'll try to reuse existing Django APIs or at least provide our own APIs that abstract away the platform-specific API, so you can write portable, database-independent code. There are however some notable differences you should be aware of.

Cascade deletes
---------------
* currently are disabled, may be reenabled, but are not properly supported (see <https://github.com/django-nonrel/django-nonrel/issues/1>)
* consider using model embedding instead of linking or use post delete signals to keep your data consistent

JOINs
-----
* only some simple cases may be emulated (through django-dbindexer_))
* this results in ManyToMany fields and lookups that span relationships being unavailable (TODO: Anything more?)
* use ListFields and EmbeddedModelFields to model your data instead

Indexes
--------
* you'll likely need to an index for each "select-like" query you make (through filter(), order_by() or field lookups)
* some back-ends can handle this semi-automatically (e.g. GAE dev server)
* some filters are not supported natively and require registering an index with dbindexer -- storing additional data (e.g. iexact, endswith with GAE)

.. TODO: Is this true for Mongo too?

Aggregates
----------
* aggregates (count, min, max, avg etc.) can only be simulated on some back-ends
* in most cases these are not efficient operations on non-relational DBs and should be used sparingly

Transactions
------------
* Django's transactions API is not usable
* some back-end-specific solutions may be available:
    * MongoDB [atomic operations](http://django-mongodb.org/topics/atomic-updates.html)
    * GAE [entity groups](https://github.com/django-nonrel/djangoappengine/pull/10)

Contrib modules
---------------
* contrib.auth relies on many-to-many relations heavily, use django-permission-backend-nonrel_ instead
* contrib.flatpages uses a single ManyToMany field that can be replaced by a ForeignKey
* to be able to edit users through contrib.admin you need to add djangotoolbox to INSTALLED_APPS
* all the rest may be used without any modifications

.. _Django documentation: http://docs.djangoproject.com/
.. _django-dbindexer: https://github.com/django-nonrel/django-dbindexer
.. _django-permission-backend-nonrel: https://github.com/django-nonrel/django-permission-backend-nonrel

