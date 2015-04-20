# JavaScript的调试

随着JavaScript项目**复杂性**的增加，开发者需要强大的调试工具，以帮助快速发现bug的原因，并有效地解决它。在Chrome DevTools中包含了许多有用的工具，以减轻开发者**调试**JavaScript的痛苦。

在这一章节，我们将学习如何使用这些工具，来调试此页面上[Google Closure hovercard demo](https://rawgit.com/google/closure-library/master/closure/goog/demos/hovercard.html)和其他例子。

> **注意**：如果你是一个Web开发人员，并希望得到DevTools的最新版本，您应该使用[Chrome Canary](https://tools.google.com/dlpage/chromesxs)。

## 源文件面板[#](#sources-panel "Permalink")

**源文件面板**提供了调试JavaScript代码的功能。它提供了图形化的[V8](https://code.google.com/p/v8/) 调试器。你可以按照下面的步骤来学习**源文件面板**：

*   打开一个网站如 [Google Closure hovercard demo page](http://closure-library.googlecode.com/svn/trunk/closure/goog/demos/hovercard.html) 或 [TodoMVC](http://todomvc.com/examples/angularjs/) Angular应用。
*   打开DevTools窗口。
*   如果**源文件面板**没有被选择，就选中它。

![](javascript-debugging/javascript-debugging-overview.jpg)

可以让你看到所有是检查网页的一部分脚本。标准控件暂停，恢复，并通过代码步骤提供下面的面板选择图标。 A键强制暂停在例外位于窗口的底部。来源是在单独的标签并点击可见！[]（../图像/显示文件-navigator.png）打开文件导航仪会显示所有打开的脚本。

**源文件面板**可以让你看到你所要检查的页面的所有脚本代码，并在面板选择栏下方提供了一组标准控件，提供了暂停，恢复，step through code等功能。 A button to force a pause at exceptions is located at the bottom of the window. Sources are visible in separate tabs and clicking ![](../images/show-file-navigator.png)opens the file navigator which will display all open scripts.

### Execution control[#](#execution-control "Permalink")

The **execution control** buttons are located at the top of the side panels and allow you to step through code. The buttons available are:

**![](../images/continue.jpg)Continue:** continues code execution until we encounter another breakpoint.

**![](../images/step_over.jpg)Step over:** step through code line-by-line to get insights into how each line affects the variables being updated. Should your code call another function, the debugger won't jump into its code, instead stepping over so that the focus remains on the current function.

**![](../images/step_into.jpg)Step into:** like Step over, however clicking Step into at the function call will cause the debugger to move its execution to the first line in the functions definition.

**![](../images/step_out.jpg)Step out:** having stepped into a function, clicking this will cause the remainder of the function definition to be run and the debugger will move its execution to the parent function.

**![](../images/disable-breakpoints.png)Toggle breakpoints:** toggles breakpoints on/off while leaving their enabled states intact.

There are also several related keyboard shortcuts available in the **Sources** panel:

*   **Continue**: &amp;lt;kbd class="kbd"&amp;gt;F8&amp;lt;/kbd&amp;gt; or &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Command&amp;lt;/kbd&amp;gt; + &amp;lt;kbd class="kbd"&amp;gt;/&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on Mac or &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Ctrl&amp;lt;/kbd&amp;gt; + &amp;lt;kbd class="kbd"&amp;gt;/&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on other platforms.
*   **Step over**: &amp;lt;kbd class="kbd"&amp;gt;F10&amp;lt;/kbd&amp;gt; or &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Command&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;'&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on Mac or &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Ctrl&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;'&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on other platforms.
*   **Step into**: &amp;lt;kbd class="kbd"&amp;gt;F11&amp;lt;/kbd&amp;gt; or &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Command&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;;&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on Mac or &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Ctrl&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;;&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on other platforms.
*   **Step out**: &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Shift&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;F11&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; or &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Shift&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;Command&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;;&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on Mac or &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Shift&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;Ctrl&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;;&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on other platforms.
*   **Next call frame**: &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Ctrl&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;.&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on all platforms.
*   **Previous call frame**: &amp;lt;kbd&amp;gt;&amp;lt;kbd class="kbd"&amp;gt;Ctrl&amp;lt;/kbd&amp;gt;+&amp;lt;kbd class="kbd"&amp;gt;,&amp;lt;/kbd&amp;gt;&amp;lt;/kbd&amp;gt; on all platforms.

For a walkthrough of other keyboard shortcuts supported, see _[Shortcuts](shortcuts.html)_.

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Debugging with breakpoints[#](#breakpoints "Permalink")

A **breakpoint** is an _intentional_ stopping or pausing place in a script. Use breakpoints in DevTools to debug JavaScript code, DOM updates, and network calls.

### Add and remove breakpoints[#](#add-remove-breakpoints "Permalink")

In the **Sources** panel, open a JavaScript file for debugging. In the example below, we are debugging the **todoCtrl.js** file from the [AngularJS version of TodoMVC](http://todomvc.com/architecture-examples/angularjs/).

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/sources-select-todoCtrl-js.png)&amp;lt;/div&amp;gt;

Click the **line gutter** to set a breakpoint for that line of code. A blue tag will indicate if a breakpoint has been set:

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/sources-view-region.jpg)&amp;lt;/div&amp;gt;

You can add multiple breakpoints. Click the **line gutter** of another line to set another breakpoint. All the breakpoints you have set appear under **Breakpoints** in the right-hand sidebar.

Breakpoints can be enabled or disabled using the checkboxes in this sidebar. If a breakpoint is disabled, the blue tag will appear faded out.

Click on a breakpoint entry to jump to that particular line in the source file:

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/multiple-breakpoints-region.jpg)&amp;lt;/div&amp;gt;

Remove a breakpoint by clicking the **blue tag** breakpoint indicator.

Right-click on the **blue tag** to access a menu with several options including: **Continue to Here**, **Remove Breakpoint**, **Edit Breakpoint**, and **Disable Breakpoint**.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/continue-to-here-region.jpg)&amp;lt;/div&amp;gt;

To set a **conditional breakpoint**, choose **Edit Breakpoint**. Alternatively, right-click on the **gutter line** and choose **Add Conditional Breakpoint**. In the input field, type any expression that could resolve to true or false. The breakpoint will pause code execution only if the condition is true.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/conditional-breakpoint-region.jpg)&amp;lt;/div&amp;gt;

Conditional breakpoints are especially useful when you're looking to analyze code in a loop or an event callback that fires often.

**Note:** It may not be desirable to set breakpoints from the DevTools interface. Should you wish to launch the debugger from your code, you may use [the `debugger` keyword](console.md#setting-breakpoints-in-javascript) to achieve this.

### Interact with paused breakpoints[#](#breakpoints-paused "Permalink")

Once you have one or more breakpoints set, return to the browser window and interact with your page. In the example below, a breakpoint was added within `removeTodo()`. Now any attempts to delete a todo item in the TodoMVC app will trigger a breakpoint pause:

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/breakpoint-paused-app.png)&amp;lt;/div&amp;gt;

To resume code execution, click the **Continue** ![](../images/continue.jpg)button or use the &amp;lt;span class="kbd"&amp;gt;F8&amp;lt;/span&amp;gt; keyboard shortcut in the DevTools window.

While a script is paused, you can interact with the **Watch Expressions**, **Call Stack**, and **Scope Variables** panels in the right-hand side bar.

#### Call Stack panel[#](#call-stack-panel "Permalink")

The **Call Stack** panel displays the complete execution path that led to the point where code was paused, giving us insights into the code flaws that caused the error.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/callstack-region.png)&amp;lt;/div&amp;gt;

