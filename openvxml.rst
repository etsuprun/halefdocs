..  _openvxml:

.. halef documentation master file, created by
   sphinx-quickstart on Fri Feb 17 10:19:06 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


OpenVXML Without Tears [1]_
============================

:Authors: Vikram Ramanarayanan and Eugene Tsuprun (with inputs from the OpenVXML Setup and User Guides)

Overview
-----------

In this manual, you will learn how to:

- Install Eclipse and the OpenVXML plugin for Eclipse
- Create your OpenVXML first project—a "Hello world" chatbot application
- Deploy the application
- Use halefBot (a text interface to Halef) to test out the application
- Use WebGGS to add logging and semantic categorization capabilities

Prerequisites
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This tutorial assumes some familiarity with regular expressions, which we use to search for patterns in the users' responses (see  `RegexOne tutorial`_.)

Installing Eclipse and OpenVXML
--------------------------------

Eclipse is an IDE (interactive development environment) that will allow you, among other things, to design call workflows compatible with Halef.

Install Java Development Kit 8
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`JDK 8`_ is a prerequisite for Eclipse and OpenVXML. Download and install it.

Install Eclipse
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Download and install Eclipse_. Do not use the latest version—only this one.

Install the OpenVXML plug-in for Eclipse
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Download OpenVXML 5.1. Here is the compiled `OpenVXML for Windows`_.

Unzip the file.

Start Eclipse.

From the menu, select *Help* → *Install New Software...*.

In the Install dialog that appears, in the "Work with:" field at the top, hit `Add...`.

Hit `Local...` and select the unzipped directory: ``OpenVXML_5.1.0201506111143``.

Make sure all the check boxes in the bottom part of the screen are unchecked (`Show only the latest versions of available software`, `Group items by category`, etc.)

Select All packages.

.. image:: /images/install_openvxml.png

On the *Review Licenses* screen, there aren't actually any licenses to review.  Just click *I accept the terms...* and then *Finish*.

During the installation you'll need to click OK once, to allow unsigned content to be installed.

When the installation is complete, restart Eclipse.

Now that you have installed OpenVXML, change your perspective (the windows and tabs you see in Eclipse). From the menu, select *Window* → *Open Perspective* → *Other...* → *OpenVXML*.

Creating a Hello World Project
--------------------------------

In this part of the tutorial, you will create and deploy a text-based chatbot that says "Hello World".

Import the starter project
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Starter war file helps you get started designing your chatbot quickly.

1. Download the Starter war file: https://github.com/etsuprun/halefdocs/raw/master/Starter.war
2. Open Eclipse. Choose a new workspace.
3. Open the OpenVXML perspective: `Window` → `Open Perspective` → `Other...` and choose "OpenVXML".
4. `File` → `Import...` Choose "General" -> "Existing Projects into Workspace" and click on "Next". Then, select the button next to "Select archive file:", hit `Browse...` and navigate to the location of the .war file you just downloaded. In the drop-down menu with file extensions (which is located in the lower right corner of the dialog box on Windows), choose `*.*`. Then choose the .war file that you would like to import and select "Open".
5. Hit "Finish" in the window that appears.
6. You may or may not get an error message. If you do, just restart Eclipse.


Create a text-based Hello World canvas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In Project Explorer, expand *Deploy_Workflow* and then *Workflow Design*. Open up *Main Canvas.canvas*. As the name suggests, this is the canvas on which you will be drawing a flowchart-like representation of your chatbot. You should now see something like this:

.. image:: /images/canvas.png

The *Project Explorer* (on the left) is where you should see the Voice and Workflow projects in your Eclipse workspace.

The *Design Area* (in the middle) shows the call flow of the currently selected canvas.

The *Voice Pallet* (on the right) displays the available blocks for the application.

Drag the block called *PlayPrompt* onto the canvas. *PlayPrompt* outputs something to your user–either a sound file or some text. Let's have it say "Hello world!":

1. Double-click the PlayPrompt on your canvas
2. Under *Media*, click on `Not Configured`.
3. Press *Add Entry*.
4. In Content Type, choose *Text*.
5. In the text box, type "Hello world!"
6. Hit OK three times.

.. image:: /images/hello_world.png

Now that you've created the PlayPrompt, let's make sure your application knows to play it when you start interacting with the chatbot. To do this, click on the arrow in the lower right corner of the Begin block and drag and drop it onto the PlayPrompt block. In the ensuing dialog box, select *Continue* and hit OK.

