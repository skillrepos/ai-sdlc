# Incorporating AI into your SDLC
## Leveraging AI tooling across the phases of your software development lifecycle
## Session labs 
## Revision 1.9 - 06/19/25

**Versions of dialogs, buttons, etc. shown in screenshots may differ from current version of Copilot**

**Follow the startup instructions in the README.md file IF NOT ALREADY DONE!**

**NOTES:**
1. We will be working in the public GitHub.com, not a private instance.
2. Chrome may work better than Firefox for some tasks.
3. Substitute the appropriate key combinations for your operating system where needed.
4. We will be using the free version of GitHub Copilot for some steps.
5. The default environment will be a GitHub Codespace (with Copilot already installed). If you prefer to use your own IDE, you are responsible for installing Copilot in it. Some things in the lab may be different if you use your own environment.
6. To copy and paste in the codespace, you may need to use keyboard commands - CTRL-C and CTRL-V.**
7. VPNs may interfere with the ability to run the codespace. It is recommended to not use a VPN if you run into problems.
8. If you use the new free Copilot plan (no signup), some advanced functionality may not be available.
</br></br></br>

**Lab 1 - Using AI in Planning**

**Purpose: In this lab, we’ll see some ideas of how we can use AI in the Planning phase**

1. For our labs in this workshop, we have a set of code that implements a simple to-do app written in Python with a toolkit called *Flask*.
   The files for this app are in a subdirectory named *app*.  Change into that directory in the terminal and take a look at the app's files.
   To view the files, you can either click on them in the file list on the left or you can open them with the *code* commands below. But make sure to switch into the *app* directory.) 

```
cd app
code app.py
code auth.py
code datastore.py
```

![Viewing app files](./images/sdlc2.png?raw=true "Viewing app files")

2. Let's see how we can create a standalone index for the code that the AI can leverage to get more details. In the *extra* directory in the project are a set of python tools for this. Run the command below to create a standalone index using ChromaDB for our code. (If you want to understand more about how these work, you can look at the actual code in the Python files in *extra*).
   
```
python ../extra/index_code.py
```

3. This created an index that is persisted in a *ChromaDB* database. Now we can run a simple search tool that will take whatever prompt/query we enter and return the primary match that it finds in the index of our codebase. Run the first command below. Then you can enter prompts like the next two lines. Type "exit" to quit.

```
python ../extra/search.py
Where does the code use authentication?
Is there already a module that implements our data store?
```

![Searching vector DB](./images/sdlc3.png?raw=true "Searching vector DB")

4. What you are seeing here is just the hits from searching the vector database that we created. To make this more useful, we would get these hits to an LLM by adding to the prompt to give it more specific context. We can see the end results of that by letting Copilot index our code in the codespace environment.

5. Click on the Copilot icon at the bottom. If you see a blue button to Setup Copilot, go ahead and click on that. Then check the two checkboxes for "Code Completions (all files)" and "Code Completions (Python)".  After a few moments, if you click the icon again, you should see a line near the middle of that dialog that either says "Locally indexed" or "Remotely indexed". 

![Copilot indexed](./images/sdlc62.png?raw=true "Copilot indexed")

6. With the index in place, let's see how Copilot responds to a generic request. Go to the Copilot Chat interface (on the right) and type in the prompt below. (Note we are using the chat variable **#codebase** to tell Copilot to look at the complete set of code in our app.) 

```
Where in this #codebase do we enforce authentication?
```
![Prompting Copilot](./images/sdlc63.png?raw=true "Prompting Copilot")

7. Note that the answers that come back have the information, but are also more conversational in their response. (The answer may vary in format and text depending on several factors.)

![Copilot response to authentication prompt](./images/sdlc64.png?raw=true "Copilot response to authentication prompt")

8. We can also try our other example. Enter the prompt below. After running, you should see something like the screenshot below.
```
Is there a module in our #codebase that handles data storage?
```

![Copilot response to datastore prompt](./images/sdlc66.png?raw=true "Copilot response to datastore prompt")

9. Let's try one more query here. To demonstrate further how AI can help with planning, prompt Copilot with the prompt below (JWT = JSON Web Token):

```
What would it take to change #codebase to use JWT for authentication?
```
![Prompting Copilot](./images/sdlc67.png?raw=true "Prompting Copilot")

