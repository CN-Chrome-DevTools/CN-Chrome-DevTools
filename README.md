Chrome 开发者工具中文手册
===============

Chrome DevTools 是公认的优秀的前端调试工具，由于功能强大，所以使用起来有一定的学习门槛，与此同时 Chrome DevTools 暂时没有中文手册，对于不太熟悉英文的同学会比较吃力。

本项目的初衷是为想使用或者正在使用 Chrome DevTools 的同学提供一个中文手册，方便大家学习使用这个优秀的工具，提高前端开发效率和质量！


## 翻译流程

### 第一阶段

先将 [Chrome DevTools](https://developer.chrome.com/devtools/index) 的内容按现有的目录结构翻译成中文，其中：

- 文章正文内容均放在 `md` 目录下，采用 `md` 格式。
- 文章中所用到的图片资源暂时先用现有英文手册的原始链接，后续图片资源会统一托管到[七牛云存储](http://www.qiniu.com/)

#### 文件命名规则

文件名为 [Chrome DevTools](https://developers.google.com/chrome-developer-tools/) 对应文章超链接中 `/docs/` 后的部分。所有的空格和 `&` 用 `-` 代替。

例如：`https://developers.google.com/chrome-developer-tools/docs/authoring-development-workflow` 这篇文档，对应 `authoring-development-workflow.md` 这个文件。

对于下级子页面文档，将其放在以父级文档名称命名的文件夹下面。

例如：`https://developers.google.com/chrome-developer-tools/docs/css-preprocessors` 属于 `https://developers.google.com/chrome-developer-tools/docs/dom-and-styles` 那么将放置在 `dom-and-styles/css-preprocessors.md`。

### 第二阶段

根据翻译文档，制作成类似在线手册或者与官方文档类似的网站，方便大家参阅。

## 参与项目

欢迎你参与翻译本项目，在翻译的过程中，可以锻炼你的英语能力和 Chrome DevTools 的实际应用能力，同时还为他人提供方便，何乐而不为？

一个个 commit 堆积起来就是一个了不起的 repo，欢迎你 Fork 并提交 Pull Request 或者 Issue ，哪怕是改正一个错别字、修正一个病句，我们都会很高兴。

参与方法和步骤如下：

* 登录 https://github.com

* Fork `git@github.com:CN-Chrome-DevTools/CN-Chrome-DevTools.git`

* 创建您的特性分支 (git checkout -b new-feature)

* 提交您的改动 (git commit -m 'Added some features or fixed a bug or change a text')

* 将您的改动记录提交到远程 git 仓库 (git push origin new-feature)

* 然后到 github 网站的该 git 远程仓库的 new-feature 分支下发起 Pull Request

如果你有任何疑问或者建议、技巧，欢迎加入 Chrome DevTools QQ 讨论群：365161310
## 翻译规则
### 外部链接
对于有质量较好的翻译的外部链接，将链接指向译文。
例如：
```md
[window](https://developer.mozilla.org/en-US/docs/Web/API/Window)
```
将其改为
```md
[`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 
```
如果外部链接没有翻译的版本或质量很差，请不要修改该链接。
### 专有名词
翻译专有名词后，在其后方以括号和英文说明原词条，例如：同源政策（Same Origin Policy）
若一篇文章出现两次以上相同专有名词，则在第一次之后补上原词条即可。
对于无法找到对应的词条翻译，直接留下原文词条。

## 正在翻译文章

* JavaScript Memory Profiling
* Keyboard Shortcuts
* Authoring and Development Workflow
* Mobile Emulation
* Editing Styles And The DOM
## 项目翻译目录

* Learn Basics
	* ~~[Overview](https://developer.chrome.com/devtools/index)~~
	* ~~[Development Workflow](https://developer.chrome.com/devtools/docs/authoring-development-workflow)~~
	* ~~[Using the Console](https://developer.chrome.com/devtools/docs/console)~~
	* [Tips & Tricks](https://developer.chrome.com/devtools/docs/tips-and-tricks)
	* [Additional Resources](https://developer.chrome.com/devtools/docs/videos)
* Use Tools
	* Inspecting & Tweaking
		* [Editing Styles And The DOM](https://developer.chrome.com/devtools/docs/dom-and-styles)
		* [Working with CSS Preprocessors](https://developer.chrome.com/devtools/docs/css-preprocessors)
		* [Managing Application Storage](https://developer.chrome.com/devtools/docs/resource-panel)
	* [Debugging JavaScript](https://developer.chrome.com/devtools/docs/javascript-debugging)
	* [Device Mode & Mobile Emulation](https://developer.chrome.com/devtools/docs/device-mode)
	* [Remote Debugging on Android](https://developer.chrome.com/devtools/docs/remote-debugging)
	* [Saving Changes with Workspaces](https://developer.chrome.com/devtools/docs/workspaces)
* Performance & Profiling
	* [Evaluating Network Performance](https://developer.chrome.com/devtools/docs/network)
	* [Using the Timeline](https://developer.chrome.com/devtools/docs/timeline)
	* [Timeline Demo: Layout Thrashing](https://developer.chrome.com/devtools/docs/demos/too-much-layout/index)
	* [Profiling JavaScript Performance](https://developer.chrome.com/devtools/docs/cpu-profiling)
	* ~~[JavaScript Memory Profiling](https://developer.chrome.com/devtools/docs/javascript-memory-profiling)~~
* Reference
	* [Console API Reference](https://developer.chrome.com/devtools/docs/console-api)
	* [Command Line API Reference](https://developer.chrome.com/devtools/docs/commandline-api)
	* DevTools Extensions API
		* [Integrating with DevTools](https://developer.chrome.com/devtools/docs/integrating)
		* [Sample DevTools Extensions](https://developer.chrome.com/devtools/docs/sample-extensions)
		* [Sample DevTools Protocol Clients](https://developer.chrome.com/devtools/docs/debugging-clients)
	* ~~[Keyboard Shortcuts](https://developer.chrome.com/devtools/docs/shortcuts)~~
	* [Settings](https://developer.chrome.com/devtools/docs/settings)
	* Remote Debugging Protocol
		* [Remote debugging protocol](https://developer.chrome.com/devtools/docs/debugger-protocol)
		* [Version 1.1](https://developer.chrome.com/devtools/docs/protocol/1.1/index)
		* [Version 1.0](https://developer.chrome.com/devtools/docs/protocol/1.0/index)
		* [Version .1](https://developer.chrome.com/devtools/docs/protocol/0.1/index)
		* [Tip-of-tree](https://developer.chrome.com/devtools/docs/protocol/tot/index)

（翻译完成的，请使用删除线将对应划去）

## 贡献者（按参与时间排序）

- [SunLn](https://github.com/SunLn)
- [JiangShui Yu](https://github.com/yujiangshui)
- [DestinyXie](https://github.com/DestinyXie)
- [He--He](https://github.com/He--He)
- [Goat](https://github.com/gzhjs)
- [kxq-ttjq](https://github.com/kxq-ttjq)

（Fork 之后自行添加到最后）