Now, let's connect the PlayPrompt to the Submit block to mark the end of the application. Connect your PlayPrompt to Return the same way: click the little arrow on the PlayPrompt, drag it to Submit, and hit OK.

Your canvas should now look like this:

.. image:: /images/hello_world_canvas.png

Creating a Branching Application
----------------------------------------

Let's now create a more complex callflow. In this section of the tutorial, you will build a text-based chatbot that will:

1. Ask the user if they like pizza
2. Save the user's response into the database on our server
3. Categorize the user's response into the semantic categories of "yes" or "no"
4. Follow up with an appropriate response ("Me too! I love pizza!" or "I'm sorry to hear you don't like pizza.")
5. Ask for clarification, if the original response was not understood

Our callflow will look like this:

.. image:: /images/pizza_callflow.png

Configure the question block
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In contrast to a PlayPrompt, which plays back a message for the user and expects no response, a Question block allows us to prompt the user for a response. In this case, we'll be asking if the user likes pizza.

Drag a *Question* block onto your canvas and connect the Begin block to it. Double click it to edit:

1. Set *User Input Style* to "S" and leave the drop-down at "Voice Only". (Do this first.)
2. Set the *Name* for the question block. This name is arbitrary and will just help you identify the block on your canvas.
3. Set a *Variable Name* for the variable that will store the response for our question. Our convention is to start the variable name with `A_`, for instance, ``A_do_you_like_pizza``.
4. Double-click on `Not Configured` next to *Prompt* (in the Media tab). Press "Add Entry". In the dialog box that appears, set Content Type to Text. Type "Do you like pizza?" in the text area. That's the text that will be shown to the test taker. Hit OK twice.
5. Double-click on `Not Configured` next to *Voice Grammar*. Choose "Grammar File" (from the dropdown menu) and then type `ignore.wfst`. This is the name of the language model Halef will be using when converting the user's speech input into text. Because we are building a text-based chatbot for now, we don't need to customize a language model. We do, however, need to specify a value here, because Halef expects one.

Your question block should look like this:

.. image:: /images/question_block.png

Create a Script block to handle semantic classification
~~~~~~~~~~~~~~~~~~~~~~~~

Script blocks allow you to use the JavaScript language to manipulate variables, communicate with external services, and control the flow of the application.

In this application, we will use the script block to classify the response to the question "Do you like pizza?" into one of two categories: yes or no. We will also send the user's response to a back-end service, which will then store it into a database.

Fortunately, you don't need to know JavaScript to achieve the above goals. We've created a tool called WebGGS that will write the script for you.

Drag a Script block and connect the Question block to it.

Now, open WebGGS: https://videotelephony.halef-research.ets.org/webggs/

In WebGGs, define the variable name: ``A_do_you_like_pizza``

Add some regular expressions representing potential responses to the question "Do you like pizza?" and corresponding semantic categories. Your WebGGS box should look like this:

.. image:: /images/webggs.png

Now, hit the right arrow to generate the script. You should the following output:

