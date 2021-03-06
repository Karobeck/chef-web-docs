.. THIS PAGE IS IDENTICAL TO docs.chef.io/delivery_truck.html BY DESIGN
.. THIS PAGE IS LOCATED AT THE /release/delivery/ PATH.

=====================================================
About the delivery-truck Cookbook
=====================================================

.. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_delivery_truck.rst

.. note:: The ``delivery-truck`` cookbook has a dependency on the ``delivery-sugar`` cookbook, which contains a set of helper methods and custom resources that may be used in build cookbook recipes. Using these helper methods and custom resources in a build cookbook is optional.


delivery-truck Recipes
=====================================================
The following recipes are available by default in the ``delivery-truck`` cookbook:

``default.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_default.rst

``deploy.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_deploy.rst

``functional.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_functional.rst

``lint.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_lint.rst

``provision.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_provision.rst

``publish.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_publish.rst

``quality.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_quality.rst

``security.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_security.rst

``smoke.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_smoke.rst

``syntax.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_syntax.rst

``unit.rb``
   .. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_common_recipe_unit.rst


Create a build-cookbook
=====================================================
.. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_setup.rst

.. note:: This section assumes that Chef Automate is already configured, a project exists, a user may access that project and submit changes, and that all work is being done from that project's root directory.

Edit the Berksfile
-----------------------------------------------------
.. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_setup_berksfile.rst

Edit metadata.rb
-----------------------------------------------------
.. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_setup_metadata.rst

Add delivery-truck to Recipes
-----------------------------------------------------
.. include:: ../../includes_delivery_cookbook/includes_delivery_cookbook_setup_recipes.rst


Read the Tutorial
=====================================================

To learn more about how to set up a project pipeline for a single cookbook and simple application, follow the steps outlined in the `Build a continuous deployment pipeline with Chef Automate <https://learn.chef.io/tutorials/#chef-automate>`__ tutorial on https://learn.chef.io/tutorials/.


Project Cookbooks
=====================================================
A project cookbook is a cookbook that is located within a project and is used to deploy that project's software onto one (or more) nodes in the Chef Automate pipeline. These cookbooks are located in the ``/cookbooks`` directory, which should exist at the root of the project (similar to the ``.delivery`` directory).

The ``default.rb`` recipe in a project cookbook is executed by the chef-client on infrastructure nodes as the project moves through the Chef Automate pipeline. The ``provision.rb`` recipe discovers all ``metadata.rb`` and/or ``metadata.json`` files in the project, including those under the ``/cookbooks`` directory.

Single Cookbook
-----------------------------------------------------
A project may use a single cookbook to tell the chef-client how to configure nodes in the Chef Automate pipeline.

Add Project Cookbook
+++++++++++++++++++++++++++++++++++++++++++++++++++++
Create a project cookbook. From the project's root directory, do the following:

#. Create a branch:

   .. code-block:: bash

      $ delivery checkout BRANCH_NAME

#. Generate a cookbook under ``/cookbooks`` in the project directory:

   .. code-block:: bash

      $ chef generate cookbook PROJECT_NAME

#. Review the ``metadata.rb`` file. It should be similar to:

   .. code-block:: ruby

      name 'my_project'
      maintainer 'The Authors'
      maintainer_email 'you@example.com'
      license 'all_rights'
      description 'Installs/Configures my_project'
      long_description 'Installs/Configures my_project'
      version '0.1.0'

   where ``version '0.1.0'`` must be changed if files within the cookbook change. The version number is what gets promoted through the stages in the Chef Automate pipeline.

Configure default.rb
+++++++++++++++++++++++++++++++++++++++++++++++++++++
In the ``default.rb`` recipe, define how this project is to be deployed. This is a normal Chef recipe that is executed by the chef-client, so do the same in this recipe as you would do in any other.

Promote the Project
+++++++++++++++++++++++++++++++++++++++++++++++++++++
When a change to a project is submitted to Chef Automate, the ``provision.rb`` does the work of promoting the project to the various nodes in the Chef Automate pipeline.

To submit changes to Chef Automate, use commands similar to:

#. Update to match the working tree:

   .. code-block:: bash

      $ git add -A

#. Commit the project:

   .. code-block:: bash

      $ git commit -m "Let us deploy our app."

