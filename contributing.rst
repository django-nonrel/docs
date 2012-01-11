How to Contribute
=================

We'd love to see you getting involved in Django-nonrel development!

Here are some ideas on how you can help evolve this project:

* :ref:`Send us feedback <contributing/mailing-list>`! Tell us about your good or bad
  experiences with Django-nonrel -- what did you like and what should be improved?
* Blog/write about using Django-nonrel and :ref:`let us know about your work <contributing/mailing-list>`.
* Help solving other people's problems on :ref:`the mailing list <contributing/mailing-list>`.
* Fix and improve this documentation. These documents are probably full of typos and
  weird, ungrammatical phrasings.  Also, if you're missing something from this
  documentation, please tell us!
* :ref:`Report bugs and feature requests <contributing/bug-reports>`.
* :ref:`Send patches or pull requests <contributing/patches>` containing bug
  fixes, new features or code improvement.
* Join the team and help with maintaining and :ref:`developing <contributing/development>` one of the projects .

.. _contributing/mailing-list:

Mailing List
------------
Our `mailing list`_, ``django-non-relational@googlegroups.com``, is the right
place for general feedback, discussion and support.

.. _contributing/bug-reports:

Bug Reports
...........
Bugs can be reported to our `ticket tracker on GitHub`_.

.. _contributing/patches:

Patches
.......
The most comfortable way to get your changes into Django-nonrel is to
use `GitHub's pull requests`_. It's perfectly fine, however, to send regular
patches to :ref:`the mailing list <contributing/mailing-list>`.

.. _contributing/development:

Development
-----------
Django-nonrel is being developed `on GitHub`_. We use `git flow`_ branching model.
Each repository has 'master' and 'develop' branches, the former is used for releases
and can be used in production, while the latter  may contain experimental features
from time to time.

Push new code to a feature branch and create a pull request, so
others can review and test your changes before they are merged into 'develop'.

You could do the following to setup your coding environment::

    git clone git@github.com:django-nonrel/django-nonrel
    git checkout master
    git flow init -d

To start a work on a fix or feature::

    git flow feature start <name>

Now do the work::

    git commit -a -m "..."

Push the branch::

    git push origin feature/<name>

...and create a pull request when it's ready.


Please, use `Django's coding style`_ in your code (this includes using `PEP 8`_ as a guideline).


.. _mailing list: http://groups.google.com/group/django-non-relational
.. _on GitHub: https://github.com/django-nonrel
.. _ticket tracker on GitHub: https://github.com/organizations/django-nonrel/dashboard/issues/repos?state=open
.. _GitHub's pull requests: http://help.github.com/pull-requests/
.._ git flow: http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/
.. _Django's coding style: https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/
.. _PEP 8: http://www.python.org/dev/peps/pep-0008
