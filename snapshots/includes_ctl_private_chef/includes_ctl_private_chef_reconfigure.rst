.. The contents of this file may be included in multiple topics (using the includes directive).
.. The contents of this file should be modified in a way that preserves its ability to appear in multiple topics.


The ``reconfigure`` subcommand is used when changes are made to the private-chef.rb file to reconfigure the server. When changes are made to the private-chef.rb file, they will not be applied to the Enterprise Chef configuration until after this command is run. This subcommand will also restart any services for which the ``service_name['enabled']`` setting is set to ``true``.

This subcommand has the following syntax:

.. code-block:: bash

   $ private-chef-ctl reconfigure