10. After this runs, you should see an answer in the chat screen similar to what's shown in the screenshot below. Notice that it includes not only text explanations, but also the changed code. If you scroll down, you'll likely see a summary of the changes needed.

![Copilot response to JWT prompt](./images/sdlc68.png?raw=true "Copilot response to JWT prompt")

<p align="center">
**[END OF LAB]**
</p>
</br></br></br>

**Lab 2: Using AI during the development phase**

**Purpose: In this lab, we'll see how to use an AI assistant to help implement a feature request to our codebase.**

1. Let's try out the app. Start the app running in the terminal via the command below:

```
python app.py
```

2. Next, let's open a second terminal to use for sending commands to the app. Right-click in the terminal and select *Split Terminal* to get a second terminal next to the current one.

![Split terminal](./images/sdlc9.png?raw=true "Split terminal")

3. Our code is missing a *search* feature currently. Try the following command in the new terminal.

```
# Search items:
curl -i \
  -H "Authorization: Bearer secret-token" \
  http://127.0.0.1:5000/items/search?q=milk
```

4. Notice that we get a 404 response and a message indicating that the URL was not found on the server.

5. In our repository, we already have a GitHub Issue for this feature. Take a look at it by clicking on this link: [GitHub Issue #1](https://github.com/skillrepos/ai-sdlc/issues/1)

6. In order to use this information as context for the AI, we'll add the text of the issue to the AI's prompt context. First, we need to get the text from the issue.
We have a script for this in our project. Run the command below to do this. (The "1" is the number of the GitHub Issue.)

```
../scripts/get-issue-info.sh 1
```

7. The output of running this file should be a new file in your project named FIX_ISSUE_1.md. You can click on it and open it up to view the contents.

![Displaying file](./images/sdlc11.png?raw=true "Displaying file")

8. In Copilot's Chat interface, change the mode to "Agent" by clicking on the drop-down labeled "Ask" at the bottom.

![Switch to Agent mode](./images/sdlc10.png?raw=true "Switch to Agent mode")

9. We now want to add this file as context for our prompt in the Chat panel. Click on the "Add context" item in the prompt area and select it from the list that pops up. (You may have to scroll down to find it.)

![Adding context](./images/sdlc13.png?raw=true "Adding context")

10. With the FIX_ISSUE_1.md file attached as context, enter the following prompt in the chat area and then submit it (with the button that looks like an arrow head at the bottom right of the dialog).

```
Here's the full text of GitHub Issue #1. Propose a diff to our Python codebase that implements the requested feature. Do not create or add any tests.
```
![Context and prompt](./images/sdlc15.png?raw=true "Context and prompt")

11. After Copilot processes the prompt, it should show two files changed - *app.py* and *datastore.py*. Click on the "+ -"  icon on the right of the "2 files changed" area in the dialog. (See figure below).  Take a look at the diffs. When you are satisfied with the proposed changes, click on the *Keep* button in the *Files changed* dialog. Then you can close the tab that was opened to show the comparisons.

![Reviewing changes](./images/sdlc17.png?raw=true "Reviewing changes")

12. After clicking on the "Keep" button, you can close the  "Suggested Edits (2 files)" tab if you want. Now, let's try the *search* operation again. If your app was running when you made the changes in step 9, it should have automatically reloaded. If you see a message in its output of the sort "Detected change ... reloading", you should be good to go. But if you don't have that you can kill the process (CTRL+C) and then run the app again.

13. You can try the search operation with the same curl command as before. This time, it should run and return a 200 code rather than 404 since the search endpoint is implemented. If the item is found, it will return the found item. If not, it returns the empty set "[]".

```
# Search items:
curl -i \
  -H "Authorization: Bearer secret-token" \
  http://127.0.0.1:5000/items/search?q=milk
```

14. (Optional) To show that the search function actually returns an item after adding, there is a script in the "scripts" directory named use-app.sh. You can open it up and look at it. It adds a new item, lists it, then does a search and delete. You can run it with the command below and then see it's output.

```
../scripts/use-app.sh
```
 <p align="center">
**[END OF LAB]**
</p>
</br></br></br>

**Lab 3: Fixing bugs with AI**

**Purpose: In this lab, we'll see how to fix bugs with AI.**

1. Let's see what happens if we try to delete a non-existent item in our list. With the app still running, in the other terminal, run the command below in the second terminal.

```
# Delete an item:
curl -i \
  -X DELETE \
  -H "Authorization: Bearer secret-token" \
  http://127.0.0.1:5000/items/4
```

2. Notice that the attempt returns a 500 return code indicating *server error*. We'd rather have it return a 404 error indicating *Not found*.

![500 error](./images/sdlc18.png?raw=true "500 error")

3. Select/open the app.py file in the editor so it will be the current context. Then, so we have more control over changes, switch Copilot back to "Edit mode" by clicking on the drop-down at the bottom of the chat input dialog. If a dialog pops up about changing the chat, just answer "Yes".

![Switch mode](./images/sdlc21.png?raw=true "Switch mode")

4. Now, let's let Copilot have a try at fixing this. Enter and submit the following prompt.

```
Fix the delete endpoint so that deleting a missing item returns 404 JSON {error: 'Not found'} instead of a server error.
```

![Fix delete](./images/sdlc22.png?raw=true "Fix delete")


5. After Copilot processes this, you should see a changed app.py file. Let's add Copilot as a reviewer to have it take a look. Go to the diff (green part) and right-click and select the menu item "Copilot" -> "Review and comment".

![Add Copilot review](./images/sdlc23.png?raw=true "Add Copilot review")

6. You'll then need to select a range for it to review. You can just tell it to review the entire delete_item function.

![Pick review range](./images/sdlc24.png?raw=true "Pick review range")

7. Copilot should review the proposed changes and offer any suggestions. For this case, it may or may not have any suggestions. If it doesn't have any suggestions ,you can just select "OK". If it does have suggestions, they will show up in a *COMMENTS* tab in the same area as the *TERMINAL* tab. You can then look at each one and decide whether to Apply/Discard using the provided buttons.

![Review output](./images/sdlc25.png?raw=true "Review output")

![Review output](./images/sdlc70.png?raw=true "Review output")

8. Once you are satisfied with the set of changes and reviews, go ahead and click one of the Keep buttons to save the changes.

![Keep](./images/sdlc26.png?raw=true "Keep")

9. (Only if needed.) If Copilot got it wrong and you now have errors (reported in the *PROBLEMS* tab at the bottom), you can right click and select "Fix with Copilot" and follow-through on the process from there.

![Fix if needed](./images/sdlc71.png?raw=true "Fix if needed")   

10. Now, you can repeat step #1 (restart the app if it stopped) and hopefully you'll see a 404 error "Not found" instead of a 500 one.

![Fixed code](./images/sdlc27.png?raw=true "Fixed code")

<p align="center">
**[END OF LAB]**
</p>
</br></br></br>

**Lab 4: Testing**

**Purpose: In this lab, we'll see how to use AI to generate tests for our code.**

1. For this lab, we'll utilize Copilot's Agent functionality again. Let's start a new chat by clicking on the large "+" button in the upper right of the Chat panel. Then change the mode from "Edit" to "Agent" as you've done before. If a dialog pops up about changing the chat, just answer "Yes". 

![New chat](./images/sdlc72.png?raw=true "New chat")
   
2. We want Copilot to generate unit tests for our datastore code and integration tests for each of our endpoints. Enter the prompt below into chat and submit.

```
Write pytest unit tests for DataStore (all CRUD + search) and Flask integration tests for each endpoint (including auth failure).
```

![Prompt for tests](./images/sdlc28.png?raw=true "Prompt for tests")

3. As this runs, if you encounter points where Copilot wants to run commands in the terminal and/or offers a "Continue" button, go ahead and accept that. If it simply notes commands and stops, go ahead and copy and paste those into the terminal and run them.

![Continue to execute command](./images/sdlc30.png?raw=true "Continue to execute command")

4. Another possibility is that it will stop with a "Let me know" type of statement. If it does, you can tell it to proceed or some similar statement as shown below.

![Telling Copilot to proceed](./images/sdlc73.png?raw=true "Telling Copilot to proceed")

5. After the testing commands are run, you should hopefully see a clean run and the agent will notify you that things have completed successfully. (If not, you may have to go through several iterations of interacting with Copilot while the agent fixes things to have passing tests.)

![Clean test run](./images/sdlc31.png?raw=true "Clean test run")
![Clean results](./images/sdlc32.png?raw=true "Clean results")

6. If you have any failing tests still, you can try adding a prompt of "Tests do not run cleanly" and submit that to Copilot. Again, accept any attempts to run things in the terminal. Or, you can start a new Agent mode chat session by clicking the "+" sign in the upper left to start a new chat session and try the same prompt again.
   
7. You should now see test files for app integration tests and unit tests for the datastore pieces. Make sure to save your testing files with one of the *Keep* buttons.

![New test files to keep](./images/sdlc33.png?raw=true "New test files to keep")

8. Now, let's see how else AI can help us with testing by entering a prompt (in the Chat panel and still in "Agent" mode) asking what else to test. 

```
What else in the #codebase should we test? 
```
![What else](./images/sdlc34.png?raw=true "What else")

9. Copilot should suggest some other test cases and then ask if it should add them. You can tell it to add the most important ones with a prompt like the one below.

```
Just add the most important ones.
```

![Add most important](./images/sdlc35.png?raw=true "Add most important")

10. After this, it may also suggest a command in the terminal to run to verify the tests. If so, click on Continue. Or it might tell you which ones are most important, and you have to tell it again to add them and go through the interactive process with it again.

![Add most important](./images/sdlc36.png?raw=true "Add most important")

11. If the command fails, it should suggest a fix. If so, you can accept that and then complete the cycle. Save files with any changes.

![Add most important](./images/sdlc38.png?raw=true "Add most important")   

12. (Optional) If you have time and want, you can prompt the AI with other prompts for other testing, such as the following:

```
What edge cases in #codebase should I test?
How do I test for performance in #codebase?
How do I test for security in #codebase?
```

 <p align="center">
**[END OF LAB]**
</p>
</br></br></br>

**Lab 5: Refactoring**

**Purpose: In this lab, we'll see how to use the AI to refactor targeted sets of code, both for efficiency and improvements.**

1.  Open a new chat and change Copilot's mode back to "Edit".

![Change to Edit](./images/sdlc75.png?raw=true "Change to Edit")

2. Now let's give the AI a targeted set of context to work with.  Add the 3 files (app/app.py, app/auth.py, and app/datastore.py) as context. You can do this in a couple of ways. You can drag and drop the files from the explorer file list on the left into the dialog area or you can use the "Add Context" button and select the files. (You may need to click on "Files and Folders" in the context picker dialog.) If other files show up as context, you can click on them in the dialog and an "X" should show up to remove them. (Or you can close them if they're open in the current tab in the IDE.)

