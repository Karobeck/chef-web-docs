.. The contents of this file may be included in multiple topics (using the includes directive).
.. The contents of this file should be modified in a way that preserves its ability to appear in multiple topics.

This resource has the following attributes:

``additional_remotes``
   **Ruby Type:** Hash

   An array of additional remotes that are added to the git repository configuration.

``depth``
   **Ruby Type:** Integer

   The number of past revisions to be included in the git shallow clone. The default behavior will do a full clone.

``destination``
   **Ruby Type:** String

   The location path to which the source is to be cloned, checked out, or exported. Default value: the ``name`` of the resource block See "Syntax" section above for more information.

``enable_submodules``
   **Ruby Types:** TrueClass, FalseClass

   Perform a sub-module initialization and update. Default value: ``false``.

``group``
   **Ruby Types:** String, Integer

   The system group that is responsible for the checked-out code.

``ignore_failure``
   **Ruby Types:** TrueClass, FalseClass

   Continue running a recipe if a resource fails for any reason. Default value: ``false``.

``notifies``
   **Ruby Type:** Symbol, 'Chef::Resource[String]'

   .. include:: ../../includes_resources_common/includes_resources_common_notification_notifies.rst

   .. include:: ../../includes_resources_common/includes_resources_common_notification_timers_12-5.rst

   .. include:: ../../includes_resources_common/includes_resources_common_notification_notifies_syntax.rst

``provider``
   **Ruby Type:** Chef Class

   Optional. Explicitly specifies a provider.

``reference``
   **Ruby Type:** String

   The alias for revision.

``remote``
   **Ruby Type:** String

   The remote repository to use when synchronizing an existing clone.

``repository``
   **Ruby Type:** String

   The URI for the git repository.

``retries``
   **Ruby Type:** Integer

   The number of times to catch exceptions and retry the resource. Default value: ``0``.

``retry_delay``
   **Ruby Type:** Integer

   The retry delay (in seconds). Default value: ``2``.

``revision``
   **Ruby Type:** String

   A branch, tag, or commit to be synchronized with git. This can be symbolic, like ``HEAD`` or it can be a source control management-specific revision identifier. Default value: ``HEAD``.

   .. include:: ../../includes_resources/includes_resource_scm_git_attributes_revision.rst

``ssh_wrapper``
   **Ruby Type:** String

   The path to the wrapper script used when running SSH with git. The ``GIT_SSH`` environment variable is set to this.

``subscribes``
   **Ruby Type:** Symbol, 'Chef::Resource[String]'

   .. include:: ../../includes_resources_common/includes_resources_common_notification_subscribes.rst

   .. include:: ../../includes_resources_common/includes_resources_common_notification_timers_12-5.rst

   .. include:: ../../includes_resources_common/includes_resources_common_notification_subscribes_syntax.rst

``user``
   **Ruby Types:** String, Integer

   The system user that is responsible for the checked-out code.
