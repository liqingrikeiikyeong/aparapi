#DevelopersGuideLinux

*Developer guide for Linux. Updated Aug 23, 2012 by frost.g...@gmail.com*

#Aparapi Developer Guide: Linux® 32- and 64-bit platforms

##SVN Client

To contribute to Aparapi you will need an SVN client to access the latest source code. This page lists a number of SVN client providers [http://subversion.apache.org/packages.html](http://subversion.apache.org/packages.html) Also you might want to consider one of the SVN-based plugins for Eclipse®. http://wiki.eclipse.org/SVN_Howto
OpenJDK or Oracle® Java JDK install (JDK1.6 or later)

http://OpenJDK.java.net http://www.oracle.com/technetwork/java/javase/downloads/index.html

Many Linux® distributions come with Java JDK pre-installed or available as an optional install component. Sometimes the version that comes pre-installed is GCJ (http://gcc.gnu.org/java/). For Aparapi you will need to ensure that you have a copy of the JDK from either the OpenJDK project or from Oracle®.

The Oracle® J2SE JDK site contains downloads and documentation showing how to install for various Linux distributions.

http://www.oracle.com/technetwork/java/javase/index-137561.html

Here is an example for my Ubuntu system:

    $ sudo apt-get install sun-java6-jdk sun-java6-jre

When the installation is complete, ensure that your JAVA_HOME environment variable is pointing to the install location (such as /usr/lib/jvm/java-6-sun-1.6.0.26).

    $ export JAVA_HOME=/usr/lib/jvm/java-6-sun-1.6.0.26

You should also add ${JAVA_HOME}/bin to your path.

    $ export PATH=$PATH}:${JAVA_HOME}/bin

Double-check your path and ensure that there is not another JDK/JRE in your path.

    $ java -version
    java version "1.6.0_26"
    Java(TM) SE Runtime Environment (build 1.6.0_26-b03)
    Java HotSpot(TM) Client VM (build 20.1-b02, mixed mode, sharing)

##Apache Ant

Apache Ant® can be downloaded from the apache project page http://ant.apache.org

Aparapi has been tested using 1.7.1 version of Ant. It may work with earlier versions, but if you encounter issues we recommend updating to at least 1.7.1 before reporting issues. Here is an example for installing Ant on Ubuntu :

    $ apt-get install ant

Ensure that ANT_HOME is set to the install dir.

    $ export ANT_HOME=/usr/local/ant

Add `${ANT_HOME}/bin` to your path.

    $ export PATH=$PATH}:${ANT_HOME}/bin

Double-check the installation and environment vars.

    ant -version
    Apache Ant version 1.7.1 compiled ...

##AMD APP SDK

To compile Aparapi JNI code you need access to OpenCL headers and libraries. The instructions below assume that there is an available AMD APP SDK v2.5® (or later) installed and that your platform supports the required device drivers for your GPU card. Install the Catalyst driver first, and then install AMD APP SDK v2.5® or later.

See http://developer.amd.com/sdks/AMDAPPSDK/pages/DriverCompatibility.aspx for help locating the appropriate driver for your AMD card. Make sure you install the catalyst driver that includes the OpenCL™ runtime components.

    The OpenCL™ runtime is required for executing Aparapi or OpenCL™ on your GPU or GPU, but it is not necessary for building/compiling Aparapi.
    The AMD APP SDK v2.5 is necessary for compiling the Aparapi JNI code against OpenCL™ APIs.

Once you have a suitable driver, download a copy of AMD APP SDK v2.5 or later from http://developer.amd.com/sdks/AMDAPPSDK/downloads/Pages/default.aspx.

Download the installation guide for Microsoft® Windows® (and Linux®) from http://developer.amd.com/sdks/AMDAPPSDK/assets/AMD_APP_SDK_Installation_Notes.pdf. Note that if you updating from a previous version of AMD APP SDK (or its predecessor ATI STREAM SDK), first uninstall the previous version.