![Selecting files for context](./images/sdlc76.png?raw=true "Selecting files for context")
![Add context](./images/sdlc45.png?raw=true "Add context")
   
3. Let's ask Copilot to refactor our selected files to be more efficient. Enter the prompt below.

```
Refactor the files to make them more efficient.
```

4. After this runs, you will likely see output like the screenshot below. Copilot will analyze the targeted files and identify areas it thinks efficiencies can be made. It will then offer the usual diffs and Keep/Undo options.

![Refactor suggestions](./images/sdlc77.png?raw=true "Refactor suggestions")

5. You can go ahead and review the changes if you want and then Keep/Undo as needed. If you wanted, you could also add Copilot as a reviewer. (If you do add Copilot as a reviewer, don't forget to select the range in the pop-up dialog at the top.) Next steps assume you're done with those changes and have accepted (keep) or discarded (undo) them.

6. Now, let's look at how to use Copilot to add another feature. Open a new chat via the "+" control at the top. This time, we'll just use  our *datastore.py* and *app.py* files as context (if not already added). You can use the same approach as in step 2 to get those files as context.

7. Let's add logging to our functions. In the prompt area, add the prompt "Add logging for all endpoints". When ready, click Submit.

![Logging prompt](./images/sdlc46.png?raw=true "Logging prompt")

8. After this runs, you should see changes in *app.py*. Navigate through them and Apply/Keep changes as warranted.

![Logging changes made](./images/sdlc47.png?raw=true "Logging changes made")

9. (Optional) To show that the logging works, you can use the script we used previously in the "scripts" directory named use-app.sh. Running it now should cause INFO messages to be output to stderr. (Don't forget to make sure the app is running first in a separate terminal via *python app.py* If you hit errors running the app, it's possible that some edits could have affected the app code. You can compare against the original app code at https://github.com/skillrepos/ai-sdlc/blob/main/app/app.py.)

```
../scripts/use-app.sh
```

![Logging events](./images/sdlc48.png?raw=true "Logging events")

 <p align="center">
**[END OF LAB]**
</p>
</br></br></br>

**Lab 6: Documentation**

**Purpose: In this lab, we'll see how to use AI to quickly and easily create different kinds of documentation for our project.**

1. Let's start out by telling our AI to generate basic doc for our app.py file. Open the app.py file in the editor if it isn't already. Activate the inline chat dialog via Ctrl+I or Option+I and enter the following shortcut command and hitting *Enter* or submitting it:

```
/doc
```

![doc command](./images/sdlc49.png?raw=true "doc command")


2. After this, you'll probably see a large "chunk" of comments at the start of code. You can go ahead and "Accept" that via the button in the dialog.

![doc results](./images/sdlc50.png?raw=true "doc results")

3. To get comments in the body of the code, we need to further prompt the AI. Let's tell Copilot to verbosely comment the code. Bring up the inline chat dialog and enter the prompt below in Copilot. You can also choose to change the model that's being used. In the dialog, it should list models that only "cost" the same from your quota to use. Hit Enter/submit when done. You might have to click an "Enable" button afterwards to enable the model access.

```
Verbosely comment all code in this file so it is easy to follow and understand
```

![verbose and change model](./images/sdlc78.png?raw=true "verbose and change model")

4. If you had to click the Enable button, you may need to input the same prompt again. Or, if don't see any results still, you can switch to a different model and try again.
   
5. In the main chat panel, open a new chat (using the "+" control in the top right) and switch the mode back to "Ask". 

6. Now, in the main chat text area, enter the prompt below:

```
Generate Sphinx-style .rst API documentation for this Flask service
```

![generate Sphinx-style doc](./images/sdlc52.png?raw=true "Generate Sphinx-style doc")

7. Let's try another example. Let's have the AI generate simple functional documentation that we can share with others. Use the prompt below for this:

```
Generate functional documentation for the app
```

8. After the documentation is generated, you can hover over the output and insert it into a new file if you want. If you then save the file with a .md extension, you'll be able to see the document in Markdown format. (You can use the three-bar menu, in the upper left of the codespace, then select "File", then "Save As...".)

![Insert into new file](./images/sdlc83.png?raw=true "Insert into new file")

![Save functional doc](./images/sdlc54.png?raw=true "Save functional doc")

 <p align="center">
**[END OF LAB]**
</p>
</br></br></br>

**Lab 7: Onboarding/Explaining code**

**Purpose: To show how AI can be used to explain code and also help with onboarding those new to a codebase.**

1. Switch back to Agent mode for this lab. (If you do it in Ask mode, it will try to answer for all the different types of files in the project, rather than just the "app" code.

2. Let's start out having Copilot explain the code to us. Enter the prompt below in one of the chat interfaces.
```
Explain in simple terms how this code works
```

![Explain code](./images/sdlc55.png?raw=true "Explain code")


3. Someone just starting out with this code would need to also know how to run it, so let's have the AI explain how to do that as well.
```
Provide examples of how to run this code
```

![How to run](./images/sdlc56.png?raw=true "How to run")

4. Let's also use the AI to try and anticipate any potential issues new users may run into. Here's a prompt for that.
```
What are the most likely problems someone new to this codebase would run into. Explain the issue clearly and succinctly.
```

![Most likely problems](./images/sdlc57.png?raw=true "Most likely problems")

5. Let's take this a step further and have the AI create a general guide for new users to the code. You can use the prompt below. When done, you can hover over the code block and insert into a new file and then save as a .md file to see the formatting.
   
```
Create an onboarding guide for anyone brand new to this code who will be working with it or maintaining it.
```

![Onboarding guide](./images/sdlc58.png?raw=true "Onboarding guide")

6. Finally, let's have the AI generate some basic Q&A to check understanding and learning for someone looking at the code. Try this prompt:
```
Construct 10 questions to check understanding of how the code works. Then prompt the user on each question and check the answer. If the answer is correct, provide positive feedback. If the answer is not correct, explain why and provide the correct answer.
```

![Checking for understanding](./images/sdlc59.png?raw=true "Checking for understanding")

 <p align="center">
**[END OF LAB]**
</p>
</br></br></br>

**THE END**
