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

### 筛选 console 输出信息

你可以通过选中筛选选项来快速筛选 console 输出的信息级别，例如错误、警告或者正常的输出信息。点击筛选漏斗图标（如下图）然后就可以选择你需要的筛选选项。


![仅仅显示 console.warn() 输出信息](https://developer.chrome.com/devtools/docs/console-files/filter-errors.png)

筛选选项：

*   **All**&mdash;显示所有输出在 console 的信息
*   **Errors**&mdash;只显示 `console.error()` 输出的信息
*   **Warnings**&mdash;只显示 `console.warn()` 输出的信息
*   **Logs**&mdash;只显示 `console.log()`， `console.info()` 以及 `console.debug()` 输出的信息
*   **Debug**&mdash;只显示 `console.timeEnd()`  and other console output.

### 分组输出

你也可以使用 [`console.group()`](console-api#consolegroupobject_object) 和 [`groupEnd()`](console-api#consolegroupend) 命令来把想要输出到 console 的语句放在一起形成一组。


    var user = "jsmith", authenticated = false;
    console.group("Authentication phase");
    console.log("Authenticating user '%s'", user);
    // authentication code here...
    if (!authenticated) {
        console.log("User '%s' not authenticated.", user)
    }
    console.groupEnd();
    

![分组输出示例](https://developer.chrome.com/devtools/docs/console-files/group.png)

你也可以输出嵌套分组。下面的例子是一个登录过程中的验证阶段演示。如果用户验证通过，就会在验证阶段里面输出一个嵌套分组信息。


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

![输出嵌套分组信息示例](https://developer.chrome.com/devtools/docs/console-files/nestedgroup.png)

你也可以使用 [`console.groupCollapsed()`](console-api#consolegroupcollapsed) 代替 `console.group()` 来输出一个初始折叠起来的分组信息。

    console.groupCollapsed("Authenticating user '%s'", user);
    if (authenticated) {
      ...
    }
    

![初始折叠起来的分组信息](https://developer.chrome.com/devtools/docs/console-files/groupcollapsed.png)

### 字符串替换和格式化

你传递到 console 的输出信息方法（例如 `log()` 或者 `error()`）的第一个参数里面，可能会包含一个或者多个 _格式声明符（format specifier）_ 。格式声明符通常以 **`%`** 符号为开头然后紧跟一个表示将要插入值的类型的字母（例如 `%s` 表示这里要输出一个字符串）。格式声明符表示在当前位置将会被替换成某个传递进去的值。

下面的这个例子将会把值以 `%s`（字符串）和 `%d` （数字）格式插入并输出。

    console.log("%s has %d points", "Sam", "100");
    
执行后，console 中将会输出 “Sam has 100 points”。

下面的这个列表是支持的格式声明符以及对应格式：

格式声明符 | 描述
---- | ----
`%s` | 将值格式化为字符串。
`%d` 或者 `%i` | 将值格式化为数值。
`%f` | 将目标格式化为浮点型值。
`%o` | 将值格式化为一个可张开的 DOM 对象（类似 Elements 面板）。
`%O` | 将值格式化为一个可张开的 JavaScript 对象。
`%c` | 将第二个参数传递进去的 CSS 样式应用在输出的字符串上。

在下面的例子中 `%d` 格式声明符将会被 `document.childNodes.length` 的数值格式替换；`%f`格式声明符将会替换为被格式化成浮点型数值的 `Date.now()` 值。

    console.log("Node count: %d, and the time is %f.", document.childNodes.length, Date.now());
    
![使用格式声明符](https://developer.chrome.com/devtools/docs/console-files/format-substitution.png)

### 将 DOM 元素格式化为 JavaScript 对象

默认的，当你在 console 输出一个 DOM 元素时，它会以 XML 的格式显示，就像在 Elements 面板中那样：


    console.log(document.body.firstElementChild)

![](https://developer.chrome.com/devtools/docs/console-files/log-element.png)

你也可以使用 `console.dir()` 方法来输出一个元素的 JavaScript 描述：

	console.dir(document.body.firstElementChild);

![](https://developer.chrome.com/devtools/docs/console-files/dir-element.png)

当然，你也可以使用 `console.log()` 配合 `%O` [格式声明符](#string_substitution_and_formatting) 来格式化输出信息：

    console.log("%O", document.body.firstElementChild);


### 使用 CSS 对输出信息添加样式

在使用 [`console.log()`](#writingtotheconsole) 或者其他有关方法的时候，你可以使用 `%c` 格式声明符来对输出的字符串赋予一些样式。

	console.log("%cThis will be formatted with large, blue text", "color: blue; font-size: x-large");
   
![使用 CSS 为输出信息进行美化](https://developer.chrome.com/devtools/docs/console-files/format-string.png)

### 测量某项任务所用时间

你可以使用 [`console.time()`](console-api#consoletimelabel) 和 [`console.timeEnd()`](console-api#consoletimeendlabel) 这两个方法来测量你代码里某个函数或者运算的完成所需时间。在你代码需要计时的地方调用 `console.time()` 方法，然后使用 `console.timeEnd()` 结束这个计时钟（timer）。console 中将会输出调用这两个方法之间经过的时间。

    console.time("Array initialize");
    var array= new Array(1000000);
    for (var i = array.length - 1; i &gt;= 0; i--) {
        array[i] = new Object();
    };
    console.timeEnd("Array initialize");


![ console.time() 和 timeEnd() 的实例](https://developer.chrome.com/devtools/docs/console-files/time-duration.png)

>**注意：** 你必须向 `console.time()` 和 `timeEnd()` 中传递相同的字符串，这样计时钟才能按照预期的工作。

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