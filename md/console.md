使用 Console
==

JavaScript Console 为开发者们测试网页和应用提供了两个主要的功能：

* 使用 [Console API](https://developers.google.com/chrome-developer-tools/docs/console-api) 来输出一段调试信息，例如使用 [console.log()](https://developers.google.com/chrome-developer-tools/docs/console-api#consolelogobject_object)，或者 [console.profile()](https://developers.google.com/chrome-developer-tools/docs/console-api#consoleprofilelabel) 。
* 一个实时的 Shell ，让你可以输入命令，并且通过 Chrome DevTools 操作 document。你可以直接在 Console 里面执行表达式，也可以执行 [Command Line API](https://developers.google.com/chrome-developer-tools/docs/commandline-api) 里面提供的一些方法，例如执行 [$()](https://developers.google.com/chrome-developer-tools/docs/commandline-api#selector) 命令来选择所有元素，或者执行 [profile()](https://developers.google.com/chrome-developer-tools/docs/commandline-api#profile) 命令来启动一个 CPU 测评器（profiler）。

这个文档提供了这两个 API 的功能介绍和常用用法。你也可以去查看 [Console API](https://developers.google.com/chrome-developer-tools/docs/console-api) 和 [Command Line API](https://developers.google.com/chrome-developer-tools/docs/commandline-api) 两个入门参考文档。


1.  [基础操作](#basic_operation)
    1.  [打开 Console](#opening_the_console)
    2.  [清空 Console 结果](#clearing_the_console_history)
    3.  [Console 设置](#console_settings)
2.  [使用 Console API](#using_the_console_api)
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
    6.  [Controlling the CPU profiler](#controlling_the_cpu_profiler)

## 基础操作

### 打开 Console

在 Chrome DevTools 中，JavaScript Console 有两种模式：Console 标签或者在其他标签（例如 Elements 或者 Sources 标签）下的一个分裂视图。

可以使用下面几种方法之一打开 Console 标签：

* 使用快捷键 **Command - Option - J** (Mac) 或者 **Control -Shift -J** (Windows/Linux)。
* 选择菜单栏中的 **视图 &gt; 开发者 &gt; JavaScript 控制台**。

![Console panel view](https://developers.google.com/chrome-developer-tools/docs/console-files/console1.png)

可以通过摁下 **Esc** 键快速切换 Console 面板的分裂视图，或者点击 Chrome DevTools 窗口左下角的
**Show/Hide Console** 按钮。下面的截图中就是在 Elements 面板下的 Console 分裂视图。


![Console 分裂视图](https://developer.chrome.com/devtools/docs/console-files/console-split-view.png)

### 清空 console 记录

使用下面任意一种方法就可以清理 console 的历史：

* 在 console 的任意地方右击或者摁下 Ctrl 同时点击即可弹出包含 **Clear Console** 菜单。
* 在 shell 输入行中输入命令行 API [**clear()**](commandline-api#clear) 并回车。
* 调用 JavaScript 的 [**console.clear()**](console-api#consoleclear) 这个 Console API。
* 使用快捷键 **⌘K** 或者 **⌃L** (Mac) **Control - L** (Windows and Linux).

默认的情况下，当你打开其他页面的时候，console 的记录就会被清理。你可以在 Settings 对话框里面的 Console 设置区域中启用 **Preserve log upon navigation** 选项来避免清理。（详情 [Console preferences](#consolepreferences) ）。


### Console 设置

你可以在 DevTools Settings 对话框中的 General 标签下修改 Console 的两个全局选项：


* **Log XMLHTTPRequests**&mdash;决定每个 XMLHTTPRequest 是否都显示在 Console 面板上。
* **Preserve log upon navigation**&mdash;决定你在当前页面中 console 的记录是否会因为你跳转到其他页面而被清空。这两个选项默认都是禁用的。

你也可以在 console 的任意地方右击，通过选择出现的菜单来改变这两个选项。


![Console 面板右击菜单视图](https://developer.chrome.com/devtools/docs/console-files/console-context-menu.png)

## 使用 Console API

Console API 是 DevTools 定义得全局对象 console 的方法集合。API 的主要目的是在你应用运行的时候[显示信息](#writing_to_the_console)（例如显示一个属性值，或者一整个对象或者 DOM 对象）到 console 上。为了避免 console 中的视觉混乱，你也可以成组的输出信息。

### 输出信息到 console

[console.log()](console-api#consolelogobject_object) 方法可以把一个或者多个语句当作参数传递进去然后把它们当前的值输出到 console。例如：

    var a = document.createElement('p');
    a.appendChild(document.createTextNode('foo'));
    a.appendChild(document.createTextNode('bar'));
    console.log("Node count: " + a.childNodes.length);

![Console 信息输出](https://developer.chrome.com/devtools/docs/console-files/log-basic.png)

如果不想使用 “＋” 这个表达式连接符（像上图那样），you can put each in its own method parameter and they will be joined together in a space-delimited line.

	console.log("Node count:", a.childNodes.length, "and the current time is:", Date.now());


![Console 信息输出](https://developer.chrome.com/devtools/docs/console-files/log-multiple.png)

### 错误和警告

[console.error()](console-api#consoleerrorobject_object) 方法会显示一个红色图标和一段红色信息文本。

    function connectToServer() {
        console.error("Error: %s (%i)", "Server is  not responding",500);
    }
    connectToServer();
    
![console.error() 的例子](https://developer.chrome.com/devtools/docs/console-files/error-server-not-resp.png)

[console.warn()](console-api#consolewarnobject_object) 方法会显示一个黄色图标和一段信息文本。

    if(a.childNodes.length &lt; 3 ) {
        console.warn('Warning! Too few nodes (%d)', a.childNodes.length);
    }
    
![console.warn() 的例子](https://developer.chrome.com/devtools/docs/console-files/warning-too-few-nodes.png)

### 断言（Assertions）

[console.assert()](console-api#consoleassertexpression_object) 方法会根据条件判断，只有在它第一个参数得结果为 false 得时候会显示一段错误信息（第二个参数是错误信息）。举个例子，下面得这个例子就是只有当 `list` 元素得子结点数目大于 500 得时候输出这段错误信息。


    console.assert(list.childNodes.length &lt; 500, "Node count is &gt; 500");

![console.assert() 的例子](https://developer.chrome.com/devtools/docs/console-files/assert-failed.png)

### Filtering console output

You can quickly filter console output by its severity level--errors, warning, or standard log statements--by selecting one of the filter options. Click on the filter funnel (as shown below) and then select which filter you want to use.

![Only show console.error() output](https://developers.google.com/chrome-developer-tools/docs/console-files/filter-errors.png)

Filter options:

*   **All**&mdash;Shows all console output.
*   **Errors**&mdash;Only show output from `console.error()`
*   **Warnings**&mdash;Only show output from `console.warn()`
*   **Logs**&mdash;Only show output from `console.log()`, `console.info()` and `console.debug()`.
*   **Debug**&mdash;Only show output from `console.timeEnd()` and other console output.

### Grouping output

You can visually group related console output statements together in the console with the [`console.group()`](console-api#consolegroupobject_object) and [`groupEnd()`](console-api#consolegroupend) commands.

    var user = "jsmith", authenticated = false;
    console.group("Authentication phase");
    console.log("Authenticating user '%s'", user);
    // authentication code here...
    if (!authenticated) {
        console.log("User '%s' not authenticated.", user)
    }
    console.groupEnd();
    

![Logging group example](https://developers.google.com/chrome-developer-tools/docs/console-files/group.png)

You can also nest logging groups. In the following example a logging group is created for the authentication phase of a login process. If the user is authenticated, a nested group is created for the authorization phase.

    var user = "jsmith", authenticated = true, authorized = true;
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

![Nested logging group example](https://developers.google.com/chrome-developer-tools/docs/console-files/nestedgroup.png)

To create a group that is initially collapsed, use [`console.groupCollapsed()`](console-api#consolegroupcollapsed) instead of `console.group()`, as shown below:

    console.groupCollapsed("Authenticating user '%s'", user);
    if (authenticated) {
      ...
    }
    

![Initially collapsed group](https://developers.google.com/chrome-developer-tools/docs/console-files/groupcollapsed.png)

### String substitution and formatting

The first parameter you pass to any of the console's logging methods (`log()` or `error()`, for example) may contain one or more _format specifiers_. A format specifier consists of a **`%`** symbol followed by a letter that indicates the formatting that should be applied to the inserted value (`%s` for strings, for example). The format specifier identifies where to substitute a value provided by a subsequent parameter value.

The following example using the `%s` (string) and `%d` (integer) formatters to insert values into the output string.

    console.log("%s has %d points", "Sam", "100");

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

    console.log("Node count: %d, and the time is %f.", document.childNodes.length, Date.now());
    
![Using format specifiers](https://developers.google.com/chrome-developer-tools/docs/console-files/format-substitution.png)

### Formatting DOM elements as JavaScript objects


By default, when you log a DOM element to the console it's displayed in an XML format, as in the Elements panel:

    console.log(document.body.firstElementChild)

![](https://developers.google.com/chrome-developer-tools/docs/console-files/log-element.png)

You can also log an element's JavaScript representation with the `console.dir()` method:

	console.dir(document.body.firstElementChild);

![](https://developers.google.com/chrome-developer-tools/docs/console-files/dir-element.png)

Equivalently, you can us the `%O` [format specifier](#string_substitution_and_formatting) with `console.log()`:

    console.log("%O", document.body.firstElementChild);


### Styling console output with CSS

You use the `%c` format specifier to apply custom CSS rules to any string you write to the Console with[`console.log()`](#writingtotheconsole) or related methods.

	console.log("%cThis will be formatted with large, blue text", "color: blue; font-size: x-large");
   
![Styling console output with CSS](https://developers.google.com/chrome-developer-tools/docs/console-files/format-string.png)

### Measuring how long something takes

You can use the [`console.time()`](console-api#consoletimelabel) and [`console.timeEnd()`](console-api#consoletimeendlabel) methods to measure how long a function or operation in your code takes to complete. You call `console.time()` at the point in your code where you want to start the timer and `console.timeEnd()` to stop the timer. The elapsed time between these two calls is displayed in the console.

    console.time("Array initialize");
    var array= new Array(1000000);
    for (var i = array.length - 1; i &gt;= 0; i--) {
        array[i] = new Object();
    };
    console.timeEnd("Array initialize");


![Example of using console.time() and timeEnd()](console-files/time-duration.png)

>**Note:** You must pass the same string to `console.time()` and `timeEnd()` for the timer to finish as expected.

### Marking the Timeline

The [Timeline panel](timeline) gives you a complete overview of where time is spent when loading and using your web app or page. The [`console.timeStamp()`](console-api#consoletimestamplabel) method marks the Timeline at the moment it was executed. This provides an easy way to correlate events in your application with other browser-related events, such as layout or paints.

>**Note:** The `console.timeStamp()` method only functions while a Timeline recording is in progress.

In the following example the Timeline is marked when the application enters the `AddResult()` function's implementation.

    function AddResult(name, result) {
      console.timeStamp("Adding result");
      var text = name + ': ' + result;
      var results = document.getElementById("results");
      results.innerHTML += (text + "&lt;br&gt;");
    }

As shown in the following screenshot, the `timeStamp()` command annotates the Timeline in the following places:

*   A yellow vertical line in the Timeline's summary and detail views.
*   A record is added to the list of recorded events.

    ![Timeline showing custom timestamp](https://developers.google.com/chrome-developer-tools/docs/console-files/timestamp2.png)

### Setting breakpoints in JavaScript

You can start a debugging session from your JavaScript code by calling the [`debugger`](console-api#debugger) command. For instance, in the following example the JavaScript debugger is opened when an object's `brightness()` function is invoked:

    brightness : function() {
        debugger;
        var r = Math.floor(this.red*255);
        var g = Math.floor(this.green*255);
        var b = Math.floor(this.blue*255);
        return (r * 77 + g * 150 + b * 29) &gt;&gt; 8;
    }

![Example of using debugger command](https://developers.google.com/chrome-developer-tools/docs/console-files/debugger.png)


>An interesting technique of using conditional breakpoints was explored by Brian Arnold in [Breakpoint Actions in JavaScript](http://www.randomthink.net/blog/2012/11/breakpoint-actions-in-javascript/).


## Using the Command Line API

In addition to being a place where you can log information from your application, the Console is also a shell prompt where you can directly evaluate expressions or issue commands provided by the [Command Line API](commandline-api). This API provides the following features:

*   Convenience functions for selecting DOM elements
*   Methods for controlling the CPU profiler
*   Aliases for a number of Console API methods
*   Monitoring events
*   View event listeners registered on objects
### Evaluating expressions

The Console attempts to evaluate any JavaScript expression you enter at the shell prompt, upon pressing the Return or Enter key. The Console provides auto-completion and tab-completion. As you type expressions, property names are automatically suggested. If there are multiple properties with the same prefix, pressing the Tab key cycles through them. Pressing the right arrow key accepts the current suggestion. The current suggestion is also accepted by pressing the Tab key if there is only one matched property.

![](https://developers.google.com/chrome-developer-tools/docs/console-files/evaluate-expressions.png)

To enter a multi-line expression at the shell prompt (such as a function definition) press Shift+Enter between lines.

![](https://developers.google.com/chrome-developer-tools/docs/console-files/multiline-expression.png)

### Selecting elements

The Command Line API provides several methods to access DOM elements in your application. For example, the [**`$()`**](commandline-api#selector) method returns the first element that matches the specified CSS selector, just like [`document.querySelector()`](http://docs.webplatform.org/wiki/css/selectors_api/querySelector). For instance, the following code returns the element with the ID "loginBtn".

    $('#loginBtn');

![](https://developers.google.com/chrome-developer-tools/docs/console-files/select-login-btn.png)

The [**`$$()`**](commandline-api#selector_1) command returns an array of all the elements that match the specified CSS selector, just like [`document.querySelectorAll()`](http://docs.webplatform.org/wiki/css/selectors_api/querySelectorAll). For instance, the following displays selects all `&lt;button&gt;` elements with the CSS class "loginBtn":

    $$('button.loginBtn');

![](https://developers.google.com/chrome-developer-tools/docs/console-files/select-multiple-login.png)

Lastly, the [`x()`](commandline-api#xpath) method takes an XPath path as a parameter and returns an array of all elements that match the specified path. The following returns all the &lt;script&gt; elements that are children of the `&lt;body&gt;` tag:

    $x('/html/body/script');

### Inspecting DOM elements and JavaScript heap objects

The [`inspect()`](commandline-api#inspectobject) method takes a DOM element reference (or JavaScript reference) as a parameter and displays the element or object in the appropriate panel&mdash;the Elements panel for DOM elements, or the Profile panel for a JavaScript object.

For example, in the following screenshot the `$()` function is used to get a reference to an `&lt;li&gt;` element. Then the last evaluated expression property ([`$_`](commandline-api#_)) is passed to `inspect()` to open that element in the Elements panel.

![](https://developers.google.com/chrome-developer-tools/docs/console-files/inspect2.png)

### Accessing recently selected elements and objects

Often when testing you'll select DOM elements&mdash;either directly in the Elements panel or using the Selection tool (magnifying glass)&mdash;so that you can further inspect the element. Or, when analyzing a memory snapshot in the Profiles panel, you might select a JavaScript object to further inspect it.

The Console remembers the last five elements (or heap objects) you've selected and makes them available as properties named [**`$0`**, **`$1`**, **`$2`**, **`$3`**](commandline-api#0_-_4) and [**`$4`**](commandline-api#0_-_4). The most recently selected element or object is available as **`$0`**, the second most as **`$1`**, and so forth.

The following screenshot shows the values of these properties after selecting three different elements in turn from the Elements panel:

![Recently selected elements](https://developers.google.com/chrome-developer-tools/docs/console-files/recent-selection.png)

>**Note:** You can also Right-click or Control-click on any element in the Console and select **Reveal in Elements Panel**.

### Monitoring events

The [`monitorEvents()`](commandline-api#monitoreventsobject_events) command monitors an object for one or more specified events. When an event occurs on the monitored object, the corresponding Event object is logged to the Console. You specify the object and the events you want to monitor on that object. For example, the following code enables event monitoring for every "resize" event on the global window object.

    monitorEvents(window, "resize");

![Monitoring window resize events](https://developers.google.com/chrome-developer-tools/docs/console-files/monitor-resize.png)

To monitor several events, you can pass an array of event names as the second parameter. The code below monitors both "mousedown" and "mouseup" events on the body of the document.

    monitorEvents(document.body, ["mousedown", "mouseup"]);

You can also pass one of the supported "event types" that DevTools maps to a set of actual event names. For example, the "touch" event type cause DevTools to monitor "touchstart", "touchend", "touchmove", and "touchcancel" events on the target object.

    monitorEvents($('#scrollBar'), "touch");


See [`monitorEvents()`](commandline-api#monitoreventsobject_events) in the Console API Reference for a list of supported event types.

To stop monitoring events call `unmonitorEvents()`, passing the object to stop monitoring.

    unmonitorEvents(window);

### Controlling the CPU profiler

You can create JavaScript CPU profiles from the command line with the [`profile()`](commandline-api#profilename) and [`profileEnd()`](commandline-api#profileendname) commands. You can optionally specify a name that's applied to the profile you create.

For example, the following shows an example of creating a new profile with the default name:

![](https://developers.google.com/chrome-developer-tools/docs/commandline-api-files/profile-console.png)

The new profile appears in the Profiles panel under the name "Profile 1":

![](https://developers.google.com/chrome-developer-tools/docs/commandline-api-files/profile-panel.png)

If you specify a label for the new profile, it is used as the new profile's heading. If you create multiple profiles with the same name, they are grouped as individual runs under the same heading:

![](https://developers.google.com/chrome-developer-tools/docs/commandline-api-files/profile-console-2.png)

The result in the Profiles panel:

![](https://developers.google.com/chrome-developer-tools/docs/commandline-api-files/profile-panel-2.png)

CPU profiles can be nested, for example:

    profile("A");
    profile("B");
    profileEnd("B")
    profileEnd("A")


The calls to stop and start profiling do not need be properly nested. For example, the following works the same as the previous example:

    profile("A");
    profile("B");
    profileEnd("A");
    profileEnd("B");

Except as otherwise noted, the content of this page is licensed under the [Creative Commons Attribution 3.0 License](http://creativecommons.org/licenses/by/3.0/), and code samples are licensed under the [Apache 2.0 License](http://www.apache.org/licenses/LICENSE-2.0). For details, see our [Site Policies](https://developers.google.com/site-policies).

Last updated February 12, 2014.