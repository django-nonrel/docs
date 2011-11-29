Managing per-field indexes on App Engine
================================================

An annoying problem when trying to reuse an existing Django app is that some apps use ``TextField`` instead of ``CharField`` and still want to filter on that field. On App Engine ``TextField`` is not indexed and thus can't be filtered against. One app which has this problem is django-openid-auth_. Previously, you had to modify the model source code directly and replace ``TextField`` with ``CharField`` where necessary. However, this is not a good solution because whenever you update the code you have to apply the patch, again. Now, djangoappengine_ provides a solution which allows you to configure indexes for individual fields without changing the models. By decoupling DB-specific indexes from the model definition we simplify maintenance and increase code portability.

Example
-------------------------------------
Let's see how we can get django-openid-auth to work correctly without modifying the app's source code. First, you need to create a module which defines the indexing settings. Let's call it "gae_openid_settings.py":

.. sourcecode:: python

    from django_openid_auth.models import Association, UserOpenID
    
    FIELD_INDEXES = {
        Association: {'indexed': ['server_url', 'assoc_type']},
        UserOpenID: {'indexed': ['claimed_id']},
    }

Then, in your settings.py you have to specify the list of gae settings modules:

.. sourcecode:: python

    GAE_SETTINGS_MODULES = (
        'gae_openid_settings',
    )

That's it. Now the ``server_url``, ``assoc_type``, and ``claimed_id`` ``TextField``\s will behave like ``CharField`` and get indexed by the datastore.

Note that we didn't place the index definition in the ``django_openid_auth`` package. It's better to keep them separate because that way upgrades are easier: Just update the ``django_openid_auth`` folder. No need to re-add the index definition (and you can't mistakenly delete the index definition during updates).

Optimization
----------------------------
You can also use this to optimize your models. For example, you might have fields which don't need to be indexed. The more indexes you have the slower ``Model.save()`` will be. Fields that shouldn't be indexed can be specified via ``'unindexed'``:

.. sourcecode:: python

    from myapp.models import MyContact
    
    FIELD_INDEXES = {
        MyContact: {
            'indexed': [...],
            'unindexed': ['creation_date', 'last_modified', ...],
        },
    }

This also has a nice extra advantage: If you specify a ``CharField`` as "unindexed" it will behave like a ``TextField`` and allow for storing strings that are longer than 500 bytes. This can also be useful when trying to integrate 3rd-party apps.

We hope you'll find this feature helpful. Feel free to post sample settings for other reusable Django apps in the comments. See you next time with some very exciting releases!

.. _django-openid-auth: https://launchpad.net/django-openid-auth
.. _djangoappengine: /projects/djangoappengine
