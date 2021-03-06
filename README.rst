OAB-Java
========
oab-java.sh v0.2.6 - Create a local 'apt' repository for Sun Java 6 and/or Oracle Java 7 packages.

Copyright (c) Martin Wimpress, http://flexion.org. MIT License

By running this script to download Java you acknowledge that you have
read and accepted the terms of the Oracle end user license agreement.

* http://www.oracle.com/technetwork/java/javase/terms/license/

Usage
-----
::

  sudo ./oab-java.sh

Optional parameters

* -7              : Build oracle-java7 packages instead of sun-java6
* -c              : Remove pre-existing packages from /var/local/oab/deb
* -k <gpg-key-id> : Use the specified existing key instead of generating one
* -s              : Skip building if the packages already exist
* -h              : This help

How do I download and run this thing?
-------------------------------------
Like this.
::

	cd ~/
	wget https://raw.github.com/raziel23x/oab-java6/raw/0.2.7/oab-java.sh -O oab-java.sh
  
	chmod +x oab-java.sh
  
	sudo ./oab-java.sh

If you are behind a proxy you may need to run using:
::

  sudo -i ./oab-java.sh

If you want to see what this script is doing while it is running then execute
the following from another shell:
::

  tail -f ./oab-java.sh.log

How it works
------------
This script is merely a wrapper for the most excellent Debian packaging
scripts prepared by Janusz Dziemidowicz.

* https://github.com/rraptorr/sun-java6
* https://github.com/rraptorr/oracle-java7

The basic execution steps are:

* Remove, my now disabled, Java PPA 'ppa:flexiondotorg/java'.
* Install the tools required to build the Java packages.
* Create download cache in /var/local/oab/pkg.
* Download the i586 and x64 Java install binaries from Oracle. Yes, both are required.
* Clone the build scripts from https://github.com/rraptorr/
* Build the Java packages applicable to your system.
* Create local apt repository in /var/local/oab/deb for the newly built Java Packages.
* Create a GnuPG signing key in /var/local/oab/gpg if none exists.
* Sign the local apt repository using the local GnuPG signing key.

What gets installed?
--------------------
Nothing!

This script will no longer try and directly install or upgrade any Java
packages, instead a local apt repository is created that hosts locally
built Java packages applicable to your system. It is up to you to install
or upgrade the Java packages you require using apt-get, aptitude or
synaptic, etc. For example, once this script has been run you can simply
install the JRE by executing the following from a shell.
::

  sudo apt-get install sun-java6-jre

Or if you ran the script with the -7 option.
::

  sudo apt-get install oracle-java7-jre

If you already have the *"official"* Ubuntu packages installed then you
can upgrade by executing the following from a shell.
::

  sudo apt-get upgrade

The local apt repository is just that, **local**. It is not accessible
remotely and oab-java.sh will never enable that capability to ensure
compliance with Oracle's asinine license requirements.

By default, the script creates a temporary GPG keyring in the working
directory. In order to use the current user's GPG chain instead, specify
the key ID of an existing secret key. Run gpg -K to list available keys.

Known Issues
------------

* The Oracle download servers can be horribly slow. My script caches the downloads
  so you only need download each file once.

What is 'oab'?
--------------
Because, O.A.B! ;-)


History
=======

0.2.6
-----

* Fixed screen scraping of the Oracle website.

  * Closes: https://github.com/flexiondotorg/oab-java6/issues/55
  * Closes: https://github.com/flexiondotorg/oab-java6/issues/56
  * Closes: https://github.com/flexiondotorg/oab-java6/issues/57
  * Closes: https://github.com/flexiondotorg/oab-java6/issues/58

0.2.5
-----

* Fixed building Oracle Java 7 by adding ``libxrender1`` to the dependencies.

0.2.4
-----
* Added support for JCE Unlimited Strength Jurisdiction Policy Files. Thanks to Ladios Jonquil and Jameson J Lee.

  * Closes: https://github.com/flexiondotorg/oab-java6/issues/31

* Reverted to https for git clone of upstream tools. 

  * Closes: https://github.com/flexiondotorg/oab-java6/issues/32
  
* Updated download links to Sun Java 6 and Oracle Java 7. Thanks to Ladios Jonquil and Jameson J Lee.

  * Closes: https://github.com/flexiondotorg/oab-java6/issues/33
  * Closes: https://github.com/flexiondotorg/oab-java6/issues/34
  * Closes: https://github.com/flexiondotorg/oab-java6/issues/39

0.2.3
-----
* Added an option to build ``oracle-java7`` packages.
* Integrated common function into oab-java6.sh

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/23

