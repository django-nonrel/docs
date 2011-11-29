HTML5 offline manifests with django-mediagenerator
========================================================

This is actually part 3 of our django-mediagenerator_ Python canvas app series (see `part 1`_ and `part 2`_), but since it has nothing to do with client-side Python we name it differently. In this part you'll see how to make your web app load without an Internet connection. HTML5 supports offline web apps through manifest_ files.

Manifest files
---------------------------------
First here's some background, so you know what a manifest file is. A manifest file is really simple. In its most basic form it lists the URLs of the files that should be cached. Here's an ``example.manifest``:

.. sourcecode:: text

    CACHE MANIFEST
    /media/main.css
    /media/main.js

The first line is always ``CACHE MANIFEST``. The next lines can list the files that should be cached. In this case we've added the ``main.css`` and ``main.js`` bundles. Additionally, the main HTML file which includes the manifest is cached, automatically. You can include the manifest in the ``<html>`` tag:

.. sourcecode:: html

    <html manifest="example.manifest">

When the browser sees this it loads the manifest and adds the current HTML and manifest file and all files listed in the manifest to the cache. The next time you visit the page the browser will try to load the manifest file from your server and compare it to the cached version. If the content of the manifest file hasn't changed the browser just loads all files from the cache. If the content of the manifest file has changed the browser refreshes its cache.

This is important, so I repeat it: The browser updates its cache only when the **content** of the **manifest** file is modified. Changes to your JavaScript, CSS, and image files will go unnoticed if the manifest file is not changed! That's exactly where things become annoying. Imagine you've changed the ``main.js`` file. Now you have to change your manifest file, too. One possibility is to add a comment to their manifest file which represents the current version number:

.. sourcecode:: text

    CACHE MANIFEST
    # version 2
    /media/main.css
    /media/main.js

Whenever you change something in your JS or CSS or image files you have to increment the version number, manually. That's not really nice.

django-mediagenerator to the rescue
-------------------------------------------
This is where the media generator comes in. It automatically modifies the manifest file whenever your media files are changed. Since media files are versioned automatically by django-mediagenerator the version hash in the file name serves as a natural and automatic solution to our problem. With the media generator a manifest file could look like this:

.. sourcecode:: text

    CACHE MANIFEST
    /media/main-bf1e7dfbd511baf660e57a1f36048750f1ee660f.css
    /media/main-fb16702a27fc6c8073aa4df0b0b5b3dd8057cc12.js

Whenever you change your media files the version hash of the affected files becomes different and thus the manifest file changes automatically, too.

Now how do we tell django-mediagenerator_ to create such a manifest file? Just add this to your ``settings.py``:

.. sourcecode:: python

    OFFLINE_MANIFEST = 'webapp.manifest'

With this simple snippet the media generator will create a manifest file called ``webapp.manifest``. However, the manifest file will contain **all** of the assets in your project. In other words, the whole ``_generated_media`` folder will be listed in the manifest file.

Often you only want specific files to be cached. You can do that by specifying a list of regular expressions matching path names (relative to your media directories, exactly like in ``MEDIA_BUNDLES``):

.. sourcecode:: python

    OFFLINE_MANIFEST = {
        'webapp.manifest': {
            'cache': (
                r'main\.css',
                r'main\.js',
                r'webapp/img/.*',
            ),
            'exclude': (
                r'webapp/img/online-only/.*',
            )
        },
    }

Here we've added the ``main.css`` and ``main.js`` bundles and all files under the ``webapp/img/`` folder, except for files under ``webapp/img/online-only/``. Also, you might have guessed it already: You can create multiple manifest files this way. Just add more entries to the ``OFFLINE_MANIFEST`` dict.

Finally, we also have to include the manifest file in our template:

.. sourcecode:: django

    {% load media %}
    <html manifest="{% media_url 'webapp.manifest' %}">

Manifest files actually provide more features than this. For example, you can also specify ``FALLBACK`` handlers in case there is no Internet connection. In the following example the "/offline.html" page will be displayed for resources which can't be reached while offline:

.. sourcecode:: python

    OFFLINE_MANIFEST = {
        'webapp.manifest': {
            'cache': (...),
            'fallback': {
                '/': '/offline.html',
            },
        },
    }

Here ``/`` is a pattern that matches all pages. You can also define ``NETWORK`` entries which specify allowed URLs that can be accessed even though they're not cached:

.. sourcecode:: python

    OFFLINE_MANIFEST = {
        'webapp.manifest': {
            'cache': (...),
            'network': (
                '*',
            ),
        },
    }

Here ``*`` is a wildcard that allows to access any URL. If you just had an empty ``NETWORK`` section you wouldn't be able to load uncached files, even when you're online (however, not all browsers are so strict).