To view the execution path including asynchronous JavaScript callbacks such as timer and XHR events, check the **Async** checkbox.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/enable-async-toggle.png)&amp;lt;/div&amp;gt;

Further information and examples using async call stacks can be found in [Debugging Asynchronous JavaScript with Chrome DevTools](http://www.html5rocks.com/en/tutorials/developertools/async-call-stack/) on HTML5Rocks.com.

#### Blackbox JavaScript files[#](#blackboxing "Permalink")

When you blackbox a JavaScript source file, you will not jump into that file when stepping through code you're debugging. You are able to debug just the code you are interested in.

&amp;lt;div class="screenshot"&amp;gt;![](blackboxing-files/blackboxing-expanded.png)&amp;lt;/div&amp;gt;

You can use the Settings panel to blackbox scripts, or right-click in the sources panel on a file and choose Blackbox Script from the context menu.

&amp;lt;div class="screenshot"&amp;gt;![](blackboxing-files/blackboxing-dialog.png)&amp;lt;/div&amp;gt;

More information on blackboxing and how to use it can be found in the [Blackboxing JavaScript files](https://developer.chrome.com/devtools/docs/blackboxing).

#### Console drawer[#](#mini-console-panel "Permalink")

The DevTools **console drawer** will allow you to experiment within the scope of where the debugger is currently paused. Hit the &amp;lt;span class="kbd"&amp;gt;Esc&amp;lt;/span&amp;gt; key to bring the console into view. The &amp;lt;span class="kbd"&amp;gt;Esc&amp;lt;/span&amp;gt; key also closes this drawer.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/console-scope-time-travel.gif)&amp;lt;/div&amp;gt;

### Breakpoints in Dynamic JavaScript[#](#breakpoints-dynamic-javascript "Permalink")

[This is used to mark dynamicScript.js as used.](javascript-debugging/dynamicScript.js) &lt;script&gt;function loadDynamicScript() { var request = new XMLHttpRequest(); request.open('GET','https://developer.chrome.com/devtools/docs/javascript-debugging/dynamicScript.js', true); request.send(); request.onreadystatechange = function() { if (request.readyState != 4) return; eval(request.responseText); document.getElementById("dynamicScriptFunctionButton").disabled = false; document.getElementById("loadDynamicScriptButton").disabled = true; } }&lt;/script&gt;
*   &amp;lt;button id="loadDynamicScriptButton" onclick="loadDynamicScript()"&amp;gt;Load dynamic script&amp;lt;/button&amp;gt;
*   In the **Sources** panel select "dynamicScript.js" from scripts drop-down and set breakpoint on line 2
*   &amp;lt;button id="dynamicScriptFunctionButton" onclick="dynamicScriptFunction()" disabled=""&amp;gt;Call function from dynamic script&amp;lt;/button&amp;gt;
*   You should stop on the breakpoint
*   Click the **Continue** ![](../images/continue.jpg)button or hit **F8** in DevTools window to resume

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/dynamic-script.jpg)&amp;lt;/div&amp;gt;

