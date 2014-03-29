Debugging JavaScript
==
As the **complexity** of JavaScript applications increase, developers need
powerful debugging tools to help quickly discover the cause of an issue and fix
it efficiently. The Chrome DevTools include a number of useful tools to help
make **debugging** JavaScript less painful.

In this section, we will walk through how to use these tools by debugging the
[Google Closure hovercard
demo](http://closure-library.googlecode.com/svn/trunk/closure/goog/demos/hovercard.html)
and other dynamic examples in this page.

**Note:** If you are a Web Developer and want to get the latest version of
DevTools, you should use [Chrome Canary](tools.google.com/dlpage/chromesxs).

#### **Contents**

**

*   [The Sources Panel](#sources-panel)
*   [Debugging With Breakpoints](#breakpoints)

    *   [Breakpoints in Dynamic JavaScript](#breakpoints-dynamic-javascript)
    *   [Pause on Next JavaScript Statement](#pause-on-next-statement)
    *   [Pause on Exceptions](#pause-on-exceptions)
    *   [Pause on Uncaught Exceptions](#pause-on-uncaught-exceptions)

        *   [Breakpoints on DOM Mutation Events](#breakpoints-mutation-events)
    *   [Breakpoints on XHR](#breakpoints-on-xhr)
    *   [Breakpoints on JavaScript Event Listeners](#breakpoints-on-javascript-event-listeners)

*   [Live Editing](#liveedit)
*   [Handling Exceptions](#exceptions)

        *   [Tracking exceptions](#tracking-exceptions)

            *   [Viewing exception stack trace](#viewing-exception-stack-trace)
        *   [Pause on exceptions](#pause-on-exceptions)

        *   [Printing stack traces](#printing-stack-traces)

            *   [Error.stack](#error-stack)
        *   [console.trace()](#console-trace)
        *   [console.assert()](#console-assert)

        *   [Handling exceptions at runtime using window.onerror](#handling-exceptions-runtime)

*   [Pretty Print](#pretty-print)
*   [Working With Source Maps](#source-maps)
**

## The Sources Panel

The **Sources** panel lets you debug your JavaScript code. It provides a
graphical interface to the [V8](https://code.google.com/p/v8/) debugger. Follow the steps below to explore the
**Sources** panel:

*   Open a site such as the [Google Closure hovercard demo
page](http://closure-library.googlecode.com/svn/trunk/closure/goog/demos/hovercard.html) or the [TodoMVC](http://todomvc.com/architecture-examples/angularjs/) Angular app.
*   Open the DevTools window.
*   If it is not already selected, select **Sources**.

<div class="screenshot">![](javascript-debugging/javascript-debugging-overview.jpg)</div>

The **Sources** panel lets you see all the scripts that are part of the
inspected page. Standard controls to pause, resume, and step through code are
provided below the panel selection icons. A button to force a pause at
exceptions is located at the bottom of the window. Sources are visible in
separate tabs and clicking ![](javascript-debugging/image_1.png) opens the file navigator
which will display all open scripts.

### Execution control

The **execution control** buttons are located at the top of the side panels and allow you to step through code. The buttons available are:

**![](javascript-debugging/button_continue.jpg) Continue:** continues code execution until we encounter another breakpoint.

**![](javascript-debugging/button_step_over.jpg) Step over:** step through code line-by-line to get insights into how each line affects the variables being updated. Should your code call another function, the debugger won't jump into its code, instead stepping over so that the focus remains on the current function.

**![](javascript-debugging/button_step_into.jpg) Step into:** like Step over, however clicking Step into at the function call will cause the debugger to move its execution to the first line in the functions definition.

**![](javascript-debugging/button_stepout.jpg)Step out:** having stepped into a function, clicking this will cause the remainder of the function definition to be run and the debugger will move its execution to the parent function.

**![](javascript-debugging/button_disable.png)Toggle breakpoints:** toggles breakpoints on/off while leaving their enabled states intact.

There are also several related keyboard shortcuts available in the **Sources** panel:

*   **Continue**: F8 or Command-/ (forward slash) on Mac or Control-/ (forward
slash) on other platforms.
*   **Step over**: F10 or Command-' (apostrophe) on Mac or Control-' (apostrophe)
on other platforms.
*   **Step into**: F11 or Command-; (semi-colon) on Mac or Control-; (semi-colon);
on other platforms.
*   **Step out**: Shift-F11 or Shift-Command-; (semi-colon) on Mac or
Shift-Control-; (semi-colon) on other platforms.
*   **Next call frame**: Control-. (period) on all platforms.
*   **Previous call frame**: Control-, (comma) on all platforms.

For a walkthrough of other keyboard shortcuts supported, see _[Shortcuts](/chrome-developer-tools/docs/shortcuts)_.

[Back to top](#top)

## Debugging With Breakpoints

A breakpoint is an **intentional** stopping or pausing place in a script, put in
place for debugging purposes. Let's look at **debugging**JavaScript using
breakpoints set on JavaScript code and breakpoints targeted at UI and network
aspects of an application.

### Workflow

First, open the DevTools by hitting the **Control-Shift-I** shortcut. Click the
**line gutter** to set a breakpoint for that line of code, select another script
and set another breakpoint.

<div class="screenshot">![](javascript-debugging/sources-view-region.jpg)</div> 

All the breakpoints you have set appear under **Breakpoints** in the right-hand
sidebar. Breakpoints can be enabled or disabled using the checkboxes in this
sidebar.

<div class="screenshot">![](javascript-debugging/sources-breakpoints-region.jpg)</div> 

Clicking on the entry jumps to the **highlighted line** in the source file. You
can **set** one or more breakpoints in one or more scripts.

<div class="screenshot">![](javascript-debugging/multiple-breakpoints-region.jpg)</div> 

Delete a breakpoint by clicking the **blue tag** breakpoint indicator. The
breakpoint indicator menu has several options including "Continue to here",
"Edit Breakpoint", "Disable Breakpoint" and "Save As", which will save the file
locally.

<div class="screenshot">![](javascript-debugging/continue-to-here-region.jpg)</div> 

Once you have set a breakpoint, you can right-click on the blue arrow tag
indicator in the gutter line to set a **conditional statement** for that
specific breakpoint. Type any expression and the breakpoint will pause only if
the condition is true. The tag cycles through its three states of active,
inactive, and removed.

 <div class="screenshot">![](javascript-debugging/conditional-breakpoint-region.jpg)</div>

Click the **Pause** button then interact with your page.  

<div class="screenshot">![](javascript-debugging/callstack-region.jpg)</div> 

While a script is paused, you can see the current call stack and in-scope
variables in the right-hand side bar. The call stack displays the **complete execution path** that led to the point where code was paused, giving us insights into the code flaws that caused the error.

You may also open up the console to experiment while paused. Hit the <span class="kbd">Esc</span> key to bring the console into view.

<aside class="special">
  Remember: the console will be evaluating within the scope of where the debugger is currently paused.
  <p>
  <div class="screenshot">
    ![](javascript-debugging/evaluate-while-paused.jpg)
  </div>

</aside>

Remember, if your code happens to be using a loop which fires every 20ms, you likely won't want the debugger to **halt** on each iteration. Using conditional statements/breakpoints can assist with this. In the below example, code execution will only break if the `minLevel` variable is greater than 2004, helping us avoid a number of additional clicks in the debugger.

 <div class="screenshot">![](javascript-debugging/conditional.png)</div>

**Note:** It may not be desirable to set breakpoints from the DevTools interface. Should you wish to launch the debugger from your code, you may use [the `debugger` keyword](console#setting_breakpoints_in_javascript) to achieve this.

### Using Breakpoints

Make sure the **Sources** panel is open and select "script.js" from scripts
drop-down

*   Set a breakpoint on line 8 by clicking the line gutter (you can use the
**Control-G** shortcut to reveal a line in a large file)
*   Move your mouse over this page
*   You should stop on the breakpoint
*   Hover over the source code to inspect local and global variables, function
arguments etc.
*   Delete the breakpoint by clicking the blue tag breakpoint indicator
*   Click the **Continue** ![](javascript-debugging/image_9.png) button or hit **F8** in DevTools window to resume

<div class="screenshot">![](javascript-debugging/live-editing-mouseout.jpg)</div>

[Back to top](#top)

### Breakpoints in Dynamic JavaScript

[
  This is used to mark dynamicScript.js as used.](/chrome-developer-tools/docs/javascript-debugging/dynamicScript.js)

<iframe
  data-src="/chrome-developer-tools/docs/javascript-debugging_32075b0b5d0c449dbb9902582b454ee1.frame"
  class="framebox inherit-locale "
  style="width: 100%; height: 500px;">

[This section requires a browser that supports JavaScript and iframes.]

</iframe>

**Note:** Notice the `"//# sourceURL=dynamicScript.js"` line at the end of
dynamicScript.js file. This technique gives a name to a script created with eval, and will be discussed in more detail in the [Source Maps](#source-maps) section. Breakpoints can be set in dynamic JavaScript only if it has a user supplied
name.

[Back to top](#top)

### Pause on Next JavaScript Statement

<iframe
  data-src="/chrome-developer-tools/docs/javascript-debugging_093a06e23d17497ac884b138382419db.frame"
  class="framebox inherit-locale "
  style="width: 100%; height: 500px;">

[This section requires a browser that supports JavaScript and iframes.]

</iframe>

[Back to top](#top)

### Pause on Exceptions

<iframe
  data-src="/chrome-developer-tools/docs/javascript-debugging_4f811500dcdfdc61e301d13afcd3a93e.frame"
  class="framebox inherit-locale "
  style="width: 100%; height: 500px;">

[This section requires a browser that supports JavaScript and iframes.]

</iframe>

[Back to top](#top)

### Pause on Uncaught Exceptions

</h3>

<iframe
  data-src="/chrome-developer-tools/docs/javascript-debugging_74bdf2d9454a0b7363196a280691b2c4.frame"
  class="framebox inherit-locale "
  style="width: 100%; height: 500px;">

[This section requires a browser that supports JavaScript and iframes.]

</iframe>

[Back to top](#top)

## Breakpoints on DOM Mutation Events

<iframe
  data-src="/chrome-developer-tools/docs/javascript-debugging_6fd4d5602f676eeee20977d033525f28.frame"
  class="framebox inherit-locale "
  style="width: 100%; height: 500px;">

[This section requires a browser that supports JavaScript and iframes.]

</iframe>

[Back to top](#top)

## Breakpoints on XHR

[
  This is used to mark data.txt as used.](/chrome-developer-tools/docs/javascript-debugging/data.txt)

<iframe
  data-src="/chrome-developer-tools/docs/javascript-debugging_e5e6d14ab1476fdbbfd8d7a66973560e.frame"
  class="framebox inherit-locale "
  style="width: 100%; height: 500px;">

[This section requires a browser that supports JavaScript and iframes.]

</iframe>

**Note:** To edit URL filter, double click on the XHR breakpoint entry in **XHR Breakpoints** sidebar pane. XHR breakpoint with empty URL filter will match any
XHR.

[Back to top](#top)

## Breakpoints on JavaScript Event Listeners

<iframe
  data-src="/chrome-developer-tools/docs/javascript-debugging_70d5a0ab97a89fcb95b8eda768f0524f.frame"
  class="framebox inherit-locale "
  style="width: 100%; height: 500px;">

[This section requires a browser that supports JavaScript and iframes.]

</iframe>

**Note: Following events are supported**
&nbsp;&nbsp;**Keyboard:** keydown, keypress, keyup, textInput

&nbsp;&nbsp;**Mouse:** click, dblclick, mousedown, mouseup, mouseover,
mousemove, mouseout, mousewheel

&nbsp;&nbsp;**Control:** resize, scroll, zoom, focus, blur, select,
change, submit, reset

&nbsp;&nbsp;**Clipboard:** copy, cut, paste, beforecopy, beforecut,
beforepaste

&nbsp;&nbsp;**Load:** load, unload, abort, error

&nbsp;&nbsp;**DOM Mutation:** DOMActivate, DOMFocusIn, DOMFocusOut,
DOMAttrModified, DOMCharacterDataModified, DOMNodeInserted,
DOMNodeInsertedIntoDocument, DOMNodeRemoved, DOMNodeRemovedFromDocument,
DOMSubtreeModified, DOMContentLoaded

&nbsp;&nbsp;**Device:** deviceorientation, devicemotion

[Back to top](#top)

## Live Editing

In **Authoring And Workflow**, we discussed how to make changes to scripts in
the **Sources** panel. While at a breakpoint, it's also possible to live edit
scripts by clicking into the main editor panel and making modifications.

*   Navigate to the Google Closure hovercard demo
*   In the Sources panel, open up "mouse.js" and use the Ctrl/Cmd + Shift + O to
navigate to the onMouseOut() function

<div class="screenshot">![](javascript-debugging/houseMouseOut.jpg)</div>

*   Click the **pause** button to pause debugging
*   Modify the function, adding a console.log('Moused out') to the end
*   Using the Cmd +S or Ctrl + S shortcuts will save these modifications. Make
sure to Save As.
*   Click the **pause/resume** button to resume execution
*   When you now hover out, the new message will be logged to the console

<div class="screenshot">![](javascript-debugging/pause-resume-mouseout.jpg)</div>

This allows you to saved changes from within the DevTools without having to
leave your browser.

[Back to top](#top)

## Exceptions

Let's now look at how to deal with exceptions and stack traces using Chrome
DevTools.

**Exception handling** is the process of responding to the occurrence of
exceptions - exceptional situations that require special processing - often
changing the normal flow of your JavaScript code's execution.

**Note:** If you are a Web Developer and want to get the latest version of
DevTools, you should use [Chrome Canary](tools.google.com/dlpage/chromesxs).

## Tracking exceptions

When something goes wrong, you can open the DevTools console (Ctrl+Shift+J /
Cmd+Option+J) and find a number of JavaScript error messages there. Each message
has a link to the file name with the line number you can navigate to.

<div class="screenshot">![](javascript-debugging/tracking-exceptions.jpg)</div>

[Back to top](#top)

### Viewing exception stack trace

There might be several execution paths that lead to the error and it's not
always obvious which one of them has happened. **Once DevTools window is
opened**, exceptions in the console are accompanied with the **complete
JavaScript call stacks**. You can expand these console messages to see the stack
frames and navigate to the corresponding locations in the code:

<div class="screenshot">![](javascript-debugging/exception-stack-trace.jpg)</div>

[Back to top](#top)

### Pause on JavaScript exceptions

You may also want to pause JavaScript execution next time exception is thrown
and inspect its call stack, scope variables and state of your app. A tri-state
stop button ( ![](javascript-debugging/image_34.png) <!-- TODO: Fix alt text
and URL --> ) at the bottom of the Scripts panel enables you to switch between
different exception handling modes: you can choose to either pause on all
exception or only on the uncaught ones or you can ignore exceptions altogether.

<div class="screenshot">![](javascript-debugging/pause-execution.jpg)</div>

[Back to top](#top)

## Printing stack traces

Printing log messages to the DevTools console may be very helpful in
understanding how your application behaves. You can make the log entries even
more informative by including associated stack traces. There are several ways of
doing that.

[Back to top](#top)

### Error.stack

Each Error object has a string property named stack that contains the stack
trace:

<div class="screenshot">![](javascript-debugging/error-stack.jpg)</div>

[Back to top](#top)

### console.trace()

You can instrument your code with console.trace() calls that would print current
JavaScript call stacks:

<div class="screenshot">![](javascript-debugging/console-trace.jpg)</div>

[Back to top](#top)

### console.assert()

There is also a way to place assertion in your JavaScript code. Just call
console.assert() with the error condition as the first parameter. Whenever this
expression evaluates to false you will see a corresponding console record:

<div class="screenshot">![](javascript-debugging/console-assert.jpg)</div>

[Back to top](#top)

## Handling exceptions at runtime using window.onerror

Chrome supports setting a handler function to window.onerror. Whenever a
JavaScript exception is thrown in the window context and is not caught by any
try/catch block, the function will be invoked with the exception's message, the
URL of the file where the exception was thrown and the line number in that file
passed as three arguments in that order. You may find it convenient to set an
error handler that would collect information about uncaught exceptions and
report it back to your server.

<div class="screenshot">![](javascript-debugging/window-onerror.jpg)</div>

[Back to top](#top)

## Pretty Print

If you have trouble trying to read and debug minified JavaScript in the
DevTools, a pretty printing option is available to make life easier. Here is how
a minified script displayed in the tools might look prior to being displayed in
the DevTools:

<div class="screenshot">![](javascript-debugging/pretty-print-off.jpg)</div>

And by clicking on the curly brace ![](javascript-debugging/image_45.png) ("Pretty Print") icon in the bottom left
corner, the JavaScript is transformed into a more human readable form. This is
also more easy for debugging and setting breakpoints.

<div class="screenshot">![](javascript-debugging/pretty-print-on.jpg)</div>

[Back to top](#top)

## Source Maps

Have you ever found yourself wishing you could keep your client-side code
readable and more importantly debuggable even after you've combined and minified
it, without impacting performance? Well now you can through the magic of [source
maps](https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k/edit?hl=en_US&amp;pli=1&amp;pli=1).

Source Maps are a generic mapping format (that are JSON-based) which can be used by any processed file to create
relations between files that are pre-processed and those that are post-processed. Of most relevance to us is that they can be used to map combined/minified scripts back to an unbuilt state for debugging.

A source map itself can look as simple as the following:

<pre>
  {
    version : 3,
    file: "out.js",
    sourceRoot : "",
    sources: ["foo.js", "bar.js"],
    names: ["src", "maps", "are", "fun"],
    mappings: "AAgBC,SAAQ,CAAEA"
}
</pre>

The idea is when you build for production, along with minifying and combining your
JavaScript files, you generate a source map which holds information about your
original files. When you query a certain line and column number in your
generated JavaScript you can do a lookup in the source map which returns the
original location. The DevTools can parse the source map automatically and make
it appear as though you're running unminified and uncombined files.

<div class="screenshot">![](javascript-debugging/source-mapping.jpg)</div>

Before you view the following real world implementation of Source Maps make sure
you've enabled the Source Maps feature by clicking the settings cog in the dev
tools panel and checking the "Enable source maps" option.

<div class="screenshot">![](javascript-debugging/source-maps.jpg)</div>

Take a look at the special build of the [font dragr tool](http://dev.fontdragr.com) in Chrome,
with source mapping enabled, and you'll notice that the JavaScript isn't
compiled and you can see all the individual JavaScript files it references. This
is using source mapping, but behind the scenes actually running the compiled
code. Any errors, logs and breakpoints will map to the dev code for awesome
debugging! So in effect it gives you the illusion that you're running a dev site
in production.

### How do Source Maps work?

Once you've combined and minified your JavaScript with a tool that supports
outputting Source Maps, alongside it will exist a sourcemap file. At present,
Closure Compiler and UglifyJS 2.0 are two such tools for achieving this but
there are also tools available that support outputting Source Maps for
CoffeeScript and SASS. A special comment is placed at the end of the file, signifying to the DevTools
that a source map is available.

`//# sourceMappingURL=/path/to/file.js.map`

This enables DevTools to map calls back to their location in original source
files. If you don't like the idea of the weird comment you can alternatively set
a special header on your compiled JavaScript file:

`X-SourceMap: /path/to/file.js.map`

Like the comment this will tell your source map consumer where to look for the
source map associated with a JavaScript file. This header also gets around the
issue of referencing Source Maps in languages that don't support single-line
comments.

### @sourceURL and displayName in action

While not part of the source map spec the following convention allows you to
make development much easier when working with evals.

This helper looks very similar to the `//# sourceMappingURL` property and is
actually mentioned in the source map V3 specifications. By including the
following special comment in your code, which will be evaled, you can name evals
so they appear as more logical names in your dev tools.

`//# sourceURL=source.coffee`

**Working with sourceURL**

*   Navigate to the
**[demo](http://www.thecssninja.com/demo/source_mapping/compile.html)**
*   Open the DevTools and go to the **Sources** panel.
*   Enter in a filename into the _Name your code:_ input field.
*   Click on the **compile** button.
*   An alert will appear with the evaluated sum from the CoffeeScript source
*   If you expand the _Sources_ sub-panel you will now see a new file with the
custom filename you entered earlier. If you double-click to view this file it
will contain the compiled JavaScript for our original source. On the last line
however will be a `// @sourceURL` comment indicating what the original source
file was. This can greatly help with debugging when working with language
abstractions.

<div class="screenshot">![](javascript-debugging/coffeescript.jpg)</div>

#### Read more

*   Conditional breakpoints

        *   [Breakpoint actions in
JavaScript](http://www.randomthink.net/blog/2012/11/breakpoint-actions-in-javascript/)

*   Working With Source Maps

        *   [The Breakpoint: Source maps
spectacular](https://www.youtube.com/watch?feature=player_embedded&amp;v=HijZNR6kc9A)
    *   [HTML5 Rocks: An Introduction To JavaScript Source
maps](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)
    *   [NetTuts: Source Maps 101](http://net.tutsplus.com/tutorials/tools-and-tips/source-maps-101/)
    *   [Source maps: languages, tools and other info](https://github.com/ryanseddon/source-map/wiki/Source-maps%3A-languages%2C-tools-and-other-info)
    *   [CSS Ninja: Multi-level Source
maps](http://www.thecssninja.com/javascript/multi-level-sourcemaps)
    *   [Source maps for
CoffeeScript](http://www.coffeescriptlove.com/2012/04/source-maps-for-coffeescript.html)