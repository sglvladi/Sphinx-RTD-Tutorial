Publishing the documentation to ReadTheDocs
===========================================

Now that we have finalised and built our documentation, we can procees to upload it to read the docs.

Pushing our repository
**********************

First, we must push our project to the Git host of our choice, as such:

.. code-block:: bash

    youruser@yourpc:~yourWorkspacePath/simpleble-master$ git add .
    youruser@yourpc:~yourWorkspacePath/simpleble-master$ git commit
    youruser@yourpc:~yourWorkspacePath/simpleble-master$ git push

Importing into ReadTheDocs
**************************

Once you have pushed your changes to your Git host, create an account with `<https://readthedocs.org/>`_ ,if you haven't done so already, and proceed to sign in. Once signed in, there are a number of different ways you can follow to import your project, two of which are described below:

1. Assuming your project is hosted on GitHub, you can proceed by linking your GitHub account to your ReadTheDocs account. By doing so, you can import your project by going on your `ReadTheDocs dashboard <https://readthedocs.org/dashboard/>`_ and clicking the `Import a project <https://readthedocs.org/dashboard/import/?>`_ button. Once there, you will by presented with a list of your GitHub repositories from which you can select the project you want to import by clicking the "+" button next to it's name, in this case ``simpleble``. On the next page, simply press the "Next" button and your project will be imported in ReadTheDocs. 

2. Alternatively, if you do not wish to link your account, you can follow this `link <https://readthedocs.org/dashboard/import/manual/?>`_, which should take you to the "Manual Project Import" page. There you should be presented with a page allowing should specify the name, in this case ``simpleble``, and the URL, in this case `<https://github.com/sglvladi/simpleble>`_, of the repository you wish to import. This method has the disadvantage, that a webhook is NOT auto-generated for you when you import the project, meaning that you will have to do this manually (check `here <http://docs.readthedocs.io/en/latest/webhooks.html>`_), should you wish your documentation to be updated automatically, every time you push to you repository.

Changing the settings
*********************

Once you have followed either of the two approaches of importing your project, outlined above, you should go the the Admin page of your project and under the "Advance Settings" tab, you should specify the path to your ``requirements.txt`` file, relative to you `Repository root` directory. In the case of `simpleble`, since the ``requirements.txt`` was placed in the root, you can simply type "requirements.txt". 

For Python 3.x projects, such as `simpleble`, you should also make sure to select the CPython 3.x Interpreter at the bottom of the "Advanced Settings" tab. If you don't do so, your project will fail to build.

After you press the "Submit" button, you should be redirected to your project "Overview" page, which should specify the build state of your project. If the page says that "Your documentation is building", simply wait for a few minutes and allow ReadTheDocs to build your project. 


Reading the docs
****************
Once this is done, if everything has been done correctly, your project should pass the building process and you should see a big button on the screen with the text "View your documentation". You can now click this button to view your documentation. Typically, your documentation will be hosted under the domain `yourProjectName.readthedocs.io`. In the case of the "simpleble", the built documentation can be found under `simpleble.readthedocs.io <https://simpleble.readthedocs.io/en/latest/>`_.  

Well, I hope you enjoyed the tutorial and that it helped you at least in some aspect of your Sphinx+RTD learning curve. 

Enjoy documenting!

**THE END....** 