.. highlight:: javascript

	/*.*yes.*	yes

	.*yeah.*	yes

	.*no.*	no*/
	Log.info("DEVELOPER LOG: Entering parsing script");
	// If JVXML session ID exists, use that. Otherwise, we're probably in halefBot, so let's generate one.
	if (typeof Variables.sessionId =='undefined') {
		// if session ID is undefined (because we're accessing via HalefBot), let's make one.
		if (typeof Variables.InitialParameters['sessionId'] == 'undefined') {
			Variables.sessionId = 'halefbot_' + Variables.dataCollectionGroup + '_' + Variables.extension + '_' + Date.now().toString() + '_' + Math.round(Math.random() * 10000000).toString();
			} else {
		Variables.sessionId = Variables.InitialParameters['sessionId'];
		}
	}
	Variables.SC_do_you_like_pizza = "";

	//Please fill in the appropriate variable name below

	var myJsStr = '' + Variables.A_do_you_like_pizza;


	//Convert input variable to lower case
	var myJsStr_LC = myJsStr.toLowerCase();

	//Check for the presence of the "yes" semantic category
	if(myJsStr_LC.match(/.*yeah.*/gi) || myJsStr_LC.match(/.*yes.*/gi))
	{
		Variables.SC_do_you_like_pizza = "yes";
		Log.info("DEVELOPER LOG: Set flag variable SC_do_you_like_pizza to yes");
	}

	//Check for the presence of the "no" semantic category
	if(myJsStr_LC.match(/.*no.*/gi))
	{
		Variables.SC_do_you_like_pizza = "no";
		Log.info("DEVELOPER LOG: Set flag variable SC_do_you_like_pizza to no");
	}

	//Condition to check if the ASR returned an empty string (corresponding to a NULL recognition or no-match hypothesis)

		if(myJsStr_LC == "")
	{

			Variables.SC_do_you_like_pizza = "";

			Log.info("DEVELOPER LOG: Set flag to empty string");
	}

	Log.info("DEVELOPER LOG: Starting web service script");

	var connection = java.net.URL("http://"+java.net.InetAddress.getLocalHost().getHostAddress()+"/PHP-Loggers/openvxml_logger.php").openConnection();
	Log.info("starting key.toString()");
	Log.info("sessionId="+Variables.sessionId);
	Log.info("InitialParameters="+Variables.InitialParameters);
	Log.info("completing key.toString()");
	var query = new java.lang.String("sessionId="+Variables.sessionId+"&A_do_you_like_pizza="+Variables.A_do_you_like_pizza+"&SC_do_you_like_pizza'="+Variables.SC_do_you_like_pizza);


	connection.setDoOutput(true);
	connection.setRequestProperty("Accept-Charset", "UTF-8");
	connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded;charset=UTF-8");
	connection.getOutputStream().write(query.getBytes("UTF-8"));
	var response = connection.getInputStream();
	Log.info("DEVELOPER LOG: Completing web service script");


This script will do the following:

* Log ``A_do_you_like_pizza`` (the variable containing the response to the question block) to the server
* If the response contains the strings "yes" or "yeah", set the variable ``SC_do_you_like_pizza`` to equal to "yes".
* If the response contains the string "no", set the variable ``SC_do_you_like_pizza`` to equal to "no".

Create a Branch block
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Drag a Branch block from the Voice pallet and connect the Script block to it. The Branch block is what allows to route the application in accordance with the semantic category of the response to "Do you like pizza?"

We will build exit paths to deal with three types of responses:

* responses that fall into the category of "yes" (users who like pizza)
* responses that fall into the category of "no" (users who don't like pizza)
* responses which did not fall into either category (i.e., what our script failed to categorize)

Open the Branch block and hit "Add Branch".

`Exit Path Name` is an arbitrary name, but we recommend keeping it consistent with the name of the semantic category. Let's first make a branch for the "yes" category. Set Exit Path Name to `yes`.

The `Expression` is a JavaScript statement that should return ``true`` or ``false``. If the statement is ``true``, the call will be routed through this exit path. In this case, we will want to enter: ``Variables.SC_do_you_like_pizza == "yes"``

.. image:: /images/branch.png

Now, make another exit path for the "no" category. The Exit Path Name should say `no`, and the Expression should read: ``Variables.SC_do_you_like_pizza == "no"``

Your Branch properties should now look like this:

.. image:: /images/branch_block.png

We do not need to define an Expression for the third exit path (neither "yes" nor "no"). This so-called Default path will be triggered if the JavaScript expressions for all the other semantic categories were computed to ``false``.

Add the PlayPrompts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Depending on the category of the response, we will respond with a relevant PlayPrompt—"Me too! I love pizza!", "I'm sorry to hear you don't like pizza.", or "Sorry, I don't understand.". Create three new PlayPrompts that say this.

Add the connectors 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Draw an arrow from your Branch to "Me too! I love pizza!" In the ensuing dialog box, choose the "yes" exit path.

Now, draw an arrow from the Branch to "I'm sorry to hear you don't like pizza." Choose the "no" exit path.

Draw an arrow from the Branch to "I don't understand" and choose the Default exit path. If the system doesn't understand the response, let's ask the user to repeat it. To do so, connect the "I don't understand" PlayPrompt back to the "Do you like pizza?" question to create a loop.

Finally, connect the remaining two play prompts to a Submit block to mark the end of the application.

Save and export your project
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

The Start URL is [your war file name without '.war']/Deploy_Workflow/Begin.

For instance, if you called your war file "helloworld.war", the Start URL is ``helloworld/Deploy_Workflow/Begin``.

Open your project in halefBot and try a few different responses:

.. image:: /images/pizza_chat.png

You'll note that the response in Russian ("я предпочитаю гамбургеры") was not in our grammar, and so it correctly went to the Default exit path. The other two responses ("yeah of course I love pizza!" and "not so much") were successfully captured by our regular expressions.

Adding Counters
--------------------------------------------------------

In our pizza callflow, there is no set limit as to how many times the user will get to the "I don't understand" dialog state. As long as they keep saying something we don't have in our semantic categories, the conversation can go on indefinitely.

We can use simple counter logic to set a limit. Let's do this:

1. Initialize a counter and set it to 0.
2. Each time we hit the Default exit path (the response does not fit into the "yes" or "no" semantic categories), increment the counter by 1.
3. If the counter is greater than 2, exit the application. Else, keep asking the user if they like pizza.

We'll revise our callflow to look like this:

.. image:: /images/pizza_counter.png

First, let's initialize the counter. Open the Begin block and add a new variable called ``counter_do_you_like_pizza``. Set Type to Decimal, and Value to 0.

.. image:: /images/begin_counter.png

Note: we strongly recommend keeping your counter variable names exactly consistent with the prompt variable names (``A_do_you_like_pizza`` in this case).

In the first version of this callflow, our Default exit path was connected directly to the "I don't understand" PlayPrompt. Now, we'll want to change this logic so that before we end up at "I don't understand", we'll first make sure that the user hasn't already been there twice.

Add another Script block to your canvas. If you'd like, give it a name like "increment counter". Set the Default exit path of the branch to go to this new script block. In your script block, add this JavaScript to increment the counter::

	Variables.counter_do_you_like_pizza = Variables.counter_do_you_like_pizza + 1;

Now, after we run this code, we'll want to decide whether the counter is greater than 2. To help with this, let's use the `Decision` block from the Voice Pallet.

The Decision block helps us formulate a single JavaScript expression that evaluates to ``true`` or ``false``. Select ``counter_do_you_like_pizza`` on the left, `Greater Than (>)` as the comparison operator, then Expression of ``2`` on the right.

.. image:: /images/counter_decision.png

Just like a Branch block, a Decision block supports multiple exit paths. Connect the ``true`` exit path to another PlayPrompt that says something like: "Sorry you're having trouble. Please try again later." Connect the ``false`` exit path to our "I don't understand" PlayPrompt, which then is connected back to the original prompt.

Let's save, deploy, and try this in halefBot:

.. image:: /images/halefbot_counter.png

After the third time we hit the Default exit path, the counter was greater than 2, and so we got kicked out from the application.

Appendix A: Portals: Extending Workflows to Span Multiple Canvases
--------------------------------------------------------
Using multiple canvases is a great way to separate an application into more manageable pieces.

To add a new design canvas to a Workflow (and configure a portal between the new and existing canvas):

1. Right-click on the `Workflow Design` folder for ``Deploy_Workflow`` in the Project Explorer and select:
 `New` → `Other` → `Voice Tools Wizard` -> `Design Document`. Hit "Next". This will open the new design document wizard. Give the new canvas a name and hit "Finish".

2. Enter a name for the new design canvas in the input box (for example, “SecondCanvas”. Note: This name must be unique amongst the existing design canvases in the application.

3. When you open up the new canvas, you'll see a `Portal Exit` block on the new canvas.

.. image:: /images/secondcanvas.png

4. Let's set up a Portal Entry and connect it to the second canvas. Drag and drop `Portal Entry` from the Voice Pallet onto the original canvas. Click on the new block and choose the Portal Exit to connect it to.

.. _JDK 8: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
.. _Eclipse: http://www.eclipse.org/downloads/packages/eclipse-rcp-and-rap-developers/keplersr2
.. _git tutorial: https://try.github.io/levels/1/challenges/1
.. _RegexOne tutorial: https://www.regexone.com
.. _Python 3: https://www.python.org/downloads/
.. _OpenVXML for Windows: https://sourceforge.net/p/halef/openvxml/ci/master/tree/OpenVXML-5.1.0/binary/OpenVXML_5.1.0.201506111143.zip

.. [1] The absence of tears is not guaranteed.


Appendix B: Creating an OpenVXML project from scratch
--------------------------------------------------------

The easiest way to get started with an OpenVXML project is to follow the instructions above and import `Starter.war`, a starter project. But if you really want to create your project from scratch using the Eclipse plugin, here are the instructions.

Every OpenVXML project consists of two components:

- The *Voice*, which stores all your audio files
- The *Workflows*, a visual representation of the chatbot application you want to design.

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