Download the release notes from: http://developer.amd.com/sdks/AMDAPPSDK/assets/AMD_APP_SDK_Release_Notes_Developer.pdf
GCC compiler (G++) for your Linux 32-bit or 64-bit platform

Aparapi has been tested with 32-bit and 64-bit Linux 4.1.2 or later GCC compilers.

Ensure you have the g++ toolchain installed:

    $ g++
    no input files

##JUnit

The initial Open Source drop includes a suite of JUnit tests for validating bytecode to OpenCL™ code generation. These tests require JUnit 4.

Download JUnit from http://www.junit.org/ and note the location of your JUnit installation; the location is needed to configure the test\codegen\build.xml file. Please see the UnitTestGuide page.

##Eclipse

Eclipse is not required to build Aparapi; however, the developers of Aparapi do use Eclipse and have made the Eclipse artifacts (.classpath and .project files) available so that projects can be imported into Eclipse. The com.amd.aparapi.jni subproject (containing C++ JNI source) should be imported as a resource project. We do not recommend importing com.amd.aparapi.jni as a CDT project, and we do not recommend trying to configure a CDT build, the existing build.xml files has been customized for multiplatform C++ compilations.

##Building

Check out the Aparapi SVN trunk:

    $ svn checkout http://aparapi.googlecode.com/svn/trunk aparapi

Checkout provides the following:

    aparapi/
       com.amd.aparapi/
          src/java/com.amd.aparapi/*.java
          build.xml
       com.amd.aparapi.jni/
          src/cpp/*.cpp
          src/cpp/*.h
          build.xml
       test/
          codegen/
             src/java/
                com.amd.aparapi/
                com.amd.aparapi.test/
             build.xml
          runtime/
             src/java/
                com.amd.aparapi/
                com.amd.aparapi.test/
             build.xml
       samples/
          mandel
             src/java/com.amd.aparapi.samples.mandel/*.java
             build.xml
             mandel.sh
             mandel.bat
          squares/
             src/java/com.amd.aparapi.samples.squares/*.java
             build.xml
             squares.sh
             squares.bat
          convolution/
             src/java/com.amd.aparapi.samples.convolution/*.java
             build.xml
             conv.sh
             conv.bat
       examples/
          nbody/
             src/java/com.amd.aparapi.nbody/
             build.xml
             nbody.sh
             nbody.bat
       build.xml
       README.txt
       LICENSE.txt
       CREDITS.txt

##Sub Directories

The com.amd.aparapi and com.amd.aparapi.jni subdirectories contain the source for building and using Aparapi.

The ant build.xml file, in each folder accept common 'clean' and 'build' targets. You can use the build.xml file at the root of the tree for two purposes:

    To initiate a build com.amd.aparapi of com.amd.aparapi.jni.
    To create a binary ‘distribution’ directory and zip file. This zip file is same as those available from the download section of the code.google.com/p/aparapi site.

##Preparing for your first build

Edit com.amd.aparapi.jni\build.properties and ensure that the properties are valid for your platform.

View the comments in the properties file for assistance. The build.xml ant file contains some simple checks to help diagnose simple configuration errors in case something gets messed up.

For Linux you should not need to edit build.xml unless your APP SDK install location differs from the default. The default for Linux® is /opt/AMDAPP

    amd.app.sdk.dir=/opt/AMDAPP

Perform a build from the root directory using the following command:

    $ ant clean build dist

Once your build has completed you should see an additional subdirectory named dist_linux_x86 or dist_linux_x86_64 (depending on the bitness of your platform).

The distribution directory contains:

    aparapi.jar containing Aparapi classes for all platforms.
    the shared library for your platform (aparapi_x86.so or aparapi_x86_64.so).
    an /api subdirectory containing the 'public' javadoc for Aparapi.
    a samples directory containing the source and binaries for the mandel and squares sample projects.

The root directory also contains either dist_linux_x86_64.zip or dist_linux_x86.zip containing a compressed archive of the distribution tree.

[Attribution](Attribution.md)
