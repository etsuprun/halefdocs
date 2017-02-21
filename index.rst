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
- Create your OpenVXML first project—a "Hello world" voice application
- Deploy an application
- Use halefBot to test out the application
- Use `autoggs.py` to add logging and semantic categorization capabilities

Installing Eclipse and OpenVXML
--------------------------------

Eclipse is an IDE (interactive development environment) that will allow you, among other things, to design call workflows compatible with Halef.

Install Java Development Kit 8
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`JDK 8`_ is a prerequisite for Eclipse and OpenVXML. Here is the direct link to the `Windows 64-bit version`_.

Install Eclipse
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Download and install Eclipse_. Do not use the latest version—only this one.

Install the OpenVXML plug-in for Eclipse
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Start Eclipse.

From the menu, select *Help* → *Install New Software...*.

In the Install dialog that appears, in the "Work with:" field at the top, add the VTP's update site: http://build.openmethods.com/downloads/OpenVXML5.1/repository/ and hit Enter.

Check the *OpenVXML 5.0* as well as *Uncategorized* packages and click Next. Note that this part takes a little while.

On the *Review Licenses* screen, there aren't actually any licenses to review.  Just click *I accept the terms...* and then *Finish*.

During the installation you'll need to click OK once, to allow unsigned content to be installed.

When the installation is complete, restart Eclipse.

Now that you have installed OpenVXML, change your perspective (the windows and tabs you see in Eclipse). From the menu, select *Window* → *Open Perspective* → *Other...* → *OpenVXML*.

Creating a *Hello World* Project
--------------------------------

Set up the voice and workflow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Every OpenVXML project consists of two components:

- The *Voice*, which stores all your audio files
- The *Workflow*, a visual representation of the voice application you want to design. 

First, let's create a new voice project: *File* → *New* → *Project* → *Voice Tool Wizards* → *Voice*

Call the voice project *HelloWorld_Voice*.

You should now see the voice project in your Project Explorer pane:

.. image:: /images/create_voice.png

Now, create the interactive workflow: *File* → *New* → *Project* → *Voice Tool Wizards* → Interactive Workflow

The application name for this and all Halef workflows should be *Deploy_Workflow*. Hit *Next*.

In the Branding dialogue box, leave Brands at *Default*. Hit *Next*.

On the *Interaction Type Support* dialogue box, leave *Voice Interaction* checked. Hit *Next*.

On the *Language Support* dialogue box, associate your workflow with your voice by clicking *Not Configured* and choosing *HelloWorld_Voice*:

.. image:: /images/associate_voice.png

You have now created your voice and your workflow, and you have associated the two. You should see both *HelloWorld_Voice* and *Deploy_Workflow* in the Project Explorer pane.

Create a Hello World canvas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In Project Explorer, expand *Deploy_Workflow* and then *Workflow Design*. Open up *Main Canvas.canvas*. As the name suggests, this is the canvas on which you will be drawing a flowchart-like representation of your voice application. You should now see something like this:

.. image:: /images/canvas.png


.. _JDK 8: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
.. _`Windows 64-bit version`: http://download.oracle.com/otn-pub/java/jdk/8u20-b26/jdk-8u20-windows-x64.exe
.. _Eclipse: http://www.eclipse.org/downloads/packages/eclipse-rcp-and-rap-developers/keplersr2