**Note:** Notice the `"//# sourceURL=dynamicScript.js"` line at the end of dynamicScript.js file. This technique gives a name to a script created with eval, and will be discussed in more detail in the [Source Maps](#source-maps) section. Breakpoints can be set in dynamic JavaScript only if it has a user supplied name.

### Pause on Next JavaScript Statement[#](#pause-on-next-statement "Permalink")

&lt;script&gt;document.addEventListener("mouseover", onMouseOver, true); function onMouseOver(event) { var target = event.target; return "onMouseOver: " + target; }&lt;/script&gt;
*   Click the **Pause** ![](../images/pause-icon.png)button
*   Move your mouse over this section
*   You should stop in `onMouseOver` function
*   Click the **Continue** ![](../images/continue.jpg)button or hit **F8** in DevTools window to resume

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/continue-to-resume.jpg)&amp;lt;/div&amp;gt;

### Pause on Exceptions[#](#pause-on exceptions "Permalink")

&lt;script&gt;function raiseAndCatchException() { var element = document.createElement("div"); try { document.body.appendChild(elemetn); } catch(e) { console.log(e); } }&lt;/script&gt;
*   Click the **Pause on exceptions** ![](../images/pause-gray.png)button at the bottom of the window to switch to **Pause on all exceptions** mode
*   &amp;lt;button onclick="raiseAndCatchException()"&amp;gt;Raise exception!&amp;lt;/button&amp;gt;
*   You should stop in &amp;lt;span class="source-code"&amp;gt;raiseAndCatchException&amp;lt;/span&amp;gt; function
*   Click the **Continue** ![](../images/continue.jpg)button or hit **F8** in DevTools window to resume

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/append-child.jpg)&amp;lt;/div&amp;gt;

