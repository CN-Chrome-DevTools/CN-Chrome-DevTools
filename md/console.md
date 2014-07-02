使用 Console
==

JavaScript Console 为开发者们测试网页和应用提供了两个主要的功能：

* 使用 [Console API](https://developers.google.com/chrome-developer-tools/docs/console-api) 来输出一段调试信息，例如使用 [console.log()](https://developers.google.com/chrome-developer-tools/docs/console-api#consolelogobject_object)，或者 [console.profile()](https://developers.google.com/chrome-developer-tools/docs/console-api#consoleprofilelabel) 。
* 一个实时的 Shell ，让你可以输入命令，并且通过 Chrome DevTools 操作 document。你可以直接在 Console 里面执行表达式，也可以执行 [Command Line API](https://developers.google.com/chrome-developer-tools/docs/commandline-api) 里面提供的一些方法，例如执行 [$()](https://developers.google.com/chrome-developer-tools/docs/commandline-api#selector) 命令来选择所有元素，或者执行 [profile()](https://developers.google.com/chrome-developer-tools/docs/commandline-api#profile) 命令来启动一个 CPU 测评器（profiler）。

这个文档提供了这两个 API 的功能介绍和常用用法。你也可以去查看 [Console API](https://developers.google.com/chrome-developer-tools/docs/console-api) 和 [Command Line API](https://developers.google.com/chrome-developer-tools/docs/commandline-api) 两个入门参考文档。

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

Console API 是 DevTools 定义的全局对象 console 的方法集合。API 的主要目的是在你应用运行的时候[显示信息](#writing_to_the_console)（例如显示一个属性值，或者一整个对象或者 DOM 对象）到 console 上。为了避免 console 中的视觉混乱，你也可以成组的输出信息。

### 输出信息到 console

[console.log()](console-api#consolelogobject_object) 方法可以把一个或者多个语句当作参数传递进去然后把它们当前的值输出到 console。例如：

    var a = document.createElement('p');
    a.appendChild(document.createTextNode('foo'));
    a.appendChild(document.createTextNode('bar'));
    console.log("Node count: " + a.childNodes.length);

![Console 信息输出](https://developer.chrome.com/devtools/docs/console-files/log-basic.png)

如果不想使用 “＋” 这个表达式连接符（像上图那样）合并输出字符串，你可以把每个要输出的内容当作参数传递进去（用逗号分割），它们会被用空格作为间隔连接成一行字符串输出。

	console.log("Node count:", a.childNodes.length, "and the current time is:", Date.now());


![Console 信息输出](https://developer.chrome.com/devtools/docs/console-files/log-multiple.png)

### 错误和警告

[console.error()](console-api#consoleerrorobject_object) 方法会显示一个红色图标和一段红色信息文本。

    function connectToServer() {
        console.error("Error: %s (%i)", "Server is  not responding",500);
    }
    connectToServer();
    
![console.error() 的例子](https://developer.chrome.com/devtools/docs/console-files/error-server-not-resp.png)

[console.warn()](console-api#consolewarnobject_object) 方法会显示一个黄色警示图标和一段信息文本。

    if(a.childNodes.length &lt; 3 ) {
        console.warn('Warning! Too few nodes (%d)', a.childNodes.length);
    }
    
![console.warn() 的例子](https://developer.chrome.com/devtools/docs/console-files/warning-too-few-nodes.png)

### 断言（Assertions）

[console.assert()](console-api#consoleassertexpression_object) 方法会根据条件判断，只有在它第一个参数的结果为 false 的时候会显示一段错误信息（第二个参数是错误信息）。比如下面的这个例子就是只有当 `list` 元素的子结点数目大于 500 的时候输出这段错误信息。


    console.assert(list.childNodes.length < 500, "Node count is > 500");

![console.assert() 的例子](https://developer.chrome.com/devtools/docs/console-files/assert-failed.png)

### 筛选 console 输出信息

你可以通过选中筛选选项来快速筛选 console 输出的信息级别，例如错误、警告或者正常的输出信息。点击筛选漏斗图标（如下图）然后就可以选择你需要的筛选选项。


![仅仅显示 console.warn() 输出信息](https://developer.chrome.com/devtools/docs/console-files/filter-errors.png)

筛选选项：

*   **All**&mdash;显示所有输出在 console 的信息
*   **Errors**&mdash;只显示 `console.error()` 输出的信息
*   **Warnings**&mdash;只显示 `console.warn()` 输出的信息
*   **Logs**&mdash;只显示 `console.log()`， `console.info()` 以及 `console.debug()` 输出的信息
*   **Debug**&mdash;只显示 `console.timeEnd()`  和其他 console 输出的信息


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

你传递到 console 的输出信息方法（例如 `log()` 或者 `error()`）的第一个参数里面，可能会包含一个或者多个 _格式声明符（format specifier）_ 。格式声明符通常以 `%` 符号为开头然后紧跟一个表示将要插入值的类型的字母（例如 `%s` 表示这里要输出一个字符串）。格式声明符表示在当前位置将会被替换成某个传递进去的值。

下面的这个例子将会把值以 `%s`（字符串）和 `%d` （数字）格式插入并输出。

    console.log("%s has %d points", "Sam", "100");
    
执行后，console 中将会输出 “Sam has 100 points”。

下面的这个列表是支持的格式声明符以及对应格式：

格式声明符 | 描述
---- | ----
`%s` | 将值格式化为字符串。
`%d` 或者 `%i` | 将值格式化为整型。
`%f` | 将目标格式化为浮点型值。
`%o` | 将值格式化为一个可张开的 DOM 对象（类似 Elements 面板）。
`%O` | 将值格式化为一个可张开的 JavaScript 对象。
`%c` | 将第二个参数传递进去的 CSS 样式应用在输出的字符串上。

在下面的例子中 `%d` 格式声明符将会被 `document.childNodes.length` 的数值以整型格式替换；`%f`格式声明符将会替换为被格式化成浮点型数值的 `Date.now()` 值。

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

在使用 [`console.log()`](#writingtotheconsole) 或者其他相关方法的时候，你可以使用 `%c` 格式声明符来对输出的字符串赋予一些样式。

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

**注意：** 你必须向 `console.time()` 和 `timeEnd()` 中传递相同的字符串，这样计时钟才能按照预期的工作。

### 生成时间线（Timeline）

[时间线](https://developer.chrome.com/devtools/docs/timeline.md) 展示了你的 web app 或者网页加载时在哪里花费了时间。[`console.timeStamp()`](console-api#consoletimestamplabel) 方法会在执行它的时候，在时间线上做一个标记。它提供了一种简单的方法来关联与其他浏览器相关的事件，如布局（layout）或者描绘（paints）。


**注意** `console.timeStamp()` 方法只有在时间线录制的时候起作用。

下面的例子中，当应用执行 `AddResult()` 函数时会在时间线上标记一下。


    function AddResult(name, result) {
      console.timeStamp("Adding result");
      var text = name + ': ' + result;
      var results = document.getElementById("results");
      results.innerHTML += (text + "&lt;br&gt;");
    }

正如下面截图所示，`timeStamp()` 命令在下面几个地方为时间线标注；

* 在时间线摘要和细节区域（detail views）中的那一条黄色的垂直线
* 在左边事件记录列表中的一条记录

![时间线中显示时间戳](https://developer.chrome.com/devtools/docs/console-files/timestamp2.png)

### 在 JavaScript 中设置断点

你可以调用 [`debugger`](console-api#debugger) 命令来对你的 JavaScript 代码开启调试模式。例如下面的例子中，当 `brightness()` 函数执行的时候，javaScript 调试器（debugger）将会开启：

    brightness : function() {
        debugger;
        var r = Math.floor(this.red*255);
        var g = Math.floor(this.green*255);
        var b = Math.floor(this.blue*255);
        return (r * 77 + g * 150 + b * 29) &gt;&gt; 8;
    }

![使用 debugger 命令的示例](https://developer.chrome.com/devtools/docs/console-files/debugger.png)

在 [Breakpoint Actions in JavaScript](http://www.randomthink.net/blog/2012/11/breakpoint-actions-in-javascript/) 中， Brian Arnold 发现了一种根据条件断点的有趣的用法。



## 使用命令行 API

为了方便让你的应用输出信息到 console 中，console 同时也可以作为一个实时的 shell 来执行表达式或者 [Command Line API](commandline-api) 提供的一堆命令。这些 API 提供了下面这些特性：

* 一些方便选择 DOM 元素的函数
* 控制 CPU 分析器的函数
* 一些 Console API  方法也可以执行
* 监测事件
* 查看注册在对象上的事件监听器


### 执行表达式

在实时 shell 中，当你摁下 Retun 或者 Enter 键时，会试图执行计算你输入的 JavaScript 表达式。Console 提供了自动完成和 tab 键完成。当你输入表达式的时候，建议属性名会自动弹出来。如果这些属性有相同的前缀，摁下 Tab 键会补全它们。摁下右方向键接受当前推荐选项。如果建议选项只有一条匹配属性时，摁下 Tab 键就会自动接受补全该选项。

![](https://developer.chrome.com/devtools/docs/console-files/evaluate-expressions.png)

如果想要在实时 shell 中一次输入多行表达式（例如一个 function 函数定义），你需要摁下 Shift＋Enter 来换行。

![](https://developer.chrome.com/devtools/docs/console-files/multiline-expression.png)

### 选取对象

命令行 API 提供几个方法可以访问你应用中的 DOM 元素。例如 `$()` 方法返回匹配传递进去的 CSS 选择器的第一个元素，功能类似 [`document.querySelector()`](http://docs.webplatform.org/wiki/css/selectors_api/querySelector)。举个例子，下面代码返回 ID 为 “loginBtn” 的元素。

    $('#loginBtn');


![](https://developer.chrome.com/devtools/docs/console-files/select-login-btn.png)

`$$()` 命令返回匹配传递进去 CSS 选择器的所有元素组成的数组，功能类似 [`document.querySelectorAll()`](http://docs.webplatform.org/wiki/css/selectors_api/querySelectorAll)。举个例子，下面代码选择了所有带有 "loginBtn" 类的  `&lt;button&gt;` 元素。

    $$('button.loginBtn');

![](https://developer.chrome.com/devtools/docs/console-files/select-multiple-login.png)

最后，[`x()`](commandline-api#xpath) 方法使用 XPath 路径作为参数然后返回所有匹配这个特定路径元素组成的数组。下面代码返回所有 `<body>` 标签下面的 &lt;script&gt; 元素：

    $x('/html/body/script');

### 审查 DOM 元素和 JavaScript 堆对象

[`inspect()`](commandline-api#inspectobject) 方法以 DOM 元素引用（或者 JavaScript 引用）作为参数然后把对应元素或者对象显示在相应的面板中－DOM 元素显示在 Elements 面板中，JavaScript 对象显示在 Profiles 面板中。

例如，下面截图中 `$()` 用来获得一个 `<li>` 元素的引用。然后将 the last evaluated expression property ([`$_`](commandline-api#_)) 传递给 `inspect()` 从而打开 Elements 面板看到那个元素。

![](https://developer.chrome.com/devtools/docs/console-files/inspect2.png)

### 获得最近选择的元素或者对象

通常的当要测试你要选择的 DOM 元素可以－直接在 Elements 面板选择或者使用 Selection 工具（放大镜图标）－这样你才可以详细的审查这个元素。同样的，当要在 Profiles 面板中统计内存使用简况，你也需要先获取需要审查的 JavaScript 对象。

Console 会记住最后五个元素（或者堆对象）你可以通过使用 `$0`, `$1`, `$2`, `$3` 和 `$4` 来选择它们并且让它们像属性一样使用。最近选择的元素或者对象可以通过 `$0` 调用，倒数第二个最近的是 `$1`，以此类推。

下面这个截图展示了在选择三个不同元素之后，这些属性在 Elements 面板中返回的值：

![最近选择的元素](https://developer.chrome.com/devtools/docs/console-files/recent-selection.png)

**注意：**你也可以在 Console 中的任意元素上右击或者摁住 Control 键点击并且选择 **Reveal in Elements Panel** 从而通过 Elements 面板查看。


### 监听事件

[`monitorEvents()`](commandline-api#monitoreventsobject_events) 命令可以监控某对象的一个或多个指定的事件。当事件在被监控的对象上触发时，相应的事件对象将会被打印到 Console 上。 你指定的对象和你想要监听的事件。例如，下面代码会监听触发在全局 window 对象的每一个“resize”事件。

    monitorEvents(window, "resize");

![监听 window resize 事件](https://developer.chrome.com/devtools/docs/console-files/monitor-resize.png)

想要监控多个事件，你可以把事件名数组作为第二个参数传递。下面代码会监控触发在 `document.body` 上的 “mousedown” 和 “mouseup” 事件。

    monitorEvents(document.body, ["mousedown", "mouseup"]);

你也可以传递一个被支持的“事件类型（event types）”作为第二参数，DevTools 会将其映射为一系列实际的事件名。例如，“touch”事件类型会让 DevTools 去监控目标对象上的 “thouchstart”，“thouchend”，“touchmove” 以及 “touchcancel” 事件。

    monitorEvents($('#scrollBar'), "touch");

查看 Console API 参考 [`monitorEvents()`](commandline-api#monitoreventsobject_events) 文档，你可以找到被支持的事件类型列表。

停止监控事件可以使用 `unmonitorEvents()`，需要把想要停止监控的对象传递进去。

    unmonitorEvents(window);

### 控制 CPU 分析器

你可以使用命令行命令 [`profile()`](commandline-api#profilename) 和 [`profileEnd()`](commandline-api#profileendname) 来创建一个 JavaScript CPU 分析器（profiler）。你可以为你创建的分析器自定义一个特定的名字。

例如，下面展示了如何用默认名字创建一个分析器：

![](https://developer.chrome.com/devtools/docs/commandline-api-files/profile-console.png)

在 Profiles 面板中，这个新创建的分析器会以 “Profile 1” 的名字出现：

![](https://developer.chrome.com/devtools/docs/commandline-api-files/profile-panel.png)

如果你为新创建的分析器指定了一个标签，它通常用作分析器的标题。如果你创建了多个同名的分析器，他们会被归在同一标题的的不同子标题下：

![](https://developer.chrome.com/devtools/docs/commandline-api-files/profile-console-2.png)

Profiles 面板的结果如下：

![](https://developer.chrome.com/devtools/docs/commandline-api-files/profile-panel-2.png)

CPU 分析器可以被嵌套，例如：

    profile("A");
    profile("B");
    profileEnd("B")
    profileEnd("A")

调用停止或者开启分析命令的时候并不需要严格按照嵌套的顺序闭合。例如，下面代码跟上面代码功能一样：

    profile("A");
    profile("B");
    profileEnd("A");
    profileEnd("B");

