# Incorporating AI into your SDLC
## Leveraging AI tooling across the phases of your software development lifecycle
## Session labs 
## Revision 1.24 - 06/04/26

**Versions of dialogs, buttons, etc. shown in screenshots may differ from current version of Copilot**

**Follow the startup instructions in the README.md file IF NOT ALREADY DONE!**

**IMPORTANT NOTES:**
1. We will be working in the public GitHub.com, not a private instance.
2. You must have a public GitHub userid. (Can go to github.com/signup if needed.)
3. Chrome may work better than Firefox for some tasks.
4. Substitute the appropriate key combinations for your operating system where needed.
5. We will be using the free version of GitHub Copilot for some steps.
6. The default environment will be a GitHub Codespace (with Copilot already installed). If you prefer to use your own IDE, you are responsible for installing Copilot in it. Some things in the lab may be different if you use your own environment.
7. To copy and paste in the codespace, you may need to use keyboard commands - CTRL-C and CTRL-V.**
8. VPNs may interfere with the ability to run the codespace. It is recommended to not use a VPN if you run into problems.

</br></br></br>
**If you are opening a file and the cursor is in the file, make sure to switch back to (click in) the terminal before running commands!**

</br></br></br>

**Lab 1 - Using AI in Planning**

**Purpose: In this lab, we’ll see some ideas of how we can use AI in the Planning phase**

<br><br>

1. For our labs in this workshop, we have a set of code that implements a simple *to-do* app, written in Python with a toolkit called *Flask*.
   The files for this app are in a subdirectory named *app*.  Change into that directory in the terminal and take a look at the app's files.
   To view the files, you can either click on them in the file list on the left or you can open them with the *code* commands below. But make sure to switch into the *app* directory.)
<br>

```
cd app
code app.py
code auth.py
code datastore.py
```
<br>

![Viewing app files](./images/sdlc85.png?raw=true "Viewing app files")

<br><br>

2. Let's see how we can create a standalone index for the code that the AI can leverage to get more details. In the *extra* directory in the project are a set of python tools for this. Run the command below to create a standalone index using ChromaDB for our code. (If you want to understand more about how these work, you can look at the actual code in the Python files in *extra*). 

  
<br><br>

```
python ../extra/index_code.py
```
</br>

![Creating vector DB](./images/sdlc86.png?raw=true "Creating vector DB")

<br><br>

3. This created an index that is persisted in a *ChromaDB* database. Now we can run a simple search tool that will take whatever prompt/query we enter and return the primary match that it finds in the index of our codebase. Run the first command below. Then you can enter prompts like the next two lines. Type "exit" to quit.

<br>

```
python ../extra/search.py
Where does the code use authentication?
Is there already a module that implements our data store?
```
<br>

![Searching vector DB](./images/sdlc3.png?raw=true "Searching vector DB")

<br><br>

4. What you are seeing here is just the hits from searching the vector database that we created. To make this more useful, we would get these hits to an LLM by adding to the prompt to give it more specific context. Copilot does a version of this by *indexing* our code in the codespace environment.

<br><br>

5. Click on the Copilot icon at the bottom. If you see a blue button to "Setup Copilot" or "Use AI Features", go ahead and click on that. 
   
![Setup Copilot](./images/sdlc87.png?raw=true "Setup Copilot")

<br><br>

Then you'll likely see a link that says `Index?`. Click on that link to have Copilot create a local index of the repository.

<br><br>

![Copilot indexing](./images/sdlc94.png?raw=true "Indexing with Copilot")

<br><br>

After a few moments, you should see an indication that the index was successfully created.

![Copilot indexing](./images/sdlc95.png?raw=true "Indexing with Copilot")

<br><br>

6. With the index in place, let's see how Copilot responds to a generic request. In the chat interface, Copilot will likely be in `Agent` mode. We want it to be in `Ask` mode for this part. Click on the second icon from left in the Chat interface, and select `Ask` mode. (See screenshot below.)

![Switching to Ask mode](./images/sdlc96.png?raw=true "Switching to Ask mode") 

