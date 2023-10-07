# **Local debugging Alexa Hosted Skills using VS code**

![](RackMultipart20231007-1-cm5loo_html_5c541b51347dd872.png)

https://youtu.be/ByE8b7d3LQM

**Prerequisites** Python, Git for Windows, and Node.js

Install the Alexa Skills Toolkit for Visual Studio Code through the extensions manager.

![](RackMultipart20231007-1-cm5loo_html_d7be64cd088e2b8f.png)

And run **ask configure** to set up your environment

**Create your skill**

Here we'll use ASK Toolkit

![](RackMultipart20231007-1-cm5loo_html_b212d2e9f71826ec.png)

Answer Questions: Name (Hosted debug), Language (English (UK), Custom skill, Runtime (Alexa Hosted – python), Region (Europe(Ireland), and local directory.

Click **Create**

This will create a skill that you can see in the dev console

![](RackMultipart20231007-1-cm5loo_html_56b8512bfd891132.png)

And you can also download to VS code

![](RackMultipart20231007-1-cm5loo_html_9264fb93dc2dbe98.png)

Choose the folder to download into and it appears in the VS code.

Change the invocation name using VS code

![](RackMultipart20231007-1-cm5loo_html_9b812818b80afef.png)

N.b. Invocation name cannot contain the words "alexa", "amazon", "echo", "skill", "app" or "ziggy".

Make changes to the invocation name, save and commit.

Alexa hosted skill use git commands to update (not ask deploy).

- git add .
- git commit -m "change invocation name"
- git push

You can try using Source control

![](RackMultipart20231007-1-cm5loo_html_42c52c61fcb20d.png)

But if control-Enter doesn't commit, see [https://github.com/microsoft/vscode/issues/108674](https://github.com/microsoft/vscode/issues/108674)

Check in developer console

![](RackMultipart20231007-1-cm5loo_html_5083dda0692d18d5.png)

Now we'll make a change in the dev console. Change the welcome message, and I've added an extra line to help with debugging

![](RackMultipart20231007-1-cm5loo_html_3cc975411c80ffd0.png)

Save and deploy.

Check that it works

1. dev console

![](RackMultipart20231007-1-cm5loo_html_ebd34fb4f6a3422.png)

1. In VS code using **ask dialog** (or use ask toolkit)

If you make a change in the developer console, use **git pull** to get the code.

It recognises the changes:

lambda/lambda\_function.py | 2 +-

1 file changed, 1 insertion(+), 1 deletion(-)

And you can see the change in your lambda function:

![](RackMultipart20231007-1-cm5loo_html_add438c955eb77f1.png)

And test using **ask dialog** :

![](RackMultipart20231007-1-cm5loo_html_b6b605fe7345575f.png)

Or via ASK Simulator:

![](RackMultipart20231007-1-cm5loo_html_17888d940c525706.png)

Now let's debug our code locally

First, solve any missing link problems, e.g. ask-sdk-core:

![](RackMultipart20231007-1-cm5loo_html_d7f6f133c11a69f.png)

Set up a virtual environment - run the following commands:

py -m venv env (create new workspace id asked)

.\env\Scripts\activate

pip install ask-sdk-core

pip install ask-sdk-local-debug

ask configure

Now set up local debugging.

**Add debugger configuration**.

1. Create a launch.json file for your skill:

In VScode, display your lambda\_function.py code, then select Run menu, and then choose Add Configuration

![](RackMultipart20231007-1-cm5loo_html_f79118310477a4de.png)

This creates a launch.json debugger for the current file.

Remove the // comments.

![](RackMultipart20231007-1-cm5loo_html_5bd9fd50f84be6b6.png)

Click Add Configuration again.

1. In drop-down box, choose **ASK: Alexa Skills Debugger (Python)**

![](RackMultipart20231007-1-cm5loo_html_d90eb3df382b639e.png)

So that your launch.json looks like this (edit 'pythonPath' to 'python')

![](RackMultipart20231007-1-cm5loo_html_c931275edf956c8b.png)

We still need to edit a few more items:

Check your region is correct – I'm in the EU

                "--region",

                "EU"

Edit 'program' to point directly to the local debugger invoker.py (not directly to the debugger)

**"program": "${workspaceFolder}/env/Lib/site-packages/ask\_sdk\_local\_debug/local\_debugger\_invoker.py",**

Save launch.json

**Debug your code**

In a terminal, type **ask run**. This can be in VS code or a separate cmd (or PS) ternimal

Response is:

\*\*\*\*\*Once the session is successfully started, you can use `ask dialog` to make simulation requests to your local skill code\*\*\*\*\*

You now have an hour's debugging session.

Start a debugging session

Click the debug symbol and make sure Debug Alexa Skill is selected:

![Shape2](RackMultipart20231007-1-cm5loo_html_b53e6a132be3a46b.gif) ![Shape1](RackMultipart20231007-1-cm5loo_html_c02d1a74a14fb30b.gif) ![](RackMultipart20231007-1-cm5loo_html_3d4aa59d269b1eb6.png)

Then click the green triangle to start a session.

You should see 'Running' in the call stack.

![](RackMultipart20231007-1-cm5loo_html_e1a820e7e3f20125.png)

You can now debug your code using the CLI, the Simulator, or a device

I've set a breakpoint at the return in the launch request.

![](RackMultipart20231007-1-cm5loo_html_21dde639a9d76f41.png)

Open another terminal, change to the to the skill's folder and type **ask dialog.**

Type your launch command ('open hosted debug demo') and the code should break

![](RackMultipart20231007-1-cm5loo_html_d303710d8606e618.png)

You can now look at variables, step or do any debug command:

**Problem: Changing variables**

If you change a variable at a breakpoint, you must do a step before the change is committed.

Here, I've truncated the speak\_output variable in the debug window (F2)

![](RackMultipart20231007-1-cm5loo_html_ebe0da6338ba8de8.png)

![](RackMultipart20231007-1-cm5loo_html_d4053ce4481ecb6c.png)

If you continue by pressing F5 or the continue icon, the speak\_output in the simulator display hasn't changed

![](RackMultipart20231007-1-cm5loo_html_241c56878dc3e807.png)

Don't forget, if you don't do this quickly enough, the skill will time out.

**Solution:**

This is why we added another line of code initially. Set the breakpoint there, so that you can step to the return statement, then run, enforcing the change to the variable.

speak\_output = speak\_output + "."

Run the code and at the breakpoint, I've changed the speak\_output variable:

![](RackMultipart20231007-1-cm5loo_html_81974aeab36df16e.png)

![](RackMultipart20231007-1-cm5loo_html_978485c30dbedd2.png)

Having made the change, perform one step to enforce the change and then run the code.

![Shape3](RackMultipart20231007-1-cm5loo_html_27c314b47dcff14c.gif) ![](RackMultipart20231007-1-cm5loo_html_c1bff50185ed397c.png)

The correct value is then displayed in the simulator (albeit with the extra dot)

![](RackMultipart20231007-1-cm5loo_html_57060c03d1b8c9c.png)

But you have to be quick!

**Make changes to your code**

As an example, I've changed this line (added a few more dots)

![](RackMultipart20231007-1-cm5loo_html_77c6693acc07fda5.png)

I still have the breakpoint.

Save the lambda\_function.py code

Start another ask run if necessary and restart a debug session. (Click the red square and then the green triangle)

It will stop at the debug, but you can continue

speak\_output now contains the extra dots.

![](RackMultipart20231007-1-cm5loo_html_e62784fa754d7f0f.png)c

You can continue changing code and locally debugging your code and finally save the changes to github as before

- git add .
- git commit -m "change invocation name"
- git push

Check the changes have been made in dev console skill.

Remember if you make change in the dev console, then you need to use **git pull** in VS code.

**Summary**

We have seen how to create an Alexa Hosted skill and move the files between the developer console and your VS environment.

We saw how to resolve any missing files problems and set up a virtual environment before going on to create a launch.json file, editing that so it points to the correct files for debugging.

We then saw how to debug our code using breakpoints and how to change variables, making those permanent by adding a step. Finally we make changes to our code, debugged the changes and uploaded those to the developer console

I hope this all helps

**My book**

![](RackMultipart20231007-1-cm5loo_html_6ad38fe1788a7f9b.png)

**https://www.elektor.com/programming-voice-controlled-iot-applications-with-alexa-and-raspberry-pi**

**References:**

Managing Alexa-Hosted Skills in GitHub - Dabble Lab #245

[https://www.youtube.com/watch?v=88AsF\_xJsj0](https://www.youtube.com/watch?v=88AsF_xJsj0)

https://www.andreasjakl.com/local-debugging-of-alexa-skills-with-visual-studio-code/