0.2.2
-----
* Added an option to use a pre-existing signing key. Thanks to Hannes Schmidt.
* The ``git clone`` of ``rraptorr/sun-java6`` now uses http rather than https.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/27

0.2.1
-----
* Fixed downloading from ``edelivery.oracle.com`` (again). Thanks to onlymostlydead (Mark).

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/28

0.2.0
-----

* Fixed downloading from ``edelivery.oracle.com`` when ``ca-certificates`` is not installed.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/22

* Fixed the skip rebuilding behaviour so it works as described.
* Fixed the format of ``apt`` source file.
* Documentation is now self referencing.

0.1.9
-----
    
* Fixed download of the Oracle binary packages, which now requires cookies. Thanks to Martin Polden and Miah Johnson.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/18

* Added an option (-s) to skip rebuilding if packages already exist, tanks to Derek Chen-Becker.
* Added a comment to the ``apt`` source file, thanks to Eshwar Andhavarapu.
* Added documentation for user running the script behind a proxy server, thanks to Olzhas.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/12

* Should now build on Ubuntu 12.04 LTS, but untested.
* Updated documentation which is now correctly formatted as reStructuredText.

0.1.8
-----
 
* Added dynamic determination of Java package URLs and sizes.
* Added an option (``-c``) to optionally clean .deb package.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/10

0.1.7
-----

* Fixed GPG key creation on VMware ESX Server.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/11

* Fixed clone of the ``sun-java6`` repository for users behind restrictive firewalls, thanks to Thorsten Möllers.

0.1.6
-----
 
* Fixed downloading of ``common.sh`` when ca-certificates is not installed.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/3

* Updated to support Java6u31

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/7
  * Closes : https://github.com/flexiondotorg/oab-java6/issues/8
  * NOTE! Requires that the upstream script tags Java6u31 as stable, see the following ticket https://github.com/rraptorr/sun-java6/issues/3

* Prevent script from running under Ubuntu Precise as it is currently known to be unsupported.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/4

* Prevent automated key generation when running in an OpenVZ container because I'm too stupid to work out a proper solution

0.1.5
-----

* Fixed missing code that actually does the build.

0.1.4
-----

* Added GnuPG signing of the local ``apt`` repository.
* Updated package building to preserve the upstream package urgency.
* Re-factored to remove hard coded versions, now uses ``debian/changelog``.
* Fixed the ``override`` file generation to ensure it doesn't contain duplicates.
* Updated documentation.

0.1.3
-----

* Added checking out of tagged releases of the upstream scripts.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/1

* Added loose distribution checking so it should now work with Linux Mint and other Ubuntu derivatives.

  * Closes : https://github.com/flexiondotorg/oab-java6/issues/2

* Added the creation of a local ``apt`` repository
* Removed installation of Java packages, you can now use ``apt-get`` yourself
* Updated documentation

0.1.2
-----

* Fixed build requirements.
* Fixed install of ``ia32-sun-java6-bin`` on 64-bit systems.
* Fixed install of Java browser plug-in on systems without a supported browser.
* Added runtime requirements.
* Added TODO.
* Updated documentation.

0.1.1
-----

* Updated to use dynamic version detection throughout.
* Fixed package installation when upgrading.
* Minor documentation updates.

0.1.0
-----

* Initial release.

Credits
=======

This package is written and maintained by Martin Wimpress, <code@flexion.org>

Other contributors, listed alphabetically, are:

* Derek Chen-Becker - Added an option to skip rebuilding packages.
* Eshwar Andhavarapu - Added comments in the apt source file.
* Hannes Schmidt - Added an option to use a pre-existing signing key.
* Jameson J Lee - Added support for JCE Unlimited Strength Jurisdiction Policy Files and fixed downloading from Oracle (yet again).
* Ladios Jonquil - Added support for JCE Unlimited Strength Jurisdiction Policy Files and fixed downloading from Oracle (yet again).
* Martin Polden - Fixed download binary packages from Oracle.
* Miah Johnson - Fixed download binary packages from Oracle.
* onlymostlydead - Fixed downloading from Oracle (again).
* Thorsten Möllers - Fixed git cloning for users behind restrictive firewalls.

Many thanks for all contributions!

Todo
====

* Check the binary packages downloaded from Oracle are the correct size.  
* Add support to build for a given Ubuntu distribution.
* Add support to build using ``pbuilder`` or use ``fakeroot``.


License
=======

Copyright (c) 2012 Martin Wimpress, http://flexion.org/

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