### Pause on Uncaught Exceptions[#](#pause-on-uncaught-exceptions "Permalink")

&lt;script&gt;function raiseAndCatchException() { var element = document.createElement("div"); try { document.body.appendChild(elemetn); } catch(e) { console.log(e); } } function raiseException() { throw 0; }&lt;/script&gt;
*   Click the **Pause on exceptions** ![](../images/pause-blue.png)button again to switch to **Pause on uncaught exceptions** mode
*   &amp;lt;button onclick="raiseAndCatchException()"&amp;gt;Raise exception!&amp;lt;/button&amp;gt;
*   You should not stop in raiseAndCatchException function since exception is caught
*   &amp;lt;button onclick="raiseException()"&amp;gt;Raise uncaught exception!&amp;lt;/button&amp;gt;
*   You should stop in `raiseException` function
*   Click the **Continue** ![](../images/continue.jpg)button or hit **F8** in DevTools window to resume

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/raise-exception.jpg)&amp;lt;/div&amp;gt;

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Breakpoints on DOM Mutation Events[#](#breakpoints-mutation-events "Permalink")

&lt;script&gt;function appendChildButtonClicked() { var parentElement = document.getElementById("parent"); var childElement = document.createElement("div"); childElement.setAttribute("style", "border: 2px solid; padding: 5px; margin: 5px; text-align: center; width: 120px"); childElement.textContent = "Child Element"; parentElement.appendChild(childElement); }&lt;/script&gt;
*   Right click on the "Parent Element" below and select **Inspect Element** from context menu
    &amp;lt;div id="parent" style="border: solid 2px; padding: 5px; margin: 5px; text-align: center; width: 140px"&amp;gt;Parent Element&amp;lt;/div&amp;gt;

*   Right click on the **Elements**' panel
    &amp;lt;div id="parent" ...=""&amp;gt;element and select **Break on Subtree Modifications**&amp;lt;/div&amp;gt;

*   &amp;lt;button onclick="appendChildButtonClicked()"&amp;gt;Append child!&amp;lt;/button&amp;gt;
*   You should stop on `appendChild` function call
*   Click the **Continue** ![](../images/continue.jpg)button or hit **F8** in DevTools window to resume

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/append-child-element.jpg)&amp;lt;/div&amp;gt;

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Breakpoints on XHR[#](#breakpoints-on-xhr "Permalink")

[This is used to mark data.txt as used.](javascript-debugging/data.txt) &lt;script&gt;function retrieveData() { var request = new XMLHttpRequest(); request.open('GET','javascript-debugging/data.txt', true); request.send(); }&lt;/script&gt;
*   Click the **Add** ![](../images/plus.png)button on **XHR Breakpoints** sidebar pane on the right side of **Sources** panel
*   Type "data.txt" in text input and hit **enter**
*   &amp;lt;button onclick="retrieveData()"&amp;gt;Retrieve data.txt by XHR&amp;lt;/button&amp;gt;
*   You should stop on `send` function call
*   Right-click on the newly created breakpoint and select **Remove Breakpoint** context menu item
*   Click the **Continue** ![](../images/continue.jpg)button or hit **F8** in DevTools window to resume

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/request-send.jpg)&amp;lt;/div&amp;gt;

