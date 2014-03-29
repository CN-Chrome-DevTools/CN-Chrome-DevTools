Using the Console
==

The JavaScript Console provides two primary functions for developers  testing web pages and applications:

*   A place to log diagnostic information using methods provided by the [Console API](console-api), such as [console.log()](console-api#consolelogobject_object), or [console.profile()](console-api#consoleprofilelabel).
*   A shell prompt where you can enter commands and interact with the document and the Chrome DevTools. You can evaluate expressions directly in the Console, and can also use the methods provided by the [Command Line API](commandline-api), such as [$()](commandline-api#selector) command for selecting elements, or [profile()](commandline-api#profile) to start the CPU profiler.

This documentation provides an overview and common uses of these two APIs. You can also browse the [Console API](console-api) and [Command Line API](commandline-api) reference guides.

<nav class="inline-toc">

1.  [Basic operation](#basic_operation)

        1.  [Opening the Console](#opening_the_console)
    2.  [Clearing the console history](#clearing_the_console_history)
    3.  [Console settings](#console_settings)
2.  [Using the Console API](#using_the_console_api)

        1.  [Writing to the console](#writing_to_the_console)
    2.  [Errors and warnings](#errors_and_warnings)
    3.  [Assertions](#assertions)
    4.  [Filtering console output](#filtering_console_output)
    5.  [Grouping output](#grouping_output)
    6.  [String substitution and formatting](#string_substitution_and_formatting)
    7.  [Formatting DOM elements as JavaScript objects](#formatting_dom_elements_as_javascript_objects)
    8.  [Styling console output with CSS](#styling_console_output_with_css)
    9.  [Measuring how long something takes](#measuring_how_long_something_takes)
    10.  [Marking the Timeline](#marking_the_timeline)
    11.  [Setting breakpoints in JavaScript](#setting_breakpoints_in_javascript)
3.  [Using the Command Line API](#using_the_command_line_api)

        1.  [Evaluating expressions](#evaluating_expressions)
    2.  [Selecting elements](#selecting_elements)
    3.  [Inspecting DOM elements and JavaScript heap objects](#inspecting_dom_elements_and_javascript_heap_objects)
    4.  [Accessing recently selected elements and objects](#accessing_recently_selected_elements_and_objects)
    5.  [Monitoring events](#monitoring_events)
    6.  [Controlling the CPU profiler](#controlling_the_cpu_profiler)</nav>

## Basic operation

### Opening the Console

The JavaScript Console is available in two modes within Chrome DevTools: the primary Console tab, or as a split-view you can display while on another tab (such as Elements or Sources).

To open the Console tab, do one of the following:

*   Use the keyboard shortcut **Command - Option - J** (Mac) or **Control -Shift -J** (Windows/Linux).
*   Select **View &gt; Developer &gt; JavaScript Console**.

![Console panel view](console-files/console1.png)

To toggle a split-view of the Console on another tab, press the **Esc** key on your keyboard, or click the **Show/Hide Console** button in the bottom left corner of the Chrome DevTools window. In the following screenshot the Console split-view is shown with the Elements panel.

![Console split-view](console-files/console-split-view.png)

### Clearing the console history

To clear the console's history, do one of the following:

*   Right-click or Ctrl-click anywhere in the Console and choose **Clear Console** from the context menu that appears.
*   Enter the [**clear()**](commandline-api#clear) Command Line API at the shell prompt.
*   Invoke [**console.clear()**](console-api#consoleclear) Console API from JavaScript.
*   Use the keyboard shortcut **⌘K** or **⌃L** (Mac) **Control - L** (Windows and Linux).

By default, the console history is cleared when you navigate to another page. You can change this behavior by enabling **Preserve log upon navigation** in the Console area of the Settings dialog (see [Console preferences](#consolepreferences)).

### Console settings

The Console has two global settings you can modify in the General tab of the DevTools Settings dialog:

*   **Log XMLHTTPRequests**&mdash;determines if each XMLHTTPRequest is logged to the Console panel.
*   **Preserve log upon navigation**&mdash;determines if console history for the current page is preserved when you navigate to another page. By default, both of these settings are disabled.

You can also change these settings by right-clicking anywhere in the Console to bring up the context menu.

![Console panel view](console-files/console-context-menu.png)

## Using the Console API

The Console API is collection of methods provided by the global `console` object defined by DevTools. One of the API's main purposes is to [log information](#writing_to_the_console) (such as a property value, or an entire objects or DOM element) to the console while your application is running. You can also group output visually in the console to reduce visual clutter.

### Writing to the console

The [console.log()](console-api#consolelogobject_object) method takes one or more expressions as parameters and writes their current values to the console. For example:

    var a = document.createElement('p');
    a.appendChild(document.createTextNode('foo'));
    a.appendChild(document.createTextNode('bar'));
    console.log("Node count: " + a.childNodes.length);
    `</pre>

    ![Console log output](console-files/log-basic.png)

    Instead of concatenating expressions together with the "+" operator (as shown above), you can put each in its own method parameter and they will be joined together in a space-delimited line.

    <pre class="prettyprint notranslate" translate="no">`console.log("Node count:", a.childNodes.length, "and the current time is:", Date.now());
    `</pre>

    ![Console log output](console-files/log-multiple.png)

### Errors and warnings

    The [`console.error()`](console-api#consoleerrorobject_object) method displays a red icon along with the message text, which is colored red.

    `function connectToServer() {
        console.error("Error: %s (%i)", "Server is  not responding",500);
    }
    connectToServer();
    `

    ![](console-files/error-server-not-resp.png)

    The [`console.warn()`](console-api#consolewarnobject_object) method displays a yellow warning icon with the message text.

    <pre class="prettyprint notranslate" translate="no">`if(a.childNodes.length &lt; 3 ) {
        console.warn('Warning! Too few nodes (%d)', a.childNodes.length);
    }
    `</pre>

    ![Example of console.warn()](console-files/warning-too-few-nodes.png)

    ### Assertions

    The [`console.assert()`](console-api#consoleassertexpression_object) method conditionally displays an error string (its second parameter) only if its first parameter evaluates to `false`. For instance, in the following example an error message is written to the console only if the number of child nodes belonging to the `list` element is greater than 500.

    <pre class="prettyprint notranslate" translate="no">`console.assert(list.childNodes.length &lt; 500, "Node count is &gt; 500");
    `</pre>

    ![Example of console.assert()](console-files/assert-failed.png)

    ### Filtering console output

    You can quickly filter console output by its severity level--errors, warning, or standard log statements--by selecting one of the filter options. Click on the filter funnel (as shown below) and then select which filter you want to use.

    ![Only show console.error() output](console-files/filter-errors.png)

    Filter options:

*   **All**&mdash;Shows all console output.
*   **Errors**&mdash;Only show output from `console.error()`
*   **Warnings**&mdash;Only show output from `console.warn()`
*   **Logs**&mdash;Only show output from `console.log()`, `console.info()` and `console.debug()`.
*   **Debug**&mdash;Only show output from `console.timeEnd()` and other console output.

    ### Grouping output

    You can visually group related console output statements together in the console with the [`console.group()`](console-api#consolegroupobject_object) and [`groupEnd()`](console-api#consolegroupend) commands.

    <pre class="prettyprint notranslate" translate="no">`var user = "jsmith", authenticated = false;
    console.group("Authentication phase");
    console.log("Authenticating user '%s'", user);
    // authentication code here...
    if (!authenticated) {
        console.log("User '%s' not authenticated.", user)
    }
    console.groupEnd();
    `</pre>

    ![Logging group example](console-files/group.png)

    You can also nest logging groups. In the following example a logging group is created for the authentication phase of a login process. If the user is authenticated, a nested group is created for the authorization phase.

    <pre class="prettyprint notranslate" translate="no">`var user = "jsmith", authenticated = true, authorized = true;
    // Top-level group
    console.group("Authenticating user '%s'", user);
    if (authenticated) {
        console.log("User '%s' was authenticated", user);
        // Start nested group
        console.group("Authorizing user '%s'", user);
        if (authorized) {
            console.log("User '%s' was authorized.", user);
        }
        // End nested group
        console.groupEnd();
    }
    // End top-level group
    console.groupEnd();
    console.log("A group-less log trace.");
    `</pre>

    ![Nested logging group example](console-files/nestedgroup.png)

    To create a group that is initially collapsed, use [`console.groupCollapsed()`](console-api#consolegroupcollapsed) instead of `console.group()`, as shown below:

    <pre class="prettyprint notranslate" translate="no">`console.groupCollapsed("Authenticating user '%s'", user);
    if (authenticated) {
      ...
    }
    `</pre>

    ![Initially collapsed group](console-files/groupcollapsed.png)

    ### String substitution and formatting

    The first parameter you pass to any of the console's logging methods (`log()` or `error()`, for example) may contain one or more _format specifiers_. A format specifier consists of a **`%`** symbol followed by a letter that indicates the formatting that should be applied to the inserted value (`%s` for strings, for example). The format specifier identifies where to substitute a value provided by a subsequent parameter value.

    The following example using the `%s` (string) and `%d` (integer) formatters to insert values into the output string.

    <pre class="prettyprint notranslate" translate="no">`console.log("%s has %d points", "Sam", "100");
    `</pre>

    This would result in "Sam has 100 points" being logged to the console.

    The following table lists the supported format specifiers and the formatting they apply:

    <table>
    <thead>
    <tr>
    <th>Format specifier</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td>`%s`</td>
    <td>Formats the value as a string.</td>
    </tr>
    <tr>
    <td>`%d` or `%i`</td>
    <td>Formats the value as an integer.</td>
    </tr>
    <tr>
    <td>`%f`</td>
    <td>Formats the object as a floating point value.</td>
    </tr>
    <tr>
    <td>`%o`</td>
    <td>Formats the value as an expandable DOM element (as in the Elements panel).</td>
    </tr>
    <tr>
    <td>`%O`</td>
    <td>Formats the value as an expandable JavaScript object.</td>
    </tr>
    <tr>
    <td>`%c`</td>
    <td>Applies CSS style rules to output string specified by the second parameter.</td>
    </tr>
    </tbody>
    </table>

    In the following example the `%d` format specifier is substituted with the value of `document.childNodes.length` and formatted as an integer; the `%f` format specifier is substituted with the value returned by `Date.now()`, which is formatted as a floating point number.

    <pre class="prettyprint notranslate" translate="no">`console.log("Node count: %d, and the time is %f.", document.childNodes.length, Date.now());
    `</pre>

    ![Using format specifiers](console-files/format-substitution.png)

    ### Formatting DOM elements as JavaScript objects

    By default, when you log a DOM element to the console it's displayed in an XML format, as in the Elements panel:

    <pre class="prettyprint notranslate" translate="no">`console.log(document.body.firstElementChild)
    `</pre>

    ![](console-files/log-element.png)

    You can also log an element's JavaScript representation with the `console.dir()` method:

    <pre class="prettyprint notranslate" translate="no">`console.dir(document.body.firstElementChild);
    `</pre>

    ![](console-files/dir-element.png)

    Equivalently, you can us the `%O` [format specifier](#string_substitution_and_formatting) with `console.log()`:

    <pre class="prettyprint notranslate" translate="no">`console.log("%O", document.body.firstElementChild);
    `</pre>

    ### Styling console output with CSS

    You use the `%c` format specifier to apply custom CSS rules to any string you write to the Console with [`console.log()`](#writingtotheconsole) or related methods.

    <pre class="prettyprint notranslate" translate="no">`console.log("%cThis will be formatted with large, blue text", "color: blue; font-size: x-large");
    `</pre>

    ![Styling console output with CSS](console-files/format-string.png)

    ### Measuring how long something takes

    You can use the [`console.time()`](console-api#consoletimelabel) and [`console.timeEnd()`](console-api#consoletimeendlabel) methods to measure how long a function or operation in your code takes to complete. You call `console.time()` at the point in your code where you want to start the timer and `console.timeEnd()` to stop the timer. The elapsed time between these two calls is displayed in the console.

    <pre class="prettyprint notranslate" translate="no">`console.time("Array initialize");
    var array= new Array(1000000);
    for (var i = array.length - 1; i &gt;= 0; i--) {
        array[i] = new Object();
    };
    console.timeEnd("Array initialize");
    `</pre>

    ![Example of using console.time() and timeEnd()](console-files/time-duration.png)

    <aside class="note">**Note:**<span> You must pass the same string to `console.time()` and `timeEnd()` for the timer to finish as expected.</span></aside>

    ### Marking the Timeline

    The [Timeline panel](timeline) gives you a complete overview of where time is spent when loading and using your web app or page. The [`console.timeStamp()`](console-api#consoletimestamplabel) method marks the Timeline at the moment it was executed. This provides an easy way to correlate events in your application with other browser-related events, such as layout or paints.

    <aside class="note">**Note:**<span> The `console.timeStamp()` method only functions while a Timeline recording is in progress.</span></aside>

    In the following example the Timeline is marked when the application enters the `AddResult()` function's implementation.

    <pre class="prettyprint notranslate" translate="no">`function AddResult(name, result) {
      console.timeStamp("Adding result");
      var text = name + ': ' + result;
      var results = document.getElementById("results");
      results.innerHTML += (text + "&lt;br&gt;");
    }
    `</pre>

    As shown in the following screenshot, the `timeStamp()` command annotates the Timeline in the following places:

*   A yellow vertical line in the Timeline's summary and detail views.
*   A record is added to the list of recorded events.

    ![Timeline showing custom timestamp](console-files/timestamp2.png)

    ### Setting breakpoints in JavaScript

    You can start a debugging session from your JavaScript code by calling the [`debugger`](console-api#debugger) command. For instance, in the following example the JavaScript debugger is opened when an object's `brightness()` function is invoked:

    <pre class="prettyprint notranslate" translate="no">`brightness : function() {
        debugger;
        var r = Math.floor(this.red*255);
        var g = Math.floor(this.green*255);
        var b = Math.floor(this.blue*255);
        return (r * 77 + g * 150 + b * 29) &gt;&gt; 8;
    }
    `</pre>

    ![Example of using debugger command](console-files/debugger.png)

    <aside class="special">
    An interesting technique of using conditional breakpoints was explored by Brian Arnold in [Breakpoint Actions in JavaScript](http://www.randomthink.net/blog/2012/11/breakpoint-actions-in-javascript/).
    </aside>

    ## Using the Command Line API

    In addition to being a place where you can log information from your application, the Console is also a shell prompt where you can directly evaluate expressions or issue commands provided by the [Command Line API](commandline-api). This API provides the following features:

*   Convenience functions for selecting DOM elements
*   Methods for controlling the CPU profiler
*   Aliases for a number of Console API methods
*   Monitoring events
*   View event listeners registered on objects

    ### Evaluating expressions

    The Console attempts to evaluate any JavaScript expression you enter at the shell prompt, upon pressing the Return or Enter key. The Console provides auto-completion and tab-completion. As you type expressions, property names are automatically suggested. If there are multiple properties with the same prefix, pressing the Tab key cycles through them. Pressing the right arrow key accepts the current suggestion. The current suggestion is also accepted by pressing the Tab key if there is only one matched property.

    ![](console-files/evaluate-expressions.png)

    To enter a multi-line expression at the shell prompt (such as a function definition) press Shift+Enter between lines.

    ![](console-files/multiline-expression.png)

    ### Selecting elements

    The Command Line API provides several methods to access DOM elements in your application. For example, the [**`$()`**](commandline-api#selector) method returns the first element that matches the specified CSS selector, just like [`document.querySelector()`](http://docs.webplatform.org/wiki/css/selectors_api/querySelector). For instance, the following code returns the element with the ID "loginBtn".

    <pre class="prettyprint notranslate" translate="no">`$('#loginBtn');
    `</pre>

    ![](console-files/select-login-btn.png)

    The [**`$$()`**](commandline-api#selector_1) command returns an array of all the elements that match the specified CSS selector, just like [`document.querySelectorAll()`](http://docs.webplatform.org/wiki/css/selectors_api/querySelectorAll). For instance, the following displays selects all `&lt;button&gt;` elements with the CSS class "loginBtn":

    <pre class="prettyprint notranslate" translate="no">`$$('button.loginBtn');
    `</pre>

    ![](console-files/select-multiple-login.png)

    Lastly, the [`x()`](commandline-api#xpath) method takes an XPath path as a parameter and returns an array of all elements that match the specified path. The following returns all the &lt;script&gt; elements that are children of the `&lt;body&gt;` tag:

    <pre class="prettyprint notranslate" translate="no">`$x('/html/body/script');
    `</pre>

    ### Inspecting DOM elements and JavaScript heap objects

    The [`inspect()`](commandline-api#inspectobject) method takes a DOM element reference (or JavaScript reference) as a parameter and displays the element or object in the appropriate panel&mdash;the Elements panel for DOM elements, or the Profile panel for a JavaScript object.

    For example, in the following screenshot the `$()` function is used to get a reference to an `&lt;li&gt;` element. Then the last evaluated expression property ([`$_`](commandline-api#_)) is passed to `inspect()` to open that element in the Elements panel.

    ![](console-files/inspect2.png)

    ### Accessing recently selected elements and objects

    Often when testing you'll select DOM elements&mdash;either directly in the Elements panel or using the Selection tool (magnifying glass)&mdash;so that you can further inspect the element. Or, when analyzing a memory snapshot in the Profiles panel, you might select a JavaScript object to further inspect it.

    The Console remembers the last five elements (or heap objects) you've selected and makes them available as properties named [**`$0`**, **`$1`**, **`$2`**, **`$3`**](commandline-api#0_-_4) and [**`$4`**](commandline-api#0_-_4). The most recently selected element or object is available as **`$0`**, the second most as **`$1`**, and so forth.

    The following screenshot shows the values of these properties after selecting three different elements in turn from the Elements panel:

    ![Recently selected elements](console-files/recent-selection.png)

    <aside class="note">**Note:**<span> You can also Right-click or Control-click on any element in the Console and select **Reveal in Elements Panel**.</span></aside>

    ### Monitoring events

    The [`monitorEvents()`](commandline-api#monitoreventsobject_events) command monitors an object for one or more specified events. When an event occurs on the monitored object, the corresponding Event object is logged to the Console. You specify the object and the events you want to monitor on that object. For example, the following code enables event monitoring for every "resize" event on the global window object.

    <pre class="prettyprint notranslate" translate="no">`monitorEvents(window, "resize");
    `</pre>

    ![Monitoring window resize events](console-files/monitor-resize.png)

    To monitor several events, you can pass an array of event names as the second parameter. The code below monitors both "mousedown" and "mouseup" events on the body of the document.

    <pre class="prettyprint notranslate" translate="no">`monitorEvents(document.body, ["mousedown", "mouseup"]);
    `</pre>

    You can also pass one of the supported "event types" that DevTools maps to a set of actual event names. For example, the "touch" event type cause DevTools to monitor "touchstart", "touchend", "touchmove", and "touchcancel" events on the target object.

    <pre class="prettyprint notranslate" translate="no">`monitorEvents($('#scrollBar'), "touch");
    `</pre>

    See [`monitorEvents()`](commandline-api#monitoreventsobject_events) in the Console API Reference for a list of supported event types.

    To stop monitoring events call `unmonitorEvents()`, passing the object to stop monitoring.

    <pre class="prettyprint notranslate" translate="no">`unmonitorEvents(window);
    `</pre>

### Controlling the CPU profiler

    You can create JavaScript CPU profiles from the command line with the [`profile()`](commandline-api#profilename) and [`profileEnd()`](commandline-api#profileendname) commands. You can optionally specify a name that's applied to the profile you create.

    For example, the following shows an example of creating a new profile with the default name:

    ![](commandline-api-files/profile-console.png)

    The new profile appears in the Profiles panel under the name "Profile 1":

    ![](commandline-api-files/profile-panel.png)

    If you specify a label for the new profile, it is used as the new profile's heading. If you create multiple profiles with the same name, they are grouped as individual runs under the same heading:

    ![](commandline-api-files/profile-console-2.png)

    The result in the Profiles panel:

    ![](commandline-api-files/profile-panel-2.png)

    CPU profiles can be nested, for example:

    <pre class="prettyprint notranslate" translate="no">`profile("A");
    profile("B");
    profileEnd("B")
    profileEnd("A")
    `</pre>

    The calls to stop and start profiling do not need be properly nested. For example, the following works the same as the previous example:

    <pre class="prettyprint notranslate" translate="no">`profile("A");
    profile("B");
    profileEnd("A");
    profileEnd("B");
    