.. The contents of this file may be included in multiple topics (using the includes directive).
.. The contents of this file should be modified in a way that preserves its ability to appear in multiple topics.

The ``POST`` method is used to create a new data bag item.

This method has no parameters.

**Request**

.. code-block:: none

   POST /organizations/NAME/data/NAME

with a request body similar to:

.. code-block:: javascript

   {
     "id": "adam",
     "real_name": "Adam Jacob"
   }

where ``id`` is required.

**Response**

This method has no response body.

**Response Codes**

.. list-table::
   :widths: 200 300
   :header-rows: 1

   * - Response Code
     - Description
   * - ``200``
     - OK. The request was successful.
   * - ``400``
     - Bad request. The contents of the request are not formatted correctly.
   * - ``401``
     - Unauthorized. The user or client who made the request could not be authenticated. Verify the user/client name, and that the correct key was used to sign the request.
   * - ``403``
     - Forbidden. The user who made the request is not authorized to perform the action.
   * - ``404``
     -  Not found. The requested object does not exist.
   * - ``409``
     - Conflict. The object already exists.
   * - ``413``
     - Request entity too large. A request may not be larger than 1000000 bytes.
