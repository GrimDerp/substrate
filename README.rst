Substrate / Exogorth
=========

This is a fork of the original PyPy artifact known as Substrate which appears to be abandoned. This is currently a
superset of that codebase. The most interesting additions have been in the management commands around `snapdeploy`
which is a version / delivery pipeline based on common `hg` and `git` supported deployment ideas.

*Substrate / Exogorth* is a base application and set of libraries for making
`Google App Engine python`_ development easier. It includes a base app with management
script, testing already set up, a set of common helper functions
(`agar`_), and a serialization library (`restler`_). It also comes with
common libraries like `webapp2`_ ready to go.

Substrate is based on best practices for `Google App Engine python`_ learned in
developing several real-world applications with many users.

We are tired of seeing App Engine frameworks languish unsupported. For
that reason, Substrate is **NOT** a framework. It is a base
application with a set of libraries and helpers. No more, no less.

Resources
---------

* `Documentation`_
* `PyPI Package`_
* `Source Code Repository`_

Development of additions and tests for substrate / exogorth
___________________________________________________________

If you are making changes or additions to substrate proper, this is right repo to clone and test your changes on.
You make changes here locally, install the new version of substrate on your local and test it. Then issue a PR.

If you make changes to anything in the exogorth packages (`agar`_ or `restler`_) make those changes in those respective
repos first, THEN hoist that work here running the update_agar.py and update_restler.py scripts accordingly. Make sure
`agar` and `restler` repos are on the same level as this repo is for it to work properly. You will need to issue separate
PRs for each repo, obviously.

Installation
------------

To install substrate, clone this repo and run

  $ sudo python ./setup.py install

Or use git+git and pip;

  $ sudo -H pip install --upgrade git+git://github.com/agostodev/substrate.git#egg=substrate

Creating a new application
--------------------------

To create a new application, run::

  $ substrate new your-app-id

This will create a new directory ``your-app-id`` and unpack the substrate
application libraries in it. The application name in ``app.yaml`` will be
set to ``your-app-id``.

Or, if you find installing a script to do this for you tedious, you
can clone the `substrate repository`_ and copy the ``app`` directory to
create your application.

Upgrading an existing application
---------------------------------

If you have an existing application, you can upgrade it to the latest
substrate code by updating the substrate package (see `Installation`_) and then running::

   $ substrate update ~/development/your-app-id

where ``~/development/your-app-id`` is the application directory
(the one containing your ``app.yaml`` file) to upgrade.
(For example, you could run this in the current directory with ``.``)

This command will NOT touch any of your application's files. Only
"substrate files" in the ``local/substrate`` and ``lib/substrate`` directories plus
``manage.py`` and ``env_setup.py`` in the application directory will be
overwritten. You can add new files to ``local/usr`` and ``lib/usr``, but do not
edit existing "substrate files" or your changes will be lost when upgrading.

Management Console
------------------

``manage.py`` is a management console for your app. It can invoke several commands.

::

  $ ./manage.py shell

Runs a shell against your local application (uses `iPython`_ if available).

::

  $ ./manage.py rshell

Runs a remote shell against your application on Google App
Engine. To specify a different application ID than what is in your
``app.yaml``, use ``-A``. If your remote API endpoint is not at
the default location, you can pass the path as an argument.

::

  $ ./manage.py test

Runs your application's tests. Any additional parameters are passed to the `unitetest2 discover`_ command

::

   $./manage.py pytest

Runs your tests using the ``py.test`` runner. Any additional parameters are passed to the underlying runner

::

   $./manage.py snapdeploy

Tries to deploy you app and modules to Google AppEngine. Relies on your local git or hg repo to conduct a deploy.
Creates a version with the SHA-1 of the current repo / branch in the cloud and updates a ``snapdeploy.yaml`` file to
commit. Very useful for deploying to multiple app ids and modules with consistent codebases. Supports most options you
can pass to ``appcfg.py``

::

Adding Your Own Commands
------------------------

``manage.py`` will add .py files in the
``local/usr/manage/substrate_manage_usr/commands`` directory as
commands.

Adding New Libraries to Your App
--------------------------------

Substrate stores its libraries in ``lib/substrate`` and
``local/substrate/lib`` (for libraries that should not be deployed to
Google App Engine). Do not add new libraries to these directories as
they are removed and re-copied on upgrade.

User libraries can be placed in ``lib/usr`` and
``local/usr/lib``. These paths are added to ``sys.path`` by
``env_setup.setup``


Testing
-------

As noted above, ``manage.py`` has a ``test`` command. This runs all
the tests in the ``tests`` directory of your application using
`unittest2`_. Included with the Substrate base app is a simple "hello
world" test that you can run to verify your installation. It is
located in ``tests/test_main.py``.

Substrate includes test helpers located in the `agar.test`_
package. ``agar.test`` uses ``google.appengine.ext.testbed`` to set up your
Google App Engine environment fresh before each test run. It is mostly
API compatable with the old `gaetestbed`_ project, plus new additions.

License
-------

Substrate is mostly a packaging of other libraries, which have their
own licenses. Original code in Substrate is under the `MIT license`_.

.. Links

.. _Documentation: http://packages.python.org/substrate-exogorth

.. _PyPI Package: http://pypi.python.org/pypi/substrate-exogorth
.. _download the PyPI package: http://pypi.python.org/pypi/substrate-exogorth#downloads

.. _Source Code Repository: https://github.com/agostodev/substrate-exogorth/substrate
.. _substrate repository: https://github.com/agostodev/substrate-exogorth/substrate

.. _agar: http://packages.python.org/substrate-exogorth/agar.html
.. _restler: http://packages.python.org/substrate-exogorth/restler.html

.. _Google App Engine python: http://code.google.com/appengine/docs/python/overview.html

.. _unittest2: http://pypi.python.org/pypi/unittest2
.. _unitetest2 discover: http://docs.python.org/library/unittest.html#test-discovery

.. _webapp2: http://code.google.com/p/webapp-improved/

.. _iPython: http://ipython.org/

.. _MIT License: http://www.opensource.org/licenses/mit-license.php

.. _agar.test: http://packages.python.org/agar/agar.html#module-agar.test

.. _gaetestbed: https://github.com/jgeewax/gaetestbed
