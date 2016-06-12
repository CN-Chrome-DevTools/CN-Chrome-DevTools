#概览

Chrome开发者工具是一套内置于google chrome浏览器的开发、调试工具。开发人员可以通过开发者工具深入浏览器的内部机制及他们自己所开发的web应用。通过开发者工
高效地追踪布局问题、设置javascript断点、探究代码优化。

##打开开发者工具

打开一个网页或者web应用。然后通过以下任一种方式打开开发者工具：

- 选择浏览器窗口右上角的**Chrome菜单**![](https://developer.chrome.com/devtools/images/chrome-menu.png)，然后选择**Tools>Developer Tools**
- 在任一元素上点鼠标右键，然后选择**Inspect Element**

有几个用于打开开发者工具的快捷键:

- 使用**Ctrl+Shift+I**(或者Mac的**Cmd+Opt+I**)打开开发者工具
- 使用**Ctrl+Shift+J**(或者Mac的**Cmd+Opt+J**)打开开发者工具并聚焦于Console栏
- 使用**Ctrl+Shift+C**(或者Mac的**Cmd+Opt+C**)以审查元素模式打开开发者工具，如果开发者工具已经处于打开状态则为切换到审查元素模式

为了便于你的日常工作流，[学习使用快捷键](https://developer.chrome.com/devtools/docs/shortcuts)将节省你很多时间

##开发者工具窗口

开发者工具面向任务进行分组，排列在窗口顶部的工具条中。每个工具条项和其对应的面板允许你以特定的方式处理网页或应用的信息(包括DOM元素，资源，源码等)

![](https://developer.chrome.com/devtools/images/devtools-window.png)
*在开发者工具中可使用拾色器*

总起来说，开发者工具一共有8个工具组

- [元素](https://developer.chrome.com/devtools/docs/dom-and-styles)
- [资源](https://developer.chrome.com/devtools/docs/resource-panel)
- [网络](https://developer.chrome.com/devtools/docs/network)
- 源码
- [时间线](https://developer.chrome.com/devtools/docs/timeline)
- [CPU和内存分析](https://developer.chrome.com/devtools/docs/profiles)
- 审计
- [控制台](https://developer.chrome.com/devtools/docs/console)

你可以用快捷键 **Ctrl+[** 和 **Ctrl+]** 来切换面板

##检查DOM和样式

[元素](https://developer.chrome.com/devtools/docs/dom-and-styles)面板允许你看到一个DOM树的全部细节，并且允许你查看和实时编辑DOM元素。如果你需要确认哪个HTML片段对应当前网页的某一部分，你会经常使用元素面板。例如：你可能想知道一个图片是否有id属性并弄清楚id属性值是什么

![](https://developer.chrome.com/devtools/images/elements-panel.png)
*在DOM结构中查看某个标题标签*

[了解更多查看DOM结构和样式](https://developer.chrome.com/devtools/docs/dom-and-styles)

##使用控制台

[JavaScript控制台](https://developer.chrome.com/devtools/docs/console)为测试网页和应用程序的开发人员提供两个重要功能。分别是：

- 开发过程中打印调试信息
- 用作与网页文档和开发者工具进行交互的命令工具

你可以使用[控制台API](https://developer.chrome.com/devtools/docs/console-api)提供的方法打印调试信息。比如[console.log()](https://developer.chrome.com/devtools/docs/console-api#consolelogobject-object)、[console.profile()](https://developer.chrome.com/devtools/docs/console-api#consoleprofilelabel)

你可以使用[命令行API](https://developer.chrome.com/devtools/docs/commandline-api)提供的方法直接在控制台进行表达式评估。例如使用[$()](https://developer.chrome.com/devtools/docs/commandline-api#selector)选择元素或使用[profile()](https://developer.chrome.com/devtools/docs/commandline-api#profilename)进行CPU分析。

![](https://developer.chrome.com/devtools/docs/console-files/expression-evaluation.png)
*在控制台评估一些指令*

[了解更多关于如何使用控制台](https://developer.chrome.com/devtools/docs/console)

##调试JavaScript

随着JavaScript应用程序变得越来越复杂，开发者需要强大的调试工具以高效地快速定位BUG的产生的原因并进行BUG修复。chrome开发者工具包括很多有用的工具，它减少了调试JavaScript的痛苦。

![](https://developer.chrome.com/devtools/images/js-debugging.png)
[了解更多关于如何使用开发者工具调试JavaScript](https://developer.chrome.com/devtools/docs/javascript-debugging)

##提升网络性能

**网络**面板允许你实时查看通过网络请求和下载的资源文件。搞清楚对这些请求进行识别、寻址为什么花费时间比预期要长是网页优化中重要的一步。

![](https://developer.chrome.com/devtools/images/network-panel.png)
*网络求亲的上下文菜单*

[了解更多关于如何提升网络性能](https://developer.chrome.com/devtools/docs/network)

##审计

审计面板可以在一个网页加载时对该网页进行分析。然后提供可以减少网页加载时间提升该网页响应能力的优化建议。我们推荐使用[网页速度查看](https://developers.google.com/speed/pagespeed/insights/)以进行更深入的探究。

![](https://developer.chrome.com/devtools/images/audits-panel.png)
*来自审计的建议*

##提升渲染性能

**时间线**面板为你展示一个完整概览，告诉你当网页或应用进行加载并被使用的过程中时间都是怎么消耗的。从加载资源文件到解析JavaScript、计算样式、重绘，所有时间都展示在时间线面板。

![](https://developer.chrome.com/devtools/devtools/images/timeline-panel.png)
*展示各事件的一个例子*

[了解更多关于如何提升渲染性能](https://developer.chrome.com/devtools/devtools/docs/timeline)

##JavaScript和CSS性能

**分析面板**允许你分析某个web应用或网页执行时间和内存占用。这些信息可以帮助你弄清楚资源被消耗在了哪里，然后帮助你优化你的代码。有以下三种类型的分析：

- **CPU分析**展示执行时间花费在了你的网页中哪些JavaScript函数中
- **堆分析**展示你的网页中JavaScript对象和相关DOM节点的内存分配情况
- **JavaScript分析**展示你的脚本中执行时间的消耗情况

![](https://developer.chrome.com/devtools/devtools/images/profiles-panel.png)
*一个堆快照的例子*

[了解更多股阿奴与偶如何提升JavaScript和CSS性能](https://developer.chrome.com/devtools/devtools/docs/profiles)

##查看存储

**资源面板**允许你查看正在被查看的网页已经加载的资源。它允许你与HTML5 数据库(Local Storage， Cookies， 应用缓存等)进行交互

![](https://developer.chrome.com/devtools/devtools/images/resources-panel.png)
*展示在资源面板中的javaScript文件(来自[web开发入门包](https://developers.google.com/web/starter-kit/))*

[了解更多关于查看资源存储的信息](https://developer.chrome.com/devtools/devtools/docs/resource-panel)

##更多资料

开发者工具文档还有其他一些信息对你非常有用，它们包括：

- [堆分析](https://developer.chrome.com/devtools/devtools/docs/heap-profiling)
- [CPU分析](https://developer.chrome.com/devtools/devtools/docs/cpu-profiling)
- [设备模式和移动端模拟](https://developer.chrome.com/devtools/devtools/docs/device-mode)
- [远程调试](https://developer.chrome.com/devtools/devtools/docs/remote-debugging)
- [开发者工具视频](https://developer.chrome.com/devtools/devtools/docs/videos)

##更多资料

你在[@ChromiumDev](http://twitter.com/ChromiumDev)可以关注我们，或者通过[论坛](https://groups.google.com/forum/?fromgroups#!forum/google-chrome-developer-tools)提问

![](https://developer.chrome.com/devtools/devtools/images/image13.png)
*在控制台进行格式化打印*
