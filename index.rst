.. halef documentation master file, created by
   sphinx-quickstart on Fri Feb 17 10:19:05 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

   
OpenVXML Without Tears [1]_
============================

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

Creating a Hello World Project
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

In the Branding dialog box, leave Brands at *Default*. Hit *Next*.

On the *Interaction Type Support* dialog box, leave *Voice Interaction* checked. Hit *Next*.

On the *Language Support* dialog box, associate your workflow with your voice by clicking `Not Configured` and choosing *HelloWorld_Voice*:

.. image:: /images/associate_voice.png

You have now created your voice and your workflow, and you have associated the two. You should see both *HelloWorld_Voice* and *Deploy_Workflow* in the Project Explorer pane.

Create a text-based Hello World canvas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this part of the tutorial, you will create and deploy a text-based chatbot that says "Hello World".

In Project Explorer, expand *Deploy_Workflow* and then *Workflow Design*. Open up *Main Canvas.canvas*. As the name suggests, this is the canvas on which you will be drawing a flowchart-like representation of your chatbot. You should now see something like this:

.. image:: /images/canvas.png

The *Project Explorer* (on the left) is where you should see the Voice and Workflow projects in your Eclipse workspace.

The *Design Area* (in the middle) shows the call flow of the currently selected canvas.

The *Voice Pallet* (on the right) displays the available components for the application.

Drag the component called *PlayPrompt* onto the canvas. *PlayPrompt* is a component that outputs something to your user–either a sound file or some text. Let's have it say "Hello world!":

1. Double-click the PlayPrompt on your canvas
2. Under *Media*, click on `Not Configured`.
3. Press *Add Entry*.
4. In Content Type, choose choose Text.
5. In the text box, type "Hello world!"
6. Hit OK three times.

.. image:: /images/hello_world.png

Now that you've created the PlayPrompt, let's make sure your application knows to play it when you start interacting with the chatbot. To do this, click on the arrow in the lower right corner of the Begin block and drag and drop it onto the PlayPrompt block. In the ensuing dialog box, select *Continue* and hit OK.

Now, let's create a Return block to mark the end of the application. Drag the *Return* component from the Voice pallet onto your canvas, and connect your PlayPrompt to Return the same way: click the little arrow on the PlayPrompt, drag it to Return, and hit OK.

Your canvas should now look like this:

.. image:: /images/hello_world_canvas.png

Save and export Your project
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We need to export the newly created project into a Web ARchive (WAR) application that can be served by the web server as VoiceXML and then read by Halef's Voice Browser. A voice browser browses voice/speech web pages (in the VoiceXML format) much like Firefox or Chrome browse HTML pages.

1. Save the project: *File* → *Save All*
2. Go to *File* → *Export ...*
3. Under Voice Tools, choose "Web Application"
4. Select "Archive file".
5. Choose where you'd like to save the file. We recommend saving it in a git repository for better version control.

Test your application on halefBot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

halefBot is the text-based interface to Halef. 

Open up halefBot URL (ask your system administrator for the URL).

The Start URL is [your warfile name without '.war']/Deploy_Workflow/Begin.

For instance, if you called your warfile "helloworld.war", the Start URL is ``helloworld/Deploy_Workflow/Begin``.

Once you specify the Start URL, halefBot should say: Hello World!

Creating a Branching Voice Application 
----------------------------------------

Let's now create a more complex callflow. In this section of the tutorial, you will build a text-based chatbot that will:

1. Ask the user if they like cheese
2. Save the user's response into the database on our server
3. Categorize the user's response into the semantic categories of "yes" or "no"
4. Follow up with an appropriate response ("Me too! I love pizza!" or "I'm sorry to hear you don't like pizza.")
5. Ask for clarification, if the original response was not understood

Our callflow will look like this:

.. image:: /images/pizza_callflow.png

Set up your workspace and project
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We find it easiest to keep each OpenVXML project in its own Eclipse workspace. So if you already have a project open in Eclipse, you may want to switch to another Eclipse workspace. Go to *File* → *Switch Workspace* → *Other...* and choose where you'd like your new workspace to be.

To restore your Project Explorer, Design Area, and Voice Pallet, go to *Window* → *Open Perspective* → *Other...*, and choose OpenVXML.

Now, follow the instructions in `Creating a Hello World Project`_ to create your voice and workflow: Cheese_Voice and Deploy_Workflow.

Configure the Question Block
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In contrast to a PlayPrompt, which plays back a message for the user and expects no response, a Question block allows us to prompt the user for a response. In this case, we'll be asking if the user likes pizza.

Drag a *Question* block onto your canvas and connect the Begin block to it. Double click it to edit:

1. Set *User Input Style* to "S" and leave the dropdown at "Voice Only". (Do this first.)
2. Set the *Name* for the question block. This name is arbitrary and will just help you identify the block on your canvas.
3. Set a *Variable Name* for the variable that will store the response for our question. Our convention is to start the variable name with `A_`, for instance, `A_do_you_like_pizza`.
4. Click on `Not Configured` next to *Voice Grammar. Press "Add Entry". In the dialog box that appears, set Content Type to Text. Type "Do you like pizza?" in the text area. That's the text that will be shown to the test taker. Hit OK twice.
5. Click on `Not Configured` next to Voice Grammar. Choose "Grammar File" and then type `ignore.wfst`. This is the name of the language model Halef will be using when converting the user's speech input into text. Because we are building a text-based chatbot for now, we don't need to customize a language model. We do, however, need to specify a value here, because Halef expects one. 

Your question block should look like this:

.. image:: /images/question_block.png

Configure the Script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Script blocks allow you to use the JavaScript language to manipulate variables, communicate with external services, and control the flow of the application.

In this application, we will use the script block to:

* Process the user's response to our question (Do you like pizza?) and classify it into one of two categories: yes or no.
* Send the user's response to a backend service which will store it for subsequent retrieval and analysis

Fortunately, you don't need to know JavaScript to do these two things. We've created a script called `autoggs.py` to help you address the two use cases above.

Drag a Script block and connect the Question block to it.

Add the following code to the script block:

::
	/*
	.*yes.*	yes
	.*yeah.*
	.*no.*	no
	*/



.. _JDK 8: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
.. _`Windows 64-bit version`: http://download.oracle.com/otn-pub/java/jdk/8u20-b26/jdk-8u20-windows-x64.exe
.. _Eclipse: http://www.eclipse.org/downloads/packages/eclipse-rcp-and-rap-developers/keplersr2

.. [1] The absence of tears is not guaranteed.