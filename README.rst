==============
typo3extension
==============

A TYPO3 extension kickstart package for usage with composer dependency manager.

.. contents:: :local:

How to use
----------

1. Download the Composer executable

   Assuming you have a download directory for your development tools: `cd` into it
   and download the Composer executable.

   .. code:: sh

   	$ cd /path/to/your/development/downloads/directory
   	$ curl -sS https://getcomposer.org/installer | php

2. Init your new TYPO3 extension project

   Assuming you have a projects root directory, where all your local and/or mirrored
   remote projects reside: `cd` into it and execute the Composer `create-project`
   command.

   .. code:: sh

   	$ cd /path/to/your/projects/root/directory
   	$ php /path/to/your/development/downloads/directory/composer.phar create-project dreadlabs/typo3extension ./YourExtensionKey

3. Kickstart with basic information

   .. code:: sh

   	$ cd YourExtensionKey
   	$ vendor/bin/phing -f build/Kickstart.xml

4. Start coding!

Features
--------

Predefined build targets
~~~~~~~~~~~~~~~~~~~~~~~~

This project package comes with some predefined build targets from best practices
during the work with TYPO3 extensions.

Deployment
''''''''''

.. code:: sh

	$ cd YourExtensionKey
	$ vendor/bin/phing -f build/Deploy.xml -DdeployTo=[targetName]

Where as [targetName] is the base name of one of the property files located at
build/Properties/Targets/.

Create new deployment target
''''''''''''''''''''''''''''

.. code:: sh

	$ cd YourExtensionKey
	$ vendor/bin/phing -f build/NewTarget.xml

Step through the wizard and enter the necessary information to create a new
deployment target property file.

Requirements
------------

Currently this project requires a \*nix machine as it makes usage of some low
level commands like `wget`, `curl`, `tar` (or `unzip`), `mktemp`, `mv` & `rmdir`.

Furthermore, you must install the PHP ssh2 extension (`libssh2-php`) in order
to use the Phing ScpTask.