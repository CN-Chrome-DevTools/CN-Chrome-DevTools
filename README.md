Chrome 开发者工具中文手册
===============

Chrome DevTools 是公认的优秀的前端调试工具，由于功能强大，所以使用起来有一定的学习门槛，与此同时 Chrome DevTools 暂时没有中文手册，对于不太熟悉英文的同学会比较吃力。

本项目的初衷是为想使用或者正在使用 Chrome DevTools 的同学提供一个中文手册，方便大家学习使用这个优秀的工具，提高前端开发效率和质量！


## 翻译流程

###第一阶段

先将 [Chrome DevTools](https://developers.google.com/chrome-developer-tools/) 的内容按现有的目录结构翻译成中文，其中：

- 文章正文内容均放在 `md` 目录下，采用 `md` 格式。
- 文章中所用到的图片资源暂时先用现有英文手册的原始链接，后续图片资源会统一托管到[七牛云存储](http://www.qiniu.com/)


####文件命名规则
文件名为 [Chrome DevTools](https://developers.google.com/chrome-developer-tools/) 对应文章超链接中 `/docs/` 后的部分。

例如：`https://developers.google.com/chrome-developer-tools/docs/authoring-development-workflow` 这篇文档，对应 `authoring-development-workflow.md` 这个文件。

对于带有二级子页面的文档，在文档前面会带有父级的名字使用 `--` 分割。

例如：`https://developers.google.com/chrome-developer-tools/docs/css-preprocessors` 属于 `https://developers.google.com/chrome-developer-tools/docs/dom-and-styles` 那么将其命名为 `dom-and-styles--css-preprocessors`。


###第二阶段

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

## 正在翻译文章

* JavaScript Memory Profiling
* Using the Console
	* Console API Reference
	* Command Line API Reference
* Keyboard Shortcuts

## 项目翻译目录

* Authoring and Development Workflow
* Editing Styles And The DOM
	* Working with CSS Preprocessors
* Managing Application Storage
* Evaluating Network Performance
* Debugging JavaScript
* Performance Profiling with the Timeline
	* Timeline demo: Diagnosing forced synchronous layouts
* Profiling JavaScript
* JavaScript Memory Profiling
	* Heap Profiler Demos
		* Gathering Scattered Objects
		* Verifying Action Cleanness
		* Exploring the Heap Contents
		* Uncovering DOM Leaks
		* Finding Accumulation Points
* Mobile Emulation
* Using the Console
	* Console API Reference
	* Command Line API Reference
* Keyboard Shortcuts
* Tips and Tricks
* Settings
* Rendering Settings
* Remote Debugging Chrome on Android
	* Debugging Protocol
		* 1.1
		* 1.0
		* 0.1
		* tip-of-tree
* Integrating with DevTools
	* Sample DeveTools Extensions
	* Sample Debugging Protocol Clients
	* DevTools Extensions Gallery
* Additional Resources
	* Creating A Clean Testing Environment
	* Videos
	* Blog posts
	* Mailing list
* Contributing

（翻译完成的，请使用删除线将对应划去）

## 贡献者（按参与时间排序）

- [SunLn](https://github.com/SunLn)
- [JiangShui Yu](https://github.com/yujiangshui)
- [DestinyXie](https://github.com/DestinyXie)
- 你

（Fork 之后自行添加到最后）
