.. The contents of this file may be included in multiple topics (using the includes directive).
.. The contents of this file should be modified in a way that preserves its ability to appear in multiple topics.

The ``GET`` method is used to get a list of users that belong to a group.

This method has no parameters.

**Request**

.. code-block:: none

   GET /organizations/NAME/groups/GROUP_NAME

**Response**

The response is similar to:

.. code-block:: javascript

   {
     "actors": [
       "pivotal",
       "grantmc"
     ],
     "users": [
     "pivotal",
       "grantmc"
     ],
     "clients": [
     
     ],
     "groups": [
     
     ],
     "orgname": "inbetweens",
     "name": "admins",
     "groupname": "admins"
   }

**Response Codes**

.. list-table::
   :widths: 200 300
   :header-rows: 1

   * - Response Code
     - Description
   * - ``200``
     - OK. The request was successful.
   * - ``401``
     - Unauthorized. The user or client who made the request could not be authenticated. Verify the user/client name, and that the correct key was used to sign the request.
   * - ``403``
     - Forbidden. The user who made the request is not authorized to perform the action.
   * - ``404``
     - Not found. The requested object does not exist.
