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

## DevTools窗口 
DevTools以任务的种类被组织到窗口顶部的工具栏里。每个工具栏项和相应的面板都有一个特定类型的页面或应用程序信息,包括 `Elements` , `Source` 和 `Resources` 等。

![Devtools的颜色选择器](https://developer.chrome.com/devtools/images/devtools-window.png)
> Devtools 中的颜色选择器

总的来说,在Devtools中有8种工具面板: 

* Elements
* Resources
* Network
* Sources
* Timeline
* Profiles
* Audits
* Console

你可以用快捷方式 `Ctrl` +` [` 和 `Ctrl` + `]`进行切换.

## 查看DOM结构和样式 

`Elements` 面板让你看到一棵 `DOM` 树,并允许你进行 `DOM` 元素检验和动态编辑。如果需要确认一些页面的 `HTML`片段，你会经常访问 `Elements` 面板。

![查看一个h标签](https://developer.chrome.com/devtools/images/elements-panel.png) 
> 查看一个h标签

[更多关于查看`DOM`结构和样式](https://developer.chrome.com/devtools/docs/dom-and-styles)











