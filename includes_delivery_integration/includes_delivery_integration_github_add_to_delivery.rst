.. The contents of this file may be included in multiple topics (using the includes directive).
.. The contents of this file should be modified in a way that preserves its ability to appear in multiple topics.


To add the |github| |oauth| app to |delivery|, log in to the |delivery| server and run the following command:

**For GitHub Enterprise**

.. code-block:: bash

   $ delivery-ctl setup-github-enterprise $GHE_SERVER_ROOT_URL $CLIENT_ID $CLIENT_SECRET

**For GitHub.com**

.. code-block:: bash

   $ delivery-ctl setup-github $CLIENT_ID $CLIENT_SECRET
