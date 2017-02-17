.. halef documentation master file, created by
   sphinx-quickstart on Fri Feb 17 10:19:05 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

   
OpenVXML Without Tears
==========================

:Authors: Vikram Ramanarayanan and Eugene Tsuprun (with inputs from the OpenVXML Setup and User Guides)

Overview
-----------

In this manual, you will learn how to:

- Install Eclipse and the OpenVXML plugin for Eclipse
- Create a callflow for a "Hello world" voice application
- Deploy an application
- Use halefBot to test out the application
- Use `autoggs.py` to add logging and semantic categorization capabilities

Installing Eclipse and OpenVXML
--------------------------------

Eclipse is an IDE (interactive development environment) that will allow you, among other things, to design call workflows compatible with Halef.

Install Java Development Kit 8.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`JDK 8`_ is a prerequisite for Eclipse and OpenVXML. Here is the direct link to the `Windows 64-bit version`_.

Install Eclipse
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Download and install Eclipse_. Do not use the latest version--only this one.

Install the OpenVXML plug-in for Eclipse
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Start Eclipse.

From the menu, select *Help* → *Install New Software...*.

In the Install dialog that appears, in the "Work with:" field at the top, add the VTP's update site: http://build.openmethods.com/downloads/OpenVXML5.0/repository/ and hit Enter.

Check the `OpenVXML 5.0` as well as the `Uncategorized packages` at the top and click Next. Note that this part takes a little while.



.. _JDK 8: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
.. _`Windows 64-bit version`: http://download.oracle.com/otn-pub/java/jdk/8u20-b26/jdk-8u20-windows-x64.exe
.. _Eclipse: http://www.eclipse.org/downloads/packages/eclipse-rcp-and-rap-developers/keplersr2
