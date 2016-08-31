.. The contents of this file may be included in multiple topics (using the includes directive).
.. The contents of this file should be modified in a way that preserves its ability to appear in multiple topics.

.. To test the mode for a file:

.. code-block:: ruby

   describe file('/dev') do
    its('mode') { should cmp '00755' }
   end
