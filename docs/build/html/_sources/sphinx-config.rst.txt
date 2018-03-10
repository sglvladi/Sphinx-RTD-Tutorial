Sphinx Configuration
====================
Previously, we saw that ``conf.py`` is the configuration file that Sphinx calls whenever a ``make`` command is called. Now it's time we made some configuration changes. Let's locate and edit the ``conf.py`` file:

.. code-block:: bash

   youruser@yourpc:~yourWorkspacePath/simpleble-master/docs$ cd source
   youruser@yourpc:~yourWorkspacePath/simpleble-master/docs/source$ nano conf.py 

There are two things we need to tweak, while we have ``conf.py`` open, before we proceed to build our documentation.

Theme configuration
*******************
The first thing we want to do is to configure Sphinx to use the RTD theme we previously installed (see :ref:`installation` step). We do this by finding the line that sets the configuration variable ``html_theme`` and we modify it as follows:

.. code-block:: python

    html_theme = 'sphinx_rtd_theme'

Note that there exist numerous other nice themes out there, so if you're not particularly interested in publishing to RTD then you can choose (or even create) any theme that suits your needs. We chose ``sphinx_rtd_theme`` since we do want to publish to RTD, plus it is generally quite a nice and clear theme to read.

Autodoc configuration
*********************
Continuing, if we want Sphinx to autogenerate documentation from the comments of our code using the ``autodoc`` extension, we have to point Sphinx to the directory in which our Python source codes reside. This can be done by uncommenting and altering the following lines, which are generally found at the top of the file:

 .. code-block:: python
    :emphasize-lines: 3

    import os
    import sys
    sys.path.insert(0, os.path.abspath('../../simpleble/'))

Notice how we altered the path specified in line 3 (highlighted). This is because our ``conf.py`` file is located in ``simpleble-master/docs/source``, while our Python source codes, in this case the file ``simpleble.py``, are located inside ``simpleble-master/simpleble``. This means that when Sphinx is looking for our source codes, it now knows that it must go two directories up from ``conf.by`` (indicated by the ``../../`` part of the absolute path) and inside the folder ``simpleble-master/simpleble``. This is equivalent to adding the directory of our Python source files to PYTHONPATH. 

**It is important to note here that the absolute path must be specified in relation to where** ``conf.py`` **resides, i.e. our `Sphinx source root`, rather than in relation to the `Documentation root`**.

Next, we proceed by creating a ``requirements.txt`` file in the root directory of our repository, which lists any dependecies of our package. In our case, ``simpleble`` depends on a package named ``bluepy`` which can be generally installed by calling ``sudo pip install bluepy``. 

Create and open the ``requirements.txt`` file:

.. code-block:: bash

   youruser@yourpc:~yourWorkspacePath/simpleble-master/docs/source$ cd ../..
   youruser@yourpc:~yourWorkspacePath/simpleble-master$ nano requirements.txt

Then type the following on the first line of the file:

.. code-block:: python
    
    bluepy

Save and close the file (Ctrl+X -> Enter -> Y -> Enter).

Apart from specifying the dependencies of our package, the ``requirements.txt`` file can be used to batch install any such dependencies by calling ``sudo pip install -r requirements.txt``. For more information on requirements files, you can have a look at `pip's documentation <https://pip.pypa.io/en/stable/user_guide/#requirements-files>`_. 

We will later also use the ``requirements.txt`` file when configuring the building process of our ReadTheDocs documentation. If we didn't have the requirements file, then ReadTheDocs would fail to run ``autodoc`` in order to generate our documentation from the comments in our source code. In other words, when building our documentation, RTD calls a ``pip install -r requirements.txt``, which installs any requirements in a virtual environment, which is then where ``sphinx-apidoc`` is run.