**Note:** To edit URL filter, double click on the XHR breakpoint entry in **XHR Breakpoints** sidebar pane. XHR breakpoint with empty URL filter will match any XHR.

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Breakpoints on JavaScript Event Listeners[#](#breakpoints-on-javascript-event-listeners "Permalink")

&lt;script&gt;window.addEventListener("load", onLoad, true); function onLoad() { var hovermeElement = document.getElementById("hoverme"); hovermeElement.addEventListener("mouseover", hovermeMouseOver, true); hovermeElement.addEventListener("mouseout", hovermeMouseOut, true); } function hovermeMouseOver(event) { event.target.style.backgroundColor = "grey"; } function hovermeMouseOut(event) { event.target.style.backgroundColor = "white"; }&lt;/script&gt;
*   Expand **Event Listener Breakpoints** sidebar pane on the right side of **Scripts** panel
*   Expand **Mouse** entry
*   Set a mouseout Event Listener breakpoint by clicking on the checkbox near the **mouseout** entry

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/resumed.jpg)&amp;lt;/div&amp;gt;

*   Move your mouse across the box below:
*   &amp;lt;div id="hoverme" style="border: solid 2px; padding: 5px; margin: 5px; text-align: center; width: 100px"&amp;gt;Hover me!&amp;lt;/div&amp;gt;

*   You should stop on `mouseout` event handler
*   Click the **Continue** ![](../images/continue.jpg)button or hit **F8** in DevTools window to resume

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/continue-to-resume.jpg)&amp;lt;/div&amp;gt;

**Note: Following events are supported** &amp;nbsp;&amp;nbsp;**Keyboard:** keydown, keypress, keyup, textInput  
 &amp;nbsp;&amp;nbsp;**Mouse:** click, dblclick, mousedown, mouseup, mouseover, mousemove, mouseout, mousewheel  
 &amp;nbsp;&amp;nbsp;**Control:** resize, scroll, zoom, focus, blur, select, change, submit, reset  
 &amp;nbsp;&amp;nbsp;**Clipboard:** copy, cut, paste, beforecopy, beforecut, beforepaste  
 &amp;nbsp;&amp;nbsp;**Load:** load, unload, abort, error  
 &amp;nbsp;&amp;nbsp;**DOM Mutation:** DOMActivate, DOMFocusIn, DOMFocusOut, DOMAttrModified, DOMCharacterDataModified, DOMNodeInserted, DOMNodeInsertedIntoDocument, DOMNodeRemoved, DOMNodeRemovedFromDocument, DOMSubtreeModified, DOMContentLoaded  
 &amp;nbsp;&amp;nbsp;**Device:** deviceorientation, devicemotion

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## The Long Resume[#](#long-resume "Permalink")

When paused, click and hold the resume button to "Resume with all pauses blocked for 500 ms". This makes all breakpoints disabled for half a second, naturally. Use this to get to the next event loop, so you can avoid hitting breakpoints continually exiting a loop, for example.

Quick protip: When "refresh" is initiated from DevTools (Ctrl+R while focused in DevTools), all pauses are disabled until the new page load is commenced (or as a plan B, until the user presses the "Pause" button). However, if you initiate refresh from the browser button (or Ctrl+R while focus is outside the DevTools), any remaining breakpoints will actually be hit. This fact could be used for those who are interested in page unloading process.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/long-resume.png)&amp;lt;/div&amp;gt;

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Live Editing[#](#liveedit "Permalink")

In **Authoring And Workflow**, we discussed how to make changes to scripts in the **Sources** panel. While at a breakpoint, it's also possible to live edit scripts by clicking into the main editor panel and making modifications.

*   Navigate to the Google Closure hovercard demo
*   In the Sources panel, open up "mouse.js" and use the Ctrl/Cmd + Shift + O to navigate to the onMouseOut() function

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/houseMouseOut.jpg)&amp;lt;/div&amp;gt;

