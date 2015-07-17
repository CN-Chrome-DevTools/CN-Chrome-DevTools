控制台API参考文档
===============

The Console API provides web applications with methods for writing information to the console, creating JavaScript profiles, and initiating a debugging session.

控制台API给web应用提供了一些方法来打印信息到控制台、创建Javascript分析以及开始调试会话。

目录：

[TOC]

### console.assert(expression, object)

If the specified expression is false, the message is written to the console along with a stack trace. In the following example, the assert message is written to the console only when the document contains fewer than ten child nodes:

如果参数表达式`expression`的值为`false`，那么参数`object`的信息会和堆栈轨迹一同显示在控制台。在下面的例子中，只有在`myList`元素的子节点少于10个时，断言信息才会显示在控制台。

``` javascript
var list = document.querySelector('#myList');
    console.assert(list.childNodes.length < 10, "List item count is >= 10");
```

![console.count()使用示例](https://developer.chrome.com/devtools/docs/console-files/assert-failed-list.png)

### console.clear()

Clears the console.

清空控制台。

``` javascript
console.clear();
```

Also see Clearing the console.

同[清空控制台](https://developer.chrome.com/devtools/docs/console.md#clearing-console-history)

However, if Preserve Logs is on, console.clear() will not do anything in case there's some iframe which calls console.clear() and can make your debugging process harder. "Clear console" in the context menu will still work, and actually clear the console.

注意，当 Preserve Logs（保留日志）被勾选时，使用`console.clear()`是无效的，以防止有些页面调用`console.clear()`使调试过程变得困难。菜单的"Clear console"按钮仍然可以工作并且清空控制台。


### console.count(label)

Writes the the number of times that count() has been invoked at the same line and with the same label.

在每行相同的标签后写下`count()`被调用的次数。

In the following example count() is invoked each time the login() function is invoked.

在下面的例子中，每次`login()`函数被调用时`count()`也被调用。

``` javascript
function login(user) {
    console.count("Login called");
    // login() code...
}
```

![console.count(label)示例](https://developer.chrome.com/devtools/docs/console-files/count.png)

In this example, count() is invoked with different labels, each of which is incremented separately.

在下面这个例子，`count()`被调用时使用了不同的标签，每一个标签是独立计算次数的。

``` javascript
function login(user) {
    console.count("Login called for user '" +  user + "'");
    // login() code...
}
```

![console.count(label)示例](https://developer.chrome.com/devtools/docs/console-files/count-unique.png)

### console.debug(object [, object, ...])

This method is identical to console.log().

该方法和[console.log()](#consolelogobject-object)作用相同。

### console.dir(object)

Prints a JavaScript representation of the specified object. If the object being logged is an HTML element, then the properties of its DOM representation are displayed, as shown below:

打印参数`object`的JavaScript描述。如果打印的`object`是一个HTML元素，它的DOM属性会被显示，如下所示：

``` javascript
console.dir(document.body);
```

![console.dir(object)例子](https://developer.chrome.com/devtools/docs/console-files/consoledir-body.png)

You can also use the object formatter (%O) in a console.log() statement to print out an element's JavaScript properties:

也可以在`console.log()`方法中使用对象格式符（%O）来打印一个元素的JavaScript属性：

``` javascript
console.log("document body: %O", document.body);
```

![console.dir(object)例子](https://developer.chrome.com/devtools/docs/console-files/consolelog-object-formatter.png)

Calling console.dir() on a JavaScript object is equivalent to calling console.log() on the same object—they both print out the object's JavaScript properties in a tree format.

对一个JavaScript对象调用`console.dir()`等同于对它调用`console.log()`——他们都是使用树形格式打印对象的JavaScript属性。

Compare this with the behavior of console.log(), which displays the element in an XML format as it would appear in the Elements panel:

对比`console.log()`，它和Elements面板里一样，使用xml格式显示元素。

``` javascript
console.log(document.body);
```

![console.dir(object)例子](https://developer.chrome.com/devtools/docs/console-files/consolelog-body.png)

### console.dirxml(object)

Prints an XML representation of the specified object, as it would appear in the Elements panel. For HTML elements, calling this method is equivalent to calling console.log().

同Elements面板里一样，打印出object的XML结构。对于HTML元素来说，调用该方法等同于调用`console.log()`.

``` javascript
var list = document.querySelector("#myList");
    console.dirxml();
```

 - %O is a shortcut for dir
 - %o acts either as dir or dirxml depending on the object type (non-dom or dom)

 - %O `dir`的快捷方式
 - %o 非dom类型同`dir`，dom类型同`dirxml`

### console.error(object [, object, ...])

Similar to console.log(), console.error() and also includes a stack trace from where the method was called.

和`console.log()`一样，`console.error()`也会显示方法调用的堆栈轨迹。

``` javascript
function connectToServer() {
    var errorCode = 1;
    if (errorCode) {
        console.error("Error: %s (%i)", "Server is  not responding", 500);
    }
}
connectToServer();
```

![console.error示例](https://developer.chrome.com/devtools/docs/console-files/error-server-not-resp.png)

### console.group(object[, object, ...])

Starts a new logging group with an optional title. All console output that occurs after calling this method and calling console.groupEnd() appears in the same visual group.

使用指定标记记录一组日志。在调用该方法后，所有控制台输出将在这一组日志中，直到调用`console.groupEnd()`。

``` javascript
console.group("Authenticating user '%s'", user);
console.log("User authenticated");
console.groupEnd();
```

![console.group示例](https://developer.chrome.com/devtools/docs/console-files/log-group-simple.png)

You can also nest groups:
可以嵌套使用组：

``` javascript
// New group for authentication:
console.group("Authenticating user '%s'", user);
// later...
console.log("User authenticated", user);
// A nested group for authorization:
console.group("Authorizing user '%s'", user);
console.log("User authorized");
console.groupEnd();
console.groupEnd();
```

![console.group示例](https://developer.chrome.com/devtools/docs/console-files/nestedgroup-api.png)

### console.groupCollapsed(object[, object, ...])
Creates a new logging group that is initially collapsed instead of open, as with console.group().

创建一组默认不展开的日志。

``` javascript
console.groupCollapsed("Authenticating user '%s'", user);
console.log("User authenticated");
console.groupEnd();
console.log("A group-less log trace.");
```

![console.groupCollapsed()示例](https://developer.chrome.com/devtools/docs/console-files/groupcollapsed.png)

### console.groupEnd()

Closes the most recently created logging group that previously created with console.group() or console.groupCollapsed(). See console.group() and console.groupCollapsed() for examples.

关闭最近的一组使用`console.group()`或者`console.groupCollapsed()`创建的日志。见上述示例。

### console.info(object [, object, ...])

This method is identical to console.log().

该方法和[console.log()](#consolelogobject-object)作用相同。

### console.log(object [, object, ...])

Displays a message in the console. You pass one or more objects to this method, each of which are evaluated and concatenated into a space-delimited string. The first parameter you pass to log() may contain format specifiers, a string token composed of the percent sign (%) followed by a letter that indicates the formatting to be applied.

用于打印信息到控制台。接受一个或多个参数，它们的结果连接起来输出。第一个参数是格式字符串，可以包含以`%`开头的格式占位符。

Dev Tools supports the following format specifiers:
支持的占位符如下：

|Format Specifier|Description|
|------|------|
|%s    |Formats the value as a string.|
|%d    |or %i   Formats the value as an integer.|
|%f    |Formats the value as a floating point value.|
|%o    |Formats the value as an expandable DOM element (as in the Elements panel).|
|%O    |Formats the value as an expandable JavaScript object.|
|%c    |Formats the output string according to CSS styles you provide.|

|占位符|说明|
|------|------|
|%s    |字符串|
|%d 或 %i   |整数|
|%f    |小数|
|%o    |DOM元素|
|%O    |javascript对象|
|%c    |css样式|

Basic example:
无占位符示例：

``` javascript
console.log("App started");
```

An example that uses string (%s) and integer (%d) format specifiers to insert the values contained by the variables userName and userPoints:

占位符`%s`和`%d`示例，将变量`userName`和`userPoints`插入到字符串显示：

``` javascript
console.log("User %s has %d points", userName, userPoints);
```

![console.log示例](https://developer.chrome.com/devtools/docs/console-files/log-format-specifier.png)

An example of using the element formatter (%o) and object formatter (%O) on the same DOM element:

对同一个DOM元素分别使用占位符`%o`和`%O`的示例：

``` javascript
console.log("%o, %O", document.body, document.body);
```

![console.log示例](https://developer.chrome.com/devtools/docs/console-files/log-object-element.png)

The following example uses the %c format specifier to colorize the output string:

使用占位符`%c`指定输出字符串的样式：

``` javascript
console.log("%cUser %s has %d points", "color:orange; background:blue; font-size: 16pt", userName, userPoints);
```

![console.log示例](https://developer.chrome.com/devtools/docs/console-files/log-format-styling.png)

### console.profile([label])

When the Chrome DevTools is open, calling this function starts a JavaScript CPU profile with an optional label.To complete the profile, call console.profileEnd(). Each profile is added to the Profiles tab.

当打开Chrome调试工具时，调用该方法会开始记录Javascript CPU分析，参数label为可选的标题。调用`console.profileEnd()`结束分析。每一次分析都会被加到Profiles面板。

In the following example a CPU profile is started at the entry to a function that is suspected to consume excessive CPU resources, and ended when the function exits.

下面的示例中，假设有一个消耗大量CPU资源的函数，在其开始执行时记录CPU数据，退出时结束记录。

``` javascript
function processPixels() {
    console.profile("Processing pixels");
    // later, after processing pixels
    console.profileEnd();
}
```

### console.profileEnd()

Stops the current JavaScript CPU profiling session, if one is in progress, and prints the report to the Profiles panel.

停止当前Javascript CPU分析，如果有正在处理的，则在Profiles面板显示报告。

``` javascript
console.profileEnd()
```

See console.profile() for example use.

示例见[console.profile()](#consoleprofilelabel)

### console.time(label)

Starts a new timer with an associated label. When console.timeEnd() is called with the same label, the timer is stopped the elapsed time displayed in the Console. Timer values are accurate to the sub-millisecond.

使用`console.time(label)`开始一个标识为`label`的计时器。当`console.timeEnd(label)`被调用时，停止计时并显示计时时间。计时器精确到毫秒。

``` javascript
console.time("Array initialize");
var array= new Array(1000000);
for (var i = array.length - 1; i >= 0; i--) {
    array[i] = new Object();
};
console.timeEnd("Array initialize");
```

![console.time()示例](https://developer.chrome.com/devtools/docs/console-files/time-duration.png)

Note: The string you pass to the time() and timeEnd() methods must match for the timer to finish as expected.

注意：传递给`time()`和`timeEnd()`的字符串参数必须相同，计时器才能准确结束计时。

### console.timeEnd(label)

Stops the timer with the specified label and prints the elapsed time.

停止标识为`label`的计时器，显示计时时间。

For example usage, see console.time().

示例见[console.time()](#consoletimelabel)

### console.timeStamp([label])

This method adds an event to the Timeline during a recording session. This lets you visually correlate your code generated time stamp to other events, such as screen layout and paints, that are automatically added to the Timeline.

在记录会话时，调用该方法会添加一条事件到Timeline面板。能够可视化显示你的代码生成的时间戳和其他事件的对应关联，例如自动添加到Timeline的页面排版和渲染。

See Marking the Timeline for an example of using `console.timeStamp()`.

使用示例见[Marking the Timeline](https://developer.chrome.com/devtools/docs/console.md#marking-the-timeline)。

### console.trace(object)

Prints a stack trace from the point where the method was called, including links to the specific lines in the JavaScript source. A counter indicates the number of times that trace() method was invoked at that point, as shown in the screen shot below.

调用该方法时，打印代码所在位置的堆栈轨迹，并显示对应的JavaScript代码行数的链接。显示的计数器表示`trace()`方法在所在位置被调用的次数。

![Example of a stack trace with counter](https://developer.chrome.com/devtools/docs/console-files/console-trace.png)

It is also possible to pass in arguments to trace(). For example:
也可以传递参数给`trace()`，如下：

![Example of a stack trace with arguments](https://developer.chrome.com/devtools/docs/console-files/console-trace-args.png)

### console.warn(object [, object, ...])

This method is like console.log() but also displays a yellow warning icon along with the logged message.

相当于调用`console.log()`的同时显示一个黄色的警告图标。

``` javascript
console.warn("User limit reached! (%d)", userPoints);
```

![console.warn()示例](https://developer.chrome.com/devtools/docs/console-files/log-warn.png)

### debugger

The global debugger function causes Chrome to stop program execution and start a debugging session at the line where it was called. It is equivalent to setting a "manual" breakpoint in the Sources tab of Chrome DevTools.

调用全局`debugger`函数使Chrome暂停执行程序，在代码所在行开启调试会话。等同于在Sources面板手动设置一个断点。

Note: The debugger command is not a method of the console object.

注意：debugger不是`console`对象的方法。

In the following example the JavaScript debugger is opened when an object's brightness() function is invoked:

在下面的示例中，当对象的`brightness()`函数被调用时，会开启Javascript调试：

``` javascript
brightness : function() {
    debugger;
    var r = Math.floor(this.red*255);
    var g = Math.floor(this.green*255);
    var b = Math.floor(this.blue*255);
    return (r * 77 + g * 150 + b * 29) >> 8;
}
```

![debugger示例](https://developer.chrome.com/devtools/docs/console-files/debugger.png)
