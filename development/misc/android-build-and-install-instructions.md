Building
========

The libraries and tools necessary for apex are built in the
`tools/android-prepare-api.sh` and `tools/android-build.sh` scripts. These
scripts were made for Linux, while it's possible to build on Windows no scripts
or documentation is provided.

Requirements
------------

Other than the requirements found in compiling_instructions you will need the
following:

###### Qt 5.6 for android

Because Qt 5.7 for android requires C++11 support, and the ndk used does not
support C++11, Qt 5.6 is required. Ndk r9b is used, newer versions can't build
some of the libraries required.

###### Java 32bit

Qt for android needs a 32bit java jdk. The script retrieves the location from
updata-java-alternatives. If no i386 installation is found, no apk can be built
and installed, but apex will build.

###### Android openssl

We compile openssl as a static archive for android. The shared object will
conflict with the system ssl (which is not part of the public api) because of
similar symbols.

###### Build tools

Autotools and AutoGen are needed for the `tools/android-prepare-api.sh` script.

Dependencies and Apex binaries
------------------------------

To crosscompile all the dependencies the `tools/android-prepare-api.sh` script
is provided.

Once all the dependencies are compiled run the `tools/android-build.sh`
script. Run it with the `--help` parameter for all options. Be sure to
provide the `--build-apk` parameter to build an actual apk.

Installing
==========

Release version
---------------

The release version can be installed with F-Droid. Install F-Droid and add
https://exporl.med.kuleuven.be/apex-fdroid/repo as a repository. Simply install
from there.

For information on how to register the device with the **apex-study** interface
you can consult the [documentation on studies](../../studies/index.md).

Debug version
-------------

The debug version which you build yourself can be installed with the Android
debugging bridge (adb).