*   Click the **pause** button to pause debugging
*   Modify the function, adding a console.log('Moused out') to the end
*   Using the Cmd +S or Ctrl + S shortcuts will save these modifications. Make sure to Save As.
*   Click the **pause/resume** button to resume execution
*   When you now hover out, the new message will be logged to the console

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/pause-resume-mouseout.jpg)&amp;lt;/div&amp;gt;

This allows you to saved changes from within the DevTools without having to leave your browser.

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Exceptions[#](#exceptions "Permalink")

Let's now look at how to deal with exceptions and stack traces using Chrome DevTools.  
 **Exception handling** is the process of responding to the occurrence of exceptions - exceptional situations that require special processing - often changing the normal flow of your JavaScript code's execution.

**Note:** If you are a Web Developer and want to get the latest version of DevTools, you should use [Chrome Canary](https://tools.google.com/dlpage/chromesxs).

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Tracking exceptions[#](#tracking-exceptions "Permalink")

When something goes wrong, you can open the DevTools console (Ctrl+Shift+J / Cmd+Option+J) and find a number of JavaScript error messages there. Each message has a link to the file name with the line number you can navigate to.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/tracking-exceptions.jpg)&amp;lt;/div&amp;gt;

### Viewing exception stack trace[#](#viewing-exception-stack-trace "Permalink")

There might be several execution paths that lead to the error and it's not always obvious which one of them has happened. **Once DevTools window is opened**, exceptions in the console are accompanied with the **complete JavaScript call stacks**. You can expand these console messages to see the stack frames and navigate to the corresponding locations in the code:

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/exception-stack-trace.jpg)&amp;lt;/div&amp;gt;

### Pause on JavaScript exceptions[#](#pause-on-exceptions "Permalink")

You may also want to pause JavaScript execution next time exception is thrown and inspect its call stack, scope variables and state of your app. A tri-state stop button ( ![](../images/pause-gray.png)) at the bottom of the Scripts panel enables you to switch between different exception handling modes: you can choose to either pause on all exception or only on the uncaught ones or you can ignore exceptions altogether.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/pause-execution.jpg)&amp;lt;/div&amp;gt;

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Printing stack traces[#](#printing-stack-traces "Permalink")

Printing log messages to the DevTools console may be very helpful in understanding how your application behaves. You can make the log entries even more informative by including associated stack traces. There are several ways of doing that.

### Error.stack[#](#error-stack "Permalink")

Each Error object has a string property named stack that contains the stack trace:

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/error-stack.jpg)&amp;lt;/div&amp;gt;

### console.trace()[#](#console-trace "Permalink")

You can instrument your code with console.trace() calls that would print current JavaScript call stacks:

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/console-trace.jpg)&amp;lt;/div&amp;gt;

### console.assert()[#](#console-assert "Permalink")

There is also a way to place assertion in your JavaScript code. Just call console.assert() with the error condition as the first parameter. Whenever this expression evaluates to false you will see a corresponding console record:

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/console-assert.jpg)&amp;lt;/div&amp;gt;

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Handling exceptions at runtime using window.onerror[#](#handling-exceptions-runtime "Permalink")

Chrome supports setting a handler function to window.onerror. Whenever a JavaScript exception is thrown in the window context and is not caught by any try/catch block, the function will be invoked with the exception's message, the URL of the file where the exception was thrown and the line number in that file passed as three arguments in that order. You may find it convenient to set an error handler that would collect information about uncaught exceptions and report it back to your server.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/window-onerror.jpg)&amp;lt;/div&amp;gt;

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Pretty Print[#](#pretty-print "Permalink")

If you have trouble trying to read and debug minified JavaScript in the DevTools, a pretty printing option is available to make life easier. Here is how a minified script displayed in the tools might look prior to being displayed in the DevTools:

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/pretty-print-off.jpg)&amp;lt;/div&amp;gt;

And by clicking on the curly brace ![](../images/prettyprint-icon.png)("Pretty Print") icon in the bottom left corner, the JavaScript is transformed into a more human readable form. This is also more easy for debugging and setting breakpoints.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/pretty-print-on.jpg)&amp;lt;/div&amp;gt;

