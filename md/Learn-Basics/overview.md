Chrome开发工具(又称DevTools),是一套内嵌在chrome浏览器内部的web编写和调试工具。DevTools提供给web开发人员深入地访问浏览器内部和web应用的机会。DevTools可以有效地 **跟踪布局问题** , **设置JavaScript断点** ,以及 **进行javascript代码的优化** 。

> 注意:如果你是一个web开发人员,想要最新版本的DevTools,移应该使用[Google Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html)。

## 打开DevTools 　　
要在一个网页或者web application中打开DevTools，可以用下面两种方法：    

1. 选择Chrome浏览器窗口右上角的 ![菜单](https://developer.chrome.com/devtools/images/chrome-menu.png),然后选择 **工具** >  **开发工具**。    

2. 右键单击任何页面元素>选择 **审查元素**。

DevTools工具将会在你的浏览器的地步打开。还有几种打开Devtools的快捷方式：

1. 使用 `Ctrl` + `Shift` + `I` (`Cmd` + `Opt` + `I` on Mac)打开DevTools。

2. 使用 `Ctrl` + `Shift` + `J` (`Cmd` + `Opt` + `J` on Mac)打开DevTools并将焦点移到控制台。

3. 使用 `Ctrl` + `Shift` + `C` (`Cmd` + `Shift` + `C` on Mac)打开DevTools并将焦点移到检查元素移模式,或者控制检查元素模式开关如果DevTools已经打开了。

[更多快捷方式](https://developer.chrome.com/devtools/docs/shortcuts)

## DevTools窗口 
DevTools以任务的种类被组织到窗口顶部的工具栏里。每个工具栏项和相应的面板都有一个特定类型的页面或应用程序信息,包括 `Elements` , `Source` 和 `Resources` 等。

![Devtools的颜色选择器](https://developer.chrome.com/devtools/images/devtools-window.png)
> Devtools 中的颜色选择器

总的来说,在Devtools中有8种工具面板: 

* [Elements](https://developer.chrome.com/devtools/docs/dom-and-styles)
* [Resources](https://developer.chrome.com/devtools/docs/resource-panel)
* [Network](https://developer.chrome.com/devtools/docs/network)
* Sources
* [Timeline](https://developer.chrome.com/devtools/docs/timeline)
* [Profiles](https://developer.chrome.com/devtools/docs/profiles)
* Audits
* [Console](https://developer.chrome.com/devtools/docs/console)

你可以用快捷方式 `Ctrl` +` [` 和 `Ctrl` + `]`进行切换.

## 查看DOM结构和样式 

`Elements` 面板让你看到一棵 `DOM` 树,并允许你进行 `DOM` 元素检验和动态编辑。如果需要确认一些页面的 `HTML`片段，你会经常访问 `Elements` 面板。

![查看一个h标签](https://developer.chrome.com/devtools/images/elements-panel.png) 
> 查看一个h标签

[更多关于查看`DOM`结构和样式](https://developer.chrome.com/devtools/docs/dom-and-styles)

## 用控制台工作
JavaScript控制台提供了两种主要功能为开发人员测试web页面和应用程序。在这个地方你可以:

* 在开发过程中打印诊断信息。
* 利用`shell`工具进行`Documents`和`Devtools`的交互。

你可以使用 [Console API](https://developer.chrome.com/devtools/docs/console-api) 提供的控制台日志诊断信息的API。如 [console.log()](https://developer.chrome.com/devtools/docs/console-api#consolelogobject-object) 或 [console.profile()](https://developer.chrome.com/devtools/docs/console-api#consoleprofilelabel) 。

你可以使用 [Command Line Api](https://developer.chrome.com/devtools/docs/commandline-api) 提供的方法直接在控制台中运行表达式。包括 [$()](https://developer.chrome.com/devtools/docs/commandline-api#selector) 命令选择元素或者 [profile()](https://developer.chrome.com/devtools/docs/commandline-api#profilename) 进行CPU分析。

![在控制台中运行命令](https://developer.chrome.com/devtools/docs/console-files/expression-evaluation.png)

[更多关于控制台](https://developer.chrome.com/devtools/docs/console)


## 调试javascript 

随着JavaScript应用程序的复杂性增加,开发人员需要强大的调试工具来帮助快速发现问题的原因并有效地修复它。Chrome DevTools中有一些有用的工具来帮助我们不那么痛苦地调试JavaScript。

![打断点](https://developer.chrome.com/devtools/images/js-debugging.png)

[更多关于调试javascript](https://developer.chrome.com/devtools/docs/javascript-debugging)

## 进行网络加载的优化

`Network` 面板提供了实时的资源请求和网络下载。识别和解决这些超过预期的请求时间是页面优化的重要一步。

![Network](https://developer.chrome.com/devtools/images/network-panel.png)

[更多关于`Network`](https://developer.chrome.com/devtools/docs/network)

## Audits
`Audits` 面板可以从一个页面加载的时候开始分析一个页面。然后提供降低页面加载时间、增加页面响应的优化建议。
[更多见PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)。

## 优化页面渲染
`Timeline` 面板给你一个完整的关于加载和使用web应用程序或页面的时间花费的信息。所有事件,从解析JavaScript加载资、,计算样式、重新渲染都将绘制于`Timeline`面板上。

![Timeline](https://developer.chrome.com/devtools/images/timeline-panel.png)

[更多关于优化渲染](https://developer.chrome.com/devtools/docs/timeline)

## javascript && CSS 优化
`Profiles` 面板可以查看web应用或页面的执行时间和内存使用。这些有助于理解,资源被使用在哪里,帮助优化代码。提供的分析器有:
* **CPU profiler** 显示页面的 `JavaScript` 函数的执行时间.
* **Heap profiler** 显示页面的 `JavaScript` 对象和 `DOM` 节点内存分配。
* **JavaScript profiler** 显示页面脚本的执行时间。

![Profiler](https://developer.chrome.com/devtools/images/profiles-panel.png)

[更多关于如何优化javascript && css 表现](https://developer.chrome.com/devtools/docs/profiles)

## 查看存储
`Resources` 面板可以查看页面上加载的资源. 也可以和 `HTML5 Database`、 `Local Storage` 、 `Cookies` 、 `AppCache` 等进行交互.

![Resource 面板](https://developer.chrome.com/devtools/images/resources-panel.png)

[更多关于Resouces](https://developer.chrome.com/devtools/docs/resource-panel)

## Further Reading

* [Heap Profiling](https://developer.chrome.com/devtools/docs/heap-profiling)  
* [CPU Profiling](https://developer.chrome.com/devtools/docs/cpu-profiling)
* [Device Mode & Mobile Emulation](https://developer.chrome.com/devtools/docs/device-mode)
* [Remote Debugging](https://developer.chrome.com/devtools/docs/remote-debugging)
* [DevTools Videos](https://developer.chrome.com/devtools/docs/videos)
