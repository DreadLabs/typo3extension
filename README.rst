==============
typo3extension
==============

A TYPO3 extension kickstart package for usage with composer dependency manager.

.. contents:: :local:

How to use
----------

1. Download the Composer executable

   Assuming you have a download directory for your development tools: `cd`:code: into it
   and download the Composer executable.

   .. code:: sh

   	$ cd /path/to/your/development/downloads/directory
   	$ curl -sS https://getcomposer.org/installer | php

2. Init your new TYPO3 extension project

   Assuming you have a projects root directory, where all your local and/or mirrored
   remote projects reside: `cd`:code: into it and execute the Composer `create-project`:code:
   command.

   .. code:: sh

   	$ cd /path/to/your/projects/root/directory
   	$ php /path/to/your/development/downloads/directory/composer.phar create-project dreadlabs/typo3extension ./YourExtensionKey

3. Kickstart with basic information

   .. code:: sh

   	$ cd YourExtensionKey
   	$ vendor/bin/phing -f build/Kickstart.xml

4. Initial commit

   .. code:: sh

   	$ git add . && git commit -m '[INIT] initial commit'

5. Start coding!

Features
--------

Predefined build targets
~~~~~~~~~~~~~~~~~~~~~~~~

This project package comes with some predefined build targets from best practices
during the work with TYPO3 extensions.

Deployment
''''''''''

**Note**: Ensure the desired target in `build/Properties/Target[targetName].properties`:code: is properly configured.

.. code:: sh

	$ cd YourExtensionKey
	$ vendor/bin/phing -f build/Deploy.xml -DdeployTo=[targetName]

Where as `[targetName]`:code: is the base name of one of the property files located at
*build/Properties/Targets/*.

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
level commands like `wget`:code:, `curl`:code:.

I suggest you to install the PHP ssh2 extension (`libssh2-php`:code:) in order
to make use of the PHP implementation for ssh/scp/sftp commands. If you can't
install this PECL extension, this package falls back to `phpseclib/phpseclib`:code:
package.

Configuration
-------------

Main configuration
~~~~~~~~~~~~~~~~~~

Can be changed in *build/Properties/Build.properties*. The following properties
influence the deployment process.

* **typo3.cms.downloader** - *(string)* - Defines the downloader tool.

  Valid values: `wget`:code:, `curl`:code:

* **typo3.cms.install** - *(boolean)* - Flags if TYPO3 must be installed.

  Default: `true`:code:

* **typo3.cms.flavor** - *(string)* - Specifies the package to download.

  - *Dummy package doesn't include symlinks.*
  - *Leave empty performs a source only download.*

  Default: empty

  Valid values: `dummy`:code:, `blank`:code:, `government`:code:, `introduction`:code:

* **typo3.cms.version** - *(string)* - TYPO3 CMS version to download

  Example: `6.0`:code:

* **typo3.cms.format** - *(string)* - Download package format

  *Also flags if symlinks should be used (zip = no symlinks)*

  Default: empty (means `tar`:code:)

  Valid values: `tar`:code:, `zip`:code:

* **typo3.cms.defaultConfigurationDirectory** - *string* - Default configuration file path relative to build dir.

  Default: `../www/t3lib/stddb/`:code:

* **typo3.cms.defaultConfigurationFile** - *string* - Default configuration file name

  Default: `DefaultConfiguration.php`:code:

* **typo3.cms.enableInstallTool** - *boolean* - Flags if the install tool should be enabled

  Default: `true`:code:

* **build.cache.dir** - *string* - Build cache directory relative to build directory

  *Downloaded packages or remote LocalConfiguration.php gets cached here.*

  Default: `../.build-cache/`:code:

* **build.cache.package** - *string* - Build cache package path & name

  Default: `${build.cache.dir}typo3cms${typo3.cms.version}.pkg`:code:

* **target.current.dir** - *string* - Symlink name at target machine which gets updated on deployment

  Default: `current`:code:

Target configuration
~~~~~~~~~~~~~~~~~~~~

Can be changed in *build/Properties/Targets/\*.properties*. Please note to leave
*NewTarget.properties* unchanged as this is the template for the NewTarget build
project.

All target properties get prefixed by `target.`:code: during the deployment process.

* **hostname** - *string* - Name (or IP address) of the target machine
* **port** - *integer* - Port number of the target machine

  *Used during deployment via scp/ssh*

  Example: 22

* **username** - *string* - Target machine authentification user name.
* **password** - *string* - Target machine authentification password.
* **path** - *string* - Target machine deployment path

  *The contents of www/ will be copied into this directory*

* **symlink.typo3_src** - *string* - Specifies the typo3_src symlink target

  *Specify a path to:*

  1. move typo3_src out of the Document Root (blank package)
  2. change typo3_src symlink (dummy package)

  *Set this to an empty value to not change anything regarding the typo3_src symlink.*

* **symlink.index_php** - *boolean* - Flags if the index.php symlink should be used.

  *On some systems the index.php may not be symlinked.*

  `false`:code:: remove symlink, replace with index.php from typo3_src folder/symlink.

  `true`:code:: leave symlink

* **db.host** - *string* - Hostname of the targets DBMS

  Example: `127.0.0.1`

* **db.name** - *string* - Database name of the target
* **db.username** - *string* - Database user name of the target
* **db.password** - *string* - Database password of the target

Assumptions
-----------

You've chosen this project package to create and deploy your TYPO3 CMS extension
with the help of some best practices which emerged by some years of experience
during the work with this fantastic PHP application.

This project package is "opinionated software", which means it has very firm ideas
about how things ought to be done, and tries to force those ideas on you. Some of
the assumptions behind these opinions are:

- You are using SSH to access the remote servers.
- You're using `git`:code: to accomplish source code management tasks.
- Deployment is only possible if you have committed your work and created an
  appropriate tag. The latter is not required as a deployment also can be done
  based upon a SHA1 commit object.