&lt;/div&gt;

&lt;div class="collapsible"&gt;
## Source Maps[#](#source-maps "Permalink")

Have you ever found yourself wishing you could keep your client-side code readable and more importantly debuggable even after you've combined and minified it? Well now you can through the magic of [source maps](https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k/edit?hl=en_US&amp;amp;pli=1&amp;amp;pli=1).

A source map is a JSON-based mapping format that creates a relationship between a minified file and its sources.

Here's an example of a simple source map:

&amp;lt;pre class="prettyprint"&amp;gt;&amp;lt;span class="pun"&amp;gt;{&amp;lt;/span&amp;gt;&amp;lt;span class="pln"&amp;gt;
    version &amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;:&amp;lt;/span&amp;gt;&amp;lt;span class="lit"&amp;gt;3&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;,&amp;lt;/span&amp;gt;&amp;lt;span class="pln"&amp;gt;
    file&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;:&amp;lt;/span&amp;gt;&amp;lt;span class="str"&amp;gt;"out.min.js"&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;,&amp;lt;/span&amp;gt;&amp;lt;span class="pln"&amp;gt;
    sourceRoot &amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;:&amp;lt;/span&amp;gt;&amp;lt;span class="str"&amp;gt;""&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;,&amp;lt;/span&amp;gt;&amp;lt;span class="pln"&amp;gt;
    sources&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;:&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;[&amp;lt;/span&amp;gt;&amp;lt;span class="str"&amp;gt;"foo.js"&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;,&amp;lt;/span&amp;gt;&amp;lt;span class="str"&amp;gt;"bar.js"&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;],&amp;lt;/span&amp;gt;&amp;lt;span class="pln"&amp;gt;
    names&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;:&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;[&amp;lt;/span&amp;gt;&amp;lt;span class="str"&amp;gt;"src"&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;,&amp;lt;/span&amp;gt;&amp;lt;span class="str"&amp;gt;"maps"&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;,&amp;lt;/span&amp;gt;&amp;lt;span class="str"&amp;gt;"are"&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;,&amp;lt;/span&amp;gt;&amp;lt;span class="str"&amp;gt;"fun"&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;],&amp;lt;/span&amp;gt;&amp;lt;span class="pln"&amp;gt;
    mappings&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;:&amp;lt;/span&amp;gt;&amp;lt;span class="str"&amp;gt;"AAgBC,SAAQ,CAAEA"&amp;lt;/span&amp;gt;&amp;lt;span class="pun"&amp;gt;}&amp;lt;/span&amp;gt;&amp;lt;/pre&amp;gt;

The idea is that when you build for production, along with minifying and combining your JavaScript files, you generate a source map that holds information about your original files. The source map causes DevTools to load your original files in addition to your minified ones. You then use the originals to set breakpoints and step through code. Meanwhile, Chrome is actually running your minified code. This gives you the illusion of running a development site in production.

### Using Source Maps[#](#using-source maps "Permalink")

**Use the Right Minifier**

You need to use a minifier that's capable of creating source maps. Closure Compiler and UglifyJS 2.0 are two such tools but there are also tools available that create source maps for CoffeeScript, SASS and many others. See the [Source maps: languages, tools and other info](https://github.com/ryanseddon/source-map/wiki/Source-maps:-languages,-tools-and-other-info) wiki page.

**Configure DevTools**

Sourcemaps are enabled by default (as of Chrome 39), but if you'd like to double-check or enable them, first open DevTools and click the settings cog ![gear](../images/gear.png). Under **Sources**, check **Enable JavaScript source maps**. You might also check **Enable CSS source maps** but you do not need to for this example.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/source-maps.png)&amp;lt;/div&amp;gt;