#. Review the changes in Chef Automate:

   .. code-block:: bash

      $ delivery review

   This command will open the Chef Automate web UI, and then run unit, lint, and syntax tests. After the tests pass, the change may be approved. Once approved, the ``provision.rb`` recipe will deploy the project onto the acceptance stage's infrastructure nodes.

#. After the change has built successfully through the Acceptance stage, approve the changes by clicking the **Deliver** button in the Chef Automate web UI. This sends the project to the Union, Rehearsal, and Delivered stages.

Update the Project
+++++++++++++++++++++++++++++++++++++++++++++++++++++
Update a file in the project, and then update the version number in the ``metadata.rb`` file. This ensures this cookbook is promoted, overwriting the old project cookbook, and then updating the project across each stage of the Chef Automate pipeline:

#. Check out the project from Chef Automate:

   .. code-block:: bash

      $ delivery checkout master

#. Create a branch:

   .. code-block:: bash

      $ delivery checkout BRANCH_NAME

#. Edit the ``version`` in the ``metadata.rb`` file:

   .. code-block:: ruby

      version '0.2.0'

   and then make the desired changes.

#. Update to match the working tree:

   .. code-block:: bash

      $ git add -A

#. Add a commit message:

   .. code-block:: bash

      $ git commit -m "Updated our project's code to version 0.2.0."

#. Review the changes in Chef Automate:

   .. code-block:: bash

      $ delivery review

Multiple Cookbooks
-----------------------------------------------------
Some projects need more than one project cookbook. Put as many cookbooks as necessary under the ``/cookbooks`` directory, which is located at the root of a project.

Each cookbook under the ``/cookbooks`` directory must have a valid cookbook structure. If the cookbook does not have a ``metadata.rb`` or ``metadata.json`` file it will not be discovered by the ``provision.rb`` recipe; consequently, that cookbook will not be used to configure nodes in the Chef Automate pipeline.

The ``default.rb`` recipes in all project cookbooks are executed by the chef-client on infrastructure nodes as the project moves through the Chef Automate pipeline. The ``default.rb`` recipe in the ``build-cookbook`` is run first, and then each ``default.rb`` recipe in each cookbook under ``/cookbooks`` is run (in alphabetical order, by cookbook name).


Project Applications
=====================================================
A project may be a binary, a package, or some other set of arbitrary information. The Chef Automate pipeline supports promoting projects through the pipeline using versioned attributes. This is known as a project application. A project application is a useful way to promote projects by using a set of attributes that are pinned to a specific version, and then using those same versioned attributes when deploying software to various stages in the Chef Automate pipeline.

Configure Project Application
-----------------------------------------------------
Project applications are defined in the ``pubish.rb`` recipe in a ``build-cookbook`` using the ``define_project_application`` helper method, and then in the ``deploy.rb`` recipe using the ``get_projet_application`` method. The publish phase happens at the end of the build stage. It is at this point where the project application version is pinned, uploaded to the Chef server as a data bag item, and then used through the remaining stages.

.. note:: The ``define_project_application`` helper method is available from the ``delivery-sugar`` cookbook, which is a dependency of the ``delivery-truck`` cookbook. This helper is available when the ``publish.rb`` recipe has ``include_recipe 'delivery-truck::publish'`` defined.

To define a project application, do the following:

#. Open the ``publish.rb`` recipe in the ``build-cookbook`` and edit it to contain:

   .. code-block:: ruby

      define_project_application(
        <app_name>,
        <app_version>,
        [ 'attribute',
          'attribute',
          ... ]
      )

   where

   * ``<app_name>`` is the name of the project application
   * ``<app_version>`` is version number to which the project application is pinned
   * ``'attribute'`` is Hash of attributes associated with this version; each attribute is defined as a key-value pair: ``'key = value'``


#. Set up the ``build-cookbook`` to know about this application. Add the following to ``.delivery/build-cookbook/attributes/default.rb``:

   .. code-block:: ruby

      default['delivery']['project_apps'] = ['<app_name>', '<app_name>', ...]

   where ``<app_name>`` is a list of one (or more) applications this ``build-cookbook`` should be aware of.

   .. note:: If the ``/attributes/default.rb`` directory and/or file does not exist, create it.


