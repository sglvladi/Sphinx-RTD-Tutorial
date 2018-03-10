Project setup
=============

Create a repository and clone it
********************************
So we begin by creating a Git repository and adding the ``README.md``, ``LICENSE`` and ``.gitignore`` files, which are of no importance to this tutorial but are generally standard for Git repos. Now on our local machine we proceed by cloning the repository:

.. code-block:: bash

   youruser@yourpc:~yourWorkspacePath$ (sudo) git clone https://github.com/sglvladi/simpleble

Once the cloning is complete, we can check to see what files we have in our repo:

.. code-block:: bash
   :emphasize-lines: 3

   youruser@yourpc:~yourWorkspacePath$ cd simpleble-master
   youruser@yourpc:~yourWorkspacePath/simpleble-master$ ls
   LICENSE  README.md

Include your source codes
*************************
Now you can add your package source codes to your repository folder. For this tutorial, we only have a single source file ``simpleble.py``, which will need to be placed inside a ``simpleble`` folder under our `Repository root`.

.. code-block:: bash
   :emphasize-lines: 4,6

   youruser@yourpc:~yourWorkspacePath/simpleble-master$ mkdir simpleble
   youruser@yourpc:~yourWorkspacePath/simpleble-master$ cp -R <path-to-source-folder>/simpleble.py ./simpleble
   youruser@yourpc:~yourWorkspacePath/simpleble-master$ ls
   LICENSE  README.md simpleble
   youruser@yourpc:~yourWorkspacePath/simpleble-master$ ls ./simpleble/
   simpleble.py