<br><br>

7. Now, enter the prompt below and submit it. (Note we are using the chat variable **#codebase** to tell Copilot to look at the complete set of code in our app.) 

<br>

```
Where in this #codebase do we enforce authentication?
```

<br>

![Prompting Copilot](./images/sdlc97.png?raw=true "Prompting Copilot")


<br><br>


8. If needed for a particular LLM, click *Enable*. Note that the answers that come back have the information, but are also more conversational in their response. (The answer may vary in format and text depending on several factors.)

<br>

![Copilot response to authentication prompt](./images/sdlc98.png?raw=true "Copilot response to authentication prompt")

<br><br>

9. We can also try our other example. Enter the prompt below. After running, you should see something like the screenshot below.

<br>

```
Is there a module in our #codebase that handles data storage?
```

<br>

![Copilot response to datastore prompt](./images/sdlc99.png?raw=true "Copilot response to datastore prompt")

<br><br>

10. Let's try one more query here. To demonstrate further how AI can help with planning, prompt Copilot with the prompt below (JWT = JSON Web Token):

<br>

```
What would it take to change #codebase to use JWT for authentication?
```

<br>

![Prompting Copilot](./images/sdlc101.png?raw=true "Prompting Copilot")

<br><br>

11. After this runs, you should see an answer in the chat screen similar to what's shown in the screenshot below. Notice that it includes not only text explanations, but also the changed code. If you scroll down, you'll likely see a summary of the changes needed.

<br>

![Copilot response to JWT prompt](./images/sdlc100.png?raw=true "Copilot response to JWT prompt")

<br><br>

<p align="center">
<b>[END OF LAB]</b>
</p>
</br></br>

**Lab 2: Using AI during the coding phase**

**Purpose: In this lab, we'll see how to use an AI assistant to help implement a feature request to our codebase.**

<br><br>

1. Let's try out the app. Start the app running in the terminal via the command below:

```
python app.py
```

<br><br>

2. Next, let's open a second terminal to use for sending commands to the app. Above the terminal, click the two-column icon (*Split Terminal*) to get a second terminal next to the current one.

![Split terminal](./images/sdlc103.png?raw=true "Split terminal")

<br><br>

3. Our code is missing a *search* feature currently. Try the following command in the new terminal.

```
# Search items:
curl -i \
  -H "Authorization: Bearer secret-token" \
  http://127.0.0.1:5000/items/search?q=milk
```

<br><br>

4. Notice that we get a 404 response and a message indicating that the URL was not found on the server.

![Split terminal](./images/sdlc104.png?raw=true "Split terminal")

<br><br>

5. In our repository, we already have a GitHub Issue for this feature. Take a look at it by clicking on this link: [GitHub Issue #1](https://github.com/skillrepos/ai-sdlc/issues/1)

![GitHub issue](./images/sdlc105.png?raw=true "GitHub issue")

<br><br>

6. Open a new chat by clicking on the `+` sign in the upper right of the chat panel.

![GitHub issue](./images/sdlc128.png?raw=true "GitHub issue")

<br><br>

7. In Copilot's Chat interface, change the mode to `Plan` by clicking on the drop-down labeled `Ask` at the bottom.

![Switch to Plan mode](./images/sdlc110.png?raw=true "Switch to Plan mode")

<br><br>

8. Enter the following prompt in the chat area and then submit it.

```
Referencing https://github.com/skillrepos/ai-sdlc/issues/1, propose a diff to our Python codebase that implements the requested feature.
```
![Context and prompt](./images/sdlc109.png?raw=true "Context and prompt")

<br><br>

9. Copilot will generate a proposed diff "plan" with some options at the bottom to proceed. Review the plan and then select the `Start Implementation` option at the bottom of the plan. Notice this will automatically switch the mode to `Agent` (in the bottom of the chat window) as it starts the implementation.

![Context and prompt](./images/sdlc111.png?raw=true "Context and prompt")

<br><br>


10. After Copilot processes the plan, it should show a section of `Todos` and a section of `files changed` above the Chat text entry area. (You can click on the ">" symbols to expand the list.) If you have one of the files open in the editor, you'll also see the highlighted proposed changes. You can review individual changes if you want, but when done, click on the blue `Keep` button in the `files changed` area of the chat to accept all changes.

![Reviewing changes](./images/sdlc112.png?raw=true "Reviewing changes")

<br><br>

11. At this point, the agent may want to run some unit tests. Click on the `Allow` button to let it run the tests. (If you see test failures, you can ask the Agent to diagnose and fix them. This may be an interative process.)

![Getting errors fixed](./images/sdlc115.png?raw=true "Getting errors fixed")

<br><br>

    
12. Now, let's try the *search* operation again. If your app was running when you made the changes in step 9, it should have automatically reloaded. If you see a message in its output of the sort "Detected change ... reloading", you should be good to go. But if you don't have that you can kill the process (CTRL+C) and then run the app again.

![Running server](./images/sdlc116.png?raw=true "Running server")

<br><br>


13. Then you can try the search operation with the same curl command as before. This time, it should run and return a 200 code rather than 404 since the search endpoint is implemented. If the item is found, it will return the found item. If not, it returns the empty set "[]".

```
# Search items:
curl -i \
  -H "Authorization: Bearer secret-token" \
  http://127.0.0.1:5000/items/search?q=milk
```

![Running search](./images/sdlc117.png?raw=true "Running search")

<br><br>

14. (Optional) To show that the search function actually returns an item after adding, there is a script in the "scripts" directory named use-app.sh. You can open it up and look at it. It adds a new item, lists it, then does a search and delete. You can run it with the command below and then see it's output.

```
../scripts/use-app.sh
```

<br><br>

 <p align="center">
<b>[END OF LAB]</b>
</p>
</br></br></br>

**Lab 3: Fixing bugs with AI**

**Purpose: In this lab, we'll see how to fix bugs with AI.**

<br><br>

1. Let's see what happens if we try to delete a non-existent item in our list. With the app still running, in the other terminal, run the command below in the second terminal.

```
# Delete an item:
curl -i \
  -X DELETE \
  -H "Authorization: Bearer secret-token" \
  http://127.0.0.1:5000/items/4
```

<br><br>

2. Notice that the attempt returns a 500 return code indicating *server error*. We'd rather have it return a 404 error indicating *Not found*.

![500 error](./images/sdlc18.png?raw=true "500 error")

<br><br>

3. Open a new chat. Select/open the app.py file in the editor so it will be the current context. You should be in `Agent` mode by default. (If not, switch to it in the same way as before.)

<br><br>

4. Now, let's let Copilot have a try at fixing this. Enter and submit the following prompt.

```
Fix the delete endpoint so that deleting a missing item returns 404 JSON {error: 'Not found'} instead of a server error. Do not create or run tests.
```

![Fix delete](./images/sdlc118.png?raw=true "Fix delete")

<br><br>


5. After Copilot processes this, you should see a changed app.py file. Let's add Copilot as a reviewer to have it take a look. Go to the diff (green part) and right-click and select the menu item `Review`.

![Add Copilot review](./images/sdlc119.png?raw=true "Add Copilot review")

<br><br>

6. Copilot should review the proposed changes and offer any suggestions. For this case, it may or may not have any suggestions. If it doesn't have any suggestions ,you can just select "OK". If it does have suggestions, they will show up in a *COMMENTS* tab in the same area as the *TERMINAL* tab. You can then look at each one and decide whether to Apply/Discard using the provided buttons.

![Review output](./images/sdlc120.png?raw=true "Review output")

<br><br>

8. Once you are satisfied with the set of changes and reviews, go ahead and click one of the Keep buttons to save the changes.

![Keep](./images/sdlc26.png?raw=true "Keep")

<br><br>

9. (Only if needed.) If Copilot got it wrong and you now have errors (reported in the *PROBLEMS* tab at the bottom), you can right click and select "Fix with Copilot" and follow-through on the process from there.

![Fix if needed](./images/sdlc71.png?raw=true "Fix if needed")  

<br><br>

10. Now, you can repeat step #1 (restart the app if it stopped) and hopefully you'll see a 404 error "Not found" instead of a 500 one.

![Fixed code](./images/sdlc27.png?raw=true "Fixed code")

<br><br>

<p align="center">
<b>[END OF LAB]</b>
</p>
</br></br>

**Lab 4: Driving Testing with AI**

**Purpose: In this lab, we'll see how to use AI to generate tests for our code.**

<br><br>

1. For this lab, we'll utilize Copilot's Agent functionality again. Start a new chat by clicking on the large "+" button in the upper right of the Chat panel. The mode should be set to `Agent` as before. 

<br><br>
   
2. We want Copilot to generate unit tests for our datastore code and integration tests for each of our endpoints. Enter the prompt below into chat and submit.

```
Write pytest unit tests for DataStore (all CRUD + search) and Flask integration tests for each endpoint (including auth failure).
```

![Prompt for tests](./images/sdlc121.png?raw=true "Prompt for tests")

<br><br>

3. As this runs, if you encounter points where Copilot wants to run commands in the terminal and/or offers a "Continue" button, go ahead and accept that. Or if it offers an "Allow" button, you can click on that. If it simply notes commands and stops, go ahead and copy and paste those into the terminal and run them.

![Continue to execute command](./images/sdlc122.png?raw=true "Continue to execute command")

<br><br>

4. Another possibility is that it will stop with a "Let me know" type of statement. If it does, you can tell it to proceed or some similar statement as shown below.

![Telling Copilot to proceed](./images/sdlc73.png?raw=true "Telling Copilot to proceed")

<br><br>

5. After the testing commands are run, you should hopefully see a clean run and the agent will notify you that things have completed successfully. (If not, you may have to go through several iterations of interacting with Copilot while the agent fixes things to have passing tests.)

![Clean test run](./images/sdlc123.png?raw=true "Clean test run")

<br>

![Clean results](./images/sdlc32.png?raw=true "Clean results")

<br><br>

6. If you have any failing tests still, you can try adding a prompt of "Tests do not run cleanly" and submit that to Copilot. Again, accept any attempts to run things in the terminal. Or, you can start a new Agent mode chat session by clicking the "+" sign in the upper left to start a new chat session and try the same prompt again.

<br><br>
   
7. You should now see test files for app integration tests and unit tests for the datastore pieces. Make sure to save your testing files with one of the *Keep* buttons.

![New test files to keep](./images/sdlc33.png?raw=true "New test files to keep")

<br><br>

8. Now, let's see how else AI can help us with testing by entering a prompt (in the Chat panel and still in "Agent" mode) asking what else to test. 

```
What else in the #codebase should we test? 
```
![What else](./images/sdlc34.png?raw=true "What else")

<br><br>

9. Copilot should suggest some other test cases and then ask if it should add them. You can tell it to add the most important ones with a prompt like the one below.

```
Just add the most important ones.
```

![Add most important](./images/sdlc125.png?raw=true "Add most important")

<br><br>

10. After this, it may also suggest a command in the terminal to run to verify the tests. If so, click on Continue. Or it might tell you which ones are most important, and you have to tell it again to add them and go through the interactive process with it again.

![Add most important](./images/sdlc36.png?raw=true "Add most important")

<br><br>

11. If the command fails, it should suggest a fix. If so, you can accept that and then complete the cycle. Save files with any changes.

![Add most important](./images/sdlc38.png?raw=true "Add most important")   

<br><br>

12. (Optional) If you have time and want, you can prompt the AI with other prompts for other testing, such as the following:

```
What edge cases in #codebase should I test?
How do I test for performance in #codebase?
How do I test for security in #codebase?
```

<p align="center">
<b>[END OF LAB]</b>
</p>
<br><br>

**Lab 5: Refactoring and Updating Code via AI**

**Purpose: In this lab, we'll see how to use the AI to refactor targeted sets of code, both for efficiency and improvements.**

<br><br>

1.  Open a new chat and change Copilot's mode back to "Edit".

![Change to Edit](./images/sdlc75.png?raw=true "Change to Edit")

<br><br>

2. Now let's give the AI a targeted set of context to work with.  Add the 3 files (app/app.py, app/auth.py, and app/datastore.py) as context. You can do this in a couple of ways. You can drag and drop the files from the explorer file list on the left into the dialog area or you can use the "Add Context" button and select the files. Or you can just add the "app" directory. (You may need to click on "Files and Folders" in the context picker dialog.) **If other files show up as context, you can click on them in the dialog and an "X" should show up to remove them. (Or you can close them if they're open in the current tab in the IDE.)**

![Selecting files for context](./images/sdlc76.png?raw=true "Selecting files for context")

<br>

![Add context](./images/sdlc45.png?raw=true "Add context")

<br><br>
   
3. Let's ask Copilot to refactor our selected files to be more efficient. Enter the prompt below.

```
Refactor the files to make them more efficient.
```

![Refactor](./images/sdlc127.png?raw=true "Refactor")

<br><br>

4. After this runs, you will likely see output like the screenshot below. Copilot will analyze the targeted files and identify areas it thinks efficiencies can be made. It will then offer the usual diffs and Keep/Undo options.

![Refactor suggestions](./images/sdlc77.png?raw=true "Refactor suggestions")

<br><br>

5. You can go ahead and review the changes if you want and then Keep/Undo as needed. If you wanted, you could also add Copilot as a reviewer. (If you do add Copilot as a reviewer, don't forget to select the range in the pop-up dialog at the top.) **Next steps assume you're done with those changes and have accepted (keep) or discarded (undo) them.**

<br><br>

6. Now, let's look at how to use Copilot to add another feature. Open a new chat via the "+" control at the top. This time, we'll just use  our *datastore.py* and *app.py* files as context (if not already added). You can use the same approach as in step 2 to get those files as context.

<br><br>

7. Let's add logging to our functions. In the prompt area, add the prompt "Add logging for all endpoints". When ready, click Submit.

![Logging prompt](./images/sdlc46.png?raw=true "Logging prompt")

<br><br>

8. After this runs, you should see changes in *app.py*. Navigate through them and Apply/Keep changes as warranted.

![Logging changes made](./images/sdlc47.png?raw=true "Logging changes made")

<br><br>

9. (Optional) To show that the logging works, you can use the script we used previously in the "scripts" directory named use-app.sh. Running it now should cause INFO messages to be output to stderr. (Don't forget to make sure the app is running first in a separate terminal via *python app.py* If you hit errors running the app, it's possible that some edits could have affected the app code. You can compare against the original app code at https://github.com/skillrepos/ai-sdlc/blob/main/app/app.py.)

```
../scripts/use-app.sh
```

![Logging events](./images/sdlc48.png?raw=true "Logging events")


<br><br>

<p align="center">
<b>[END OF LAB]</b>
</p>
</br></br>

**Lab 6: Easy Documentation with AI**

**Purpose: In this lab, we'll see how to use AI to quickly and easily create different kinds of documentation for our project.**

<br><br>

1. Let's start out by telling our AI to generate basic doc for our app.py file. Open the app.py file in the editor if it isn't already. Activate the inline chat dialog via Ctrl+I or Option+I and enter the following shortcut command and hitting *Enter* or submitting it:

```
/doc
```

![doc command](./images/sdlc49.png?raw=true "doc command")

<br><br>

2. After this, you'll probably see a large "chunk" of comments at the start of code. You can go ahead and "Accept" that via the button in the dialog.

![doc results](./images/sdlc50.png?raw=true "doc results")

<br><br>

3. To get comments in the body of the code, we need to further prompt the AI. Let's tell Copilot to verbosely comment the code. Bring up the inline chat dialog and enter the prompt below in Copilot. (Optional: You can also choose to change the model that's being used by clicking on the model name. In the dialog, select a model that is "1x" or "0x" so it "cost" the same from your quota to use.  You might have to click an "Enable" button afterwards to enable the model access.)  Hit Enter/submit when done. (**NOTE:** If you don't get comments generated throughout the code, try issuing the same prompt in the main chat interface instead of the inline one.)

```
Verbosely comment all code in this file so it is easy to follow and understand
```

![verbose and change model](./images/sdlc78.png?raw=true "verbose and change model")

<br><br>

4. If you had to click the Enable button, you may need to input the same prompt again. Or, if don't see any results still, you can switch to a different model and try again. When ready, you can review the changes and select to "Accept" or "Close" in the inline chat dialog.

![review changes](./images/sdlc89.png?raw=true "review changes")

<br><br>
   
5. In the main chat panel, open a new chat (using the "+" control in the top right) and switch the mode back to "Ask". 

<br><br>

6. Let's generate documentation in the style of Sphinx (https://www.sphinx-doc.org/en/master/).  Now, in the main chat text area, enter the prompt below:

```
Generate Sphinx-style .rst API documentation for this Flask service
```

![generate Sphinx-style doc](./images/sdlc52.png?raw=true "Generate Sphinx-style doc")

<br><br>

7. Let's try another example. Let's have the AI generate simple functional documentation that we can share with others. Use the prompt below for this:

```
Generate functional documentation for the app and create a new block
```

<br><br>

8. After the documentation is generated, you can hover over the output and insert it into a new file if you want. If you then save the file with a .md extension, you'll be able to see the document in Markdown format. (You can use the three-bar menu, in the upper left of the codespace, then select "File", then "Save As...".)

![Insert into new file](./images/sdlc83.png?raw=true "Insert into new file")

<br> 

![Save functional doc](./images/sdlc54.png?raw=true "Save functional doc")

<br><br>

 <p align="center">
<b>[END OF LAB]</b>
</p>
</br></br>

**Lab 7: Using AI to Simplify Onboarding/Explaining Code**

**Purpose: To show how AI can be used to explain code and also help with onboarding those new to a codebase.**

<br><br>

1. Switch back to Agent mode for this lab. (If you do it in Ask mode, it will try to answer for all the different types of files in the project, rather than just the "app" code.

<br><br>

2. Let's start out having Copilot explain the code to us. Enter the prompt below in one of the chat interfaces.
```
Explain in simple terms how this code works
```

![Explain code](./images/sdlc55.png?raw=true "Explain code")

<br><br>

3. Someone just starting out with this code would need to also know how to run it, so let's have the AI explain how to do that as well.
```
Provide examples of how to run this code
```

![How to run](./images/sdlc56.png?raw=true "How to run")

<br><br>

4. Let's also use the AI to try and anticipate any potential issues new users may run into. Here's a prompt for that.
```
What are the most likely problems someone new to this codebase would run into. Explain the issue clearly and succinctly.
```

![Most likely problems](./images/sdlc57.png?raw=true "Most likely problems")

<br><br>

5. Let's take this a step further and have the AI create a general guide for new users to the code. You can use the prompt below. When done, you can hover over the code block and insert into a new file and then save as a .md file to see the formatting.
   
```
Create an onboarding guide for anyone brand new to this code who will be working with it or maintaining it.
```

![Onboarding guide](./images/sdlc58.png?raw=true "Onboarding guide")

<br><br>

6. Finally, let's have the AI generate some basic Q&A to check understanding and learning for someone looking at the code. Try this prompt:
```
Construct 10 questions to check understanding of how the code works. Then prompt the user on each question and check the answer. If the answer is correct, provide positive feedback. If the answer is not correct, explain why and provide the correct answer.
```

![Checking for understanding](./images/sdlc59.png?raw=true "Checking for understanding")

<br><br>

7. If you want, you can play along to check understanding. Just type your response in the chat dialog and then submit it. Copilot will tell you if the answer is right or wrong (with any needed explanation) and then move on to the next question.

![Checking for understanding](./images/sdlc84.png?raw=true "Checking for understanding")

<br><br>
   
 <p align="center">
<br>[END OF LAB]</b>
</p>
</br></br></br>

**THE END**