#. Open the ``default.rb`` recipe in the ``build-cookbook`` and edit it to contain:

   .. code-block:: ruby

      { 'hash_of_attributes' } = get_project_application(<app_name>)

   where ``'hash_of_attributes'`` is a list of one (or more) attributes defined in the ``define_project_application`` block.

   .. note:: Do not pass ``'id'``, ``'version'``, or ``'name'`` as part of the ``'hash_of_attributes'`` as these are already defined in the ``define_project_application`` block, are pulled in automatically by the ``get_project_application`` helper method, and will overwrite any value specified in the Hash.


Example Project Application
-----------------------------------------------------
This example shows how to use project applications to deploy a package into a ``.deb`` file during the deploy phase. (This example assumes a Chef Automate project exists with a properly configured ``build-cookbook``.)

#. Open the ``publish.rb`` recipe in the ``build-cookbook`` and edit to look like the following:

   .. code-block:: ruby

      include_recipe 'delivery-truck::publish'

      # Generate your artifact and document it's location on a download server.
      artifact_location = <generated_artifact_location>

      # It's recommended to generate a checksum from your package too.
      artifact_checksum = <package_checksum>

      # Version the artifact based on the current date.
      artifact_version = Time.now.strftime('%F_%H%M')

      # Name your application.
      name = "<app_name>"

      project_app_attributes = {
        'artifact_location' => artifact_location,
        'artifact_checksum' => artifact_checksum
      }

      define_project_application(
        name,
        artifact_version,
        project_app_attributes
      )

#. In the ``publish.rb`` recipe, update ``<generated_artifact_location>`` and ``<package_checksum>`` to be correct for this project.

#. Set up the ``build-cookbook`` to know about this application. Add the following to ``.delivery/build-cookbook/attributes/default.rb``:

   .. code-block:: ruby

      default['delivery']['project_apps'] = ["<app_name>"]

   where ``<app_name>`` is the same value as the name of the application in the ``publish.rb`` file.

   When the publish phase is run, an application is created, versioned by timestamp, and including all of the information needed to install that version of the application. The provisioning code in ``delivery-truck`` will automatically pin based on this version.

#. Configure the ``build-cookbook`` to know about the application. Add the following to ``.delivery/build-cookbook/attributes/default.rb``:

   .. code-block:: ruby

      default['delivery']['project_apps'] = ["<APPLICATION_NAME>"]

#. Configure the ``build-cookbook`` to know how to install the application. Add the following to ``.delivery/build-cookbook/deploy.rb``:

   .. code-block:: ruby

      app_attributes = get_project_application("<APPLICATION_NAME>")

      # Download your package.
      remote_file "/tmp/latest_package.deb" do
        source   app_attributes['artifact_location']
        checksum app_attributes['artifact_checksum']
        action :create
      end

      # Install it onto your build infrastructure.
      package app_attributes['name'] do
        source "/tmp/latest_package.deb"
        action :install
      end


Validate the Installation
=====================================================
The surest way to validate a Chef Automate installation is to create a cookbook, and then submit it to Chef Automate to kick off a new build in the pipeline.

If a project is a cookbook, we recommend starting with ``delivery-truck``, an open source build cookbook created for driving cookbook pipelines in Chef Automate. You can customize some aspects of ``delivery-truck`` through your project's ``.delivery/config.json``. To have more control or to opt-out of some of the behavior of ``delivery-truck``, create a wrapper build cookbook.

.. include:: ../../includes_delivery/includes_delivery_projects_add_with_delivery_truck.rst

Using ``delivery-truck`` in air-gapped environment
========================================================

Chef Automate can be set up to deploy cookbooks and applications in an air-gapped environment and this section describes 
how to set up a basic cookbook to be delivered through Chef Automate using the `delivery-truck cookbook <https://github.com/chef-cookbooks/delivery-truck>`__
in that environment.

.. note:: By default, the delivery-truck cookbook is configured for use with Chef Automate-backed cookbook projects.

Prerequisites
-----------------------------------------------------

