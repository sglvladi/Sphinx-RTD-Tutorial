.. _installation:

Installation
============

First we must make sure we have ``Sphinx`` installed:

.. code-block:: bash

   $ (sudo) pip install sphinx

Also, since this tutorial aims to show how to publish project documentation on ReadTheDocs, then we will also need to download the RTD sphinx theme, as so:

.. code-block:: bash

   $ (sudo) pip install sphinx-rtd-theme

For the purposes of this tutorial, the ``simpleble`` package, which we will be documenting, depends on another package named ``bluepy``. In order for ``Sphinx`` to auto-generate documentation from our comments/docstrings, it must also be able to build our package, and thus we must have any dependencies installed and made visible to ``Sphinx``. For this reason, and **ONLY** for this specific tutorial, we need to install ``bluepy``, as so:

.. code-block:: bash

   $ (sudo) pip install bluepy

Note that as long as we used the same ``pip`` version to install both ``Sphinx`` and ``bluepy``, then they will both be added to the same ``PYTHONPATH``, meaning that ``Sphinx`` can see ``blueby`` (at least on our local machine). A common mistake that people do is to, for example, use ``pip3`` to install one of the two and ``pip`` for the other, in which case the above requirement will not be satisfied as the two packages are installed for different versions of Python and thus are added to a different ``PYTHONPATH``.


