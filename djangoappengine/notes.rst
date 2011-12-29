=====
Notes
=====

Zip packages
---------------------------------------------
**Important:** Your instances will load slower when using zip packages because zipped Python files are not precompiled. Also, i18n doesn't work with zip packages. Zipping should only be a **last resort**! If you hit the 3000 files limit you should better try to reduce the number of files by, e.g., deleting unused packages from Django's "contrib" folder. Only when **nothing** (!) else works you should consider zip packages.

Since you can't upload more than 3000 files on App Engine you sometimes have to create zipped packages. Luckily, djangoappengine can help you with integrating those zip packages. Simply create a "zip-packages" directory in your project folder and move your zip packages there. They'll automatically get added to ``sys.path``.

In order to create a zip package simply select a Python package (e.g., a Django app) and zip it. However, keep in mind that only Python modules can be loaded transparently from such a zip file. You can't easily access templates and JavaScript files from a zip package, for example. In order to be able to access the templates you should move the templates into your global "templates" folder within your project before zipping the Python package.