* Ensure you have a private Supermarket installed, setup, and running. See `Install Private Supermarket <https://docs.chef.io/install_supermarket.html>`__ for more information.
* Ensure you have a Chef server with the Chef identify authentication/authorization service configured, a Chef Automate server setup that references your private Supermarket, and at least one Chef Automate build node installed, setup, and running. See `Install Chef Automate <https://docs.chef.io/install_chef_automate.html>`__ and `Chef Identify <https://docs.chef.io/install_supermarket.html#chef-identify>`__ for more information.
* Ensure you have created a project in Chef Automate. Follow these instructions to `Set Up Projects <https://docs.chef.io/delivery_build_cookbook.html#set-up-projects>`__.
* Ensure you have `ChefDK <https://downloads.chef.io/chef-dk/>`__ installed on your `workstation <https://docs.chef.io/workstation.html>`__.


Share cookbooks with your private Supermarket
-----------------------------------------------------

To use ``delivery-truck`` and its dependency, ``delivery-sugar``, you must first share them with a private Supermarket that is authenticated with your Chef server.

#. From a workstation, create a cookbooks directory, ``$COOKBOOKS_DIR``:

   .. code-block:: bash
   
      mkdir -p $COOKBOOKS_DIR

#. Clone the ``delivery-truck`` cookbook and its dependency ``deliver-sugar`` from GitHub:

   .. code-block:: bash
   
      cd $COOKBOOKS_DIR
      git clone https://github.com/chef-cookbooks/delivery-sugar.git
      git clone https://github.com/chef-cookbooks/delivery-truck.git

#. To ensure your private Supermarket does not try to connect to third-party services, log into it and set the ``AIR_GAPPED`` environment variable to ``'true'`` in the ``/etc/supermarket/supermarket.rb`` file.

   .. code-block:: ruby

      default['supermarket']['air_gapped'] = 'true'

#. Save your changes and close the file.
#. Reconfigure your private Supermarket.

   .. code-block:: bash

      supermarket-ctl reconfigure

#. Share the ``delivery-truck`` and ``delivery-sugar`` cookbooks with your private Supermarket using the ``knife`` command-line tool. If you have not configured ``knife`` to share cookbooks with your private Supermarket, see `Upload a Cookbook <https://docs.chef.io/supermarket.html#upload-a-cookbook>`__ before running the following ``knife`` subcommands.

   .. code-block::bash

      knife cookbook site share 'delivery-truck'
      knife cookbook site share 'delivery-sugar'


Generate a cookbook
-----------------------------------------------------

#. On your workstation, use ChefDK's `cookbook generator command <https://docs.chef.io/ctl_chef.html#chef-generate-cookbook>`__ to create a default cookbook directory structure called ``my_cookbook``.

   .. code-block:: bash
      
      chef generate cookbook my_cookbook

#. Run ``delivery init`` in your ``my_cookbook`` local directory to create a new project in Chef Automate and push your first change for review.

   .. code-block:: bash

      cd my_cookbook
      delivery init

#. Finally, check out the added files and commit your changes.


Use the ``delivery-truck`` cookbook in your project
-----------------------------------------------------

From the root of your project's directory, do the following:

#. Modify the build cookbook's Berksfile to reference ``delivery-truck`` and ``delivery-sugar``. By default, this file is located at ``.delivery/build-cookbook/Berksfile``.

   .. code-block:: ruby

      source 'https://your_private_supermarket_url'

      metadata

      group :delivery do
        cookbook 'delivery-sugar'
        cookbook 'delivery-truck'
      end

#. Modify the build cookbook's metadata to include ``delivery-truck``. By default, this file is located at ``.delivery/build-cookbook/metadata.rb``.

   .. code-block:: ruby

      depends 'delivery-truck'

#. Edit your build cookbook's recipes to include the corresponding ``delivery-truck`` recipe.

   .. code-block:: none

      # Cookbook Name:: $BUILD_COOKBOOK_NAME
      # Recipe:: $RECIPE
      #
      # Copyright (c) 2016 The Authors, All Rights Reserved.

      include_recipe "delivery-truck::$RECIPE"

   By default, each build cookbook recipe ``$RECIPE`` is located at ``.delivery/build-cookbook/recipes/$RECIPE.rb``.

#. Increment your build cookbook's version in the cookbook's metadata file.

#. Commit your changes and run ``delivery review``. Changes to your cookbook project can now be managed by your Chef Automate cluster.