Serving manifest files
---------------------------------------------------
Manifest files should be served with the MIME type ``text/cache-manifest``. Also it's **critical** that you disable HTTP caching for manifest files! Otherwise the browser will **never** load a new version of your app because it always loads the cached manifest! Make sure that you've configured your web server correctly.

As an example, on App Engine you'd configure your ``app.yaml`` like this:

.. sourcecode:: yaml

    handlers:
    - url: /media/(.*\.manifest)
      static_files: _generated_media/\1
      mime_type: text/cache-manifest
      upload: _generated_media/(.*\.manifest)
      expiration: '0'

    - url: /media
      static_dir: _generated_media/
      expiration: '365d'

Here we first catch all manifest files and serve them with an expiration of "0" and the correct MIME type. The normal ``/media`` handler must be installed **after** the manifest handler.

Like a native iPad/iPhone app
----------------------------------------
Offline-capable web apps have a nice extra advantage: We can put them on the iPad's/iPhone's home screen, so they appear exactly like native apps! All browser bars will disappear and your whole web app will be full-screen (except for the top-most status bar which shows the current time and battery and network status). Just add the following to your template:

.. sourcecode:: django

    <head>
    <meta name="apple-mobile-web-app-capable" content="yes" />
    ...

Now when you're in the browser you can tap on the "+" icon in the middle of the bottom toolbar (**update:** I just updated to iOS 4.2.1 and the "+" icon got replaced with some other icon, but it's still in the middle of the bottom toolbar :) and select "Add to Home Screen":

.. image:: http://lh3.ggpht.com/_03uxRzJMadw/TOfkL5YEULI/AAAAAAAAAIo/sCOT_u4ymdQ/add-to-home-screen.png

Then you can enter the name of the home screen icon:

.. image:: http://lh4.ggpht.com/_03uxRzJMadw/TOfkLpUSIeI/AAAAAAAAAIk/n3IZTgfZyIo/add-to-home.png

Tapping "Add" will add an icon for your web app to the home screen:

.. image:: http://lh3.ggpht.com/_03uxRzJMadw/TOfkMLPDyQI/AAAAAAAAAIw/qducGXp4DzE/app-on-home-screen.png

When you tap that icon the canvas demo app starts in full-screen:

.. image:: http://lh5.ggpht.com/_03uxRzJMadw/TOfkLyiW0SI/AAAAAAAAAIs/lOIzhyI6BMQ/app.png

We can also specify an icon for your web app. For example, if your icon is in ``img/app-icon.png`` you can add it like this:

.. sourcecode:: django

    {% load media %}
    <head>
    <link rel="apple-touch-icon" href="{% media_url 'img/app-icon.png' %}" />
    ...

The image should measure 57x57 pixels.

Finally, you can also add a startup image which is displayed while your app loads. The following snippet assumes that the startup image is in ``img/startup.png``:

.. sourcecode:: django

    {% load media %}
    <head>
    <link rel="apple-touch-startup-image" href="{% media_url 'img/startup.png' %}" />
    ...

The image dimensions should be 320x460 pixels and it should be in portrait orientation.

Summary
--------------------------------
* The manifest file just lists the files that should be cached
* Files are only reloaded if the manifest file's content has changed
* The manifest file must not be cached (!) or the browser will never reload anything
* django-mediagenerator_ automatically maintains the manifest file for you
* Offline web apps can appear like native apps on the iPad and iPhone
* Download_ the latest canvas drawing app source which is now offline-capable

As you've seen in this post, it's very easy to make your web app offline-capable with django-mediagenerator_. This is also the foundation for making your app look like a native app on the iPhone and iPad. Offline web apps open up exciting possibilities and allow you to become independent of Apple's slow approval processes for the app store and the iOS platform in general because web apps can run on Android, webOS, and many other mobile platforms. It's also possible to write a little wrapper for the App Store which just opens Safari with your website. That way users can still find your app in the App Store (in addition to the web).

The next time you want to write a native app for the iOS platform, consider making a web app, instead (unless you're writing e.g. a real-time game, of course).

.. _part 1: /blog/django/2010/11/Offline-HTML5-canvas-app-in-Python-with-django-mediagenerator-Part-1-pyjs
.. _part 2: /blog/django/2010/11/Offline-HTML5-canvas-app-in-Python-with-django-mediagenerator-Part-2-Drawing
.. _manifest: http://www.w3.org/TR/html5/offline.html
.. _django-mediagenerator: /projects/django-mediagenerator
.. _download: http://bitbucket.org/wkornewald/offline-canvas-python-web-app/get/tip.zip
