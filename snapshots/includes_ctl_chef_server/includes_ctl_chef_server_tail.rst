.. The contents of this file may be included in multiple topics (using the includes directive).
.. The contents of this file should be modified in a way that preserves its ability to appear in multiple topics.


The ``tail`` subcommand is used to follow all of the Chef server logs for all services. This command can also be run for an individual service by specifying the name of the service in the command. 

This subcommand has the following syntax:

.. code-block:: bash

   $ chef-server-ctl tail SERVICE_NAME

where ``SERVICE_NAME`` represents the name of any service that is listed after running the ``service-list`` subcommand.
