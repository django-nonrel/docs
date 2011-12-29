Building the Django Non-rel docs
================================

This is a placeholder with the original Django Non-rel documentation taken from the All Buttons Pressed Blog. To read it here go to the content/ directory, The _build/ directory has the Sphinx version. 

As of now no editing has been done on the content. So there are numerous references to the blog that are not appropriate (and probably some other stuff as well.)

I suspect that we will want to put the docs with the project components over time. we can then point Sphinx to the right spots. 

To build the docs, first install `Sphinx`_::

	easy_install sphinx

Then build::

	make html

.. _Sphinx: http://sphinx.pocoo.org/index.html