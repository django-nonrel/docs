============
Installation
============

Make sure you've installed the `App Engine SDK`_. On Windows simply use the default installation path. On Linux you can put it in /usr/local/google_appengine. On MacOS it should work if you put it in your Applications folder. Alternatively, on all systems you can add the google_appengine folder to your PATH (not PYTHONPATH) environment variable.

Download the following zip files:

* `django-nonrel <https://github.com/django-nonrel/django-nonrel/zipball/master>`__ (or `clone it <https://github.com/django-nonrel/django-nonrel>`__)
* `djangoappengine <https://github.com/django-nonrel/djangoappengine/zipball/master>`__ (or `clone it <https://github.com/django-nonrel//djangoappengine>`__)
* `djangotoolbox <https://github.com/django-nonrel/djangotoolbox/zipball/master>`__ (or `clone it <https://github.com/django-nonrel/djangotoolbox>`__)
* `django-autoload <http://bitbucket.org/twanschik/django-autoload/get/tip.zip>`__ (or `clone it <https://bitbucket.org/twanschik/django-autoload>`__)
* `django-dbindexer <https://github.com/django-nonrel/django-dbindexer/zipball/master>`__ (or `clone it <https://github.com/django-nonrel/django-dbindexer>`__)
* `django-testapp <https://github.com/django-nonrel/django-testapp/zipball/master>`__ (or `clone it <https://github.com/django-nonrel/django-testapp>`__)

Unzip everything.

The django-testapp folder contains a sample project to get you started. If you want to start a new project or port an existing Django project you can just copy all ".py" and ".yaml" files from the root folder into your project and adapt settings.py and app.yaml to your needs.

Copy the following folders into your project (e.g., django-testapp):

* django-nonrel/django => ``<project>``/django
* djangotoolbox/djangotoolbox => ``<project>``/djangotoolbox
* django-autoload/autoload => ``<project>``/autoload
* django-dbindexer/dbindexer => ``<project>``/dbindexer
* djangoappengine => ``<project>``/djangoappengine

That's it. Your project structure should look like this:

* <project>/django
* <project>/djangotoolbox
* <project>/autoload
* <project>/dbindexer
* <project>/djangoappengine

Alternatively, you can of course clone the respective repositories and create symbolic links instead of copying the folders to your project. That might be easier if you have a lot of projects and don't want to update each one manually.

.. _App Engine SDK: http://code.google.com/appengine/downloads.html