**Make the Source Map Accessible**

To tell DevTools that a source map is available, verify the following line is at the end of the minified file.

`//# sourceMappingURL=/path/to/file.js.map`

This line, usually added by whatever tool generated the map, is what enables DevTools to associate minified with unminified files. In CSS, the line would look like `/*# sourceMappingURL=style.css.map */`.

If you don't want an extra comment in your file you can use an HTTP header field on the minified JavaScript file to tell DevTools where to find the source map. This requires configuration or customization of your web server and is beyond the scope of this document.

`X-SourceMap: /path/to/file.js.map`

Like the comment this tells DevTools where to look for the source map associated with a JavaScript file. This header also gets around the issue of referencing source maps in languages that don't support single-line comments.

You should also verify that your web server is configured to serve source maps. Some web servers, like Google App Engine for example, require explicit configuration for each file type served. In this case, your source maps should be served with a MIME type of `application/json`, but Chrome will actually [accept any content-type](http://stackoverflow.com/questions/19911929/what-mime-type-should-i-use-for-source-map-files), for example `application/octet-stream`.

Take a look at the special build of the [font dragr tool](http://dev.fontdragr.com) in Chrome, with source mapping enabled, and you'll notice that the JavaScript isn't compiled and you can see all the individual JavaScript files it references. This is using source mapping, but behind the scenes actually running the compiled code. Any errors, logs and breakpoints will map to the dev code for awesome debugging! So in effect it gives you the illusion that you're running a dev site in production.

### @sourceURL and displayName in action[#](#@sourceurl-and displayname in action "Permalink")

While not part of the source map spec the following convention allows you to make development much easier when working with evals.

This helper looks very similar to the `//# sourceMappingURL` property and is actually mentioned in the source map V3 specifications. By including the following special comment in your code, which will be evaled, you can name evals and inline scripts and styles so they appear as more logical names in your dev tools.

`//# sourceURL=source.coffee`

**Working with sourceURL**

*   Navigate to the **[demo](http://www.thecssninja.com/demo/source_mapping/compile.html)**
*   Open the DevTools and go to the **Sources** panel.
*   Enter in a filename into the _Name your code:_ input field.
*   Click on the **compile** button.
*   An alert will appear with the evaluated sum from the CoffeeScript source
*   If you expand the _Sources_ sub-panel you will now see a new file with the custom filename you entered earlier. If you double-click to view this file it will contain the compiled JavaScript for our original source. On the last line however will be a `// @sourceURL` comment indicating what the original source file was. This can greatly help with debugging when working with language abstractions.

&amp;lt;div class="screenshot"&amp;gt;![](javascript-debugging/coffeescript.jpg)&amp;lt;/div&amp;gt;

#### Read more[#](#read-more "Permalink")

*   Conditional breakpoints
*   [Breakpoint actions in JavaScript](http://www.randomthink.net/blog/2012/11/breakpoint-actions-in-javascript/)
*   Working With Source Maps
*   [The Breakpoint: Source maps spectacular](https://www.youtube.com/watch?feature=player_embedded&amp;amp;v=HijZNR6kc9A)
*   [HTML5 Rocks: An Introduction To JavaScript Source maps](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)
*   [NetTuts: Source Maps 101](http://net.tutsplus.com/tutorials/tools-and-tips/source-maps-101/)
*   [Source maps: languages, tools and other info](https://github.com/ryanseddon/source-map/wiki/Source-maps%3A-languages%2C-tools-and-other-info)
*   [CSS Ninja: Multi-level Source maps](http://www.thecssninja.com/javascript/multi-level-sourcemaps)
*   [Source maps for CoffeeScript](http://www.coffeescriptlove.com/2012/04/source-maps-for-coffeescript.html)

&lt;/div&gt;

&lt;footer id="cc-info"&gt;
Content available under the [CC-By 3.0 license](http://creativecommons.org/licenses/by/3.0/)

&lt;/footer&gt;

</div>