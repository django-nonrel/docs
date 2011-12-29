===================
Management commands
===================

Overview
-----------------------------------------

You can directly use Django's manage.py commands. For example, run ``manage.py createsuperuser`` to create a new admin user and ``manage.py runserver`` to start the development server.

**Important:**  Don't use dev_appserver.py directly. This won't work as expected because ``manage.py runserver`` uses a customized dev_appserver.py configuration. Also, never run ``manage.py runserver`` together with other management commands at the same time. The changes won't take effect. That's an App Engine SDK limitation which might get fixed in a later release.

With djangoappengine you get a few extra manage.py commands:

* ``manage.py remote`` allows you to execute a command on the production database (e.g., ``manage.py remote shell`` or ``manage.py remote createsuperuser``)
* ``manage.py deploy`` uploads your project to App Engine (use this instead of ``appcfg.py update``)

Note that you can only use ``manage.py remote`` if your app is deployed and if you have enabled authentication via the Google Accounts API in your app settings in the App Engine Dashboard. Also, if you use a custom app.yaml you have to make sure that it contains the remote_api handler.

High-replication datastore settings
-------------------------------------------
In order to use ``manage.py remote`` with the high-replication datastore you need to add the following to the top of your ``settings.py``:

.. sourcecode:: python

    from djangoappengine.settings_base import *
    DATABASES['default']['HIGH_REPLICATION'] = True
