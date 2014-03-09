Chrome开发者工具中文手册
===============


## 项目介绍

前端调试工具除了firebug,就是chrome devtools了（尼玛，要和我提ie的开发者工具，我和你拼命）。firebug有良好的中文翻译，但chrome devtools暂时没有中文手册。

本项目的初衷是为想使用或者正在使用chrome devtools的童鞋们提供一个中文手册，方便大家交流。

如果能够顺带提高chrome devtools的使用率，改变firebug一统天下的局面，那是本项目无毒无害的副作用。

## 翻译思路

* 第一阶段先将[chrome devtools](https://developers.google.com/chrome-developer-tools/)的内容按现有的目录结构翻译成中文
 
	* 文章正文内容均放在`md`目录下，采用`md`格式，文件名为[chrome devtools](https://developers.google.com/chrome-developer-tools/)每篇文章url`/docs/`后的部分，比如现有的`authoring-development-workflow.md` 对应的是`https://developers.google.com/chrome-developer-tools/docs/authoring-development-workflow`文章
 
	* 文章中所用到的图片资源暂时先用现有英文手册的原始链接，后续图片资源会统一托管到[七牛云存储](http://www.qiniu.com/)
	
	* 如果您英语水平一般，但也想参与项目，将对应的文档做成md格式提交 Pull Request 即可，后续的人可以在您提交的`md`文档中继续工作。善莫大焉。
 
* 第二阶段会做一个手册的展示站，现在考虑的是github page。


## 参与项目

您的参与是对本项目的最大支持，一个个commit堆积起来就是一个了不起的repo，欢迎您提交 Pull Request，哪怕是改正一个错别字、修正一个病句。

如有兴趣或疑问，随时和我联系，邮箱 dengchenhua@gmail.com ，qq 422282420。

参与方法如下：

* 登录 https://github.com
 
* Fork `git@github.com:CN-Chrome-DevTools/CN-Chrome-DevTools.git`
 
* 创建您的特性分支 (git checkout -b new-feature)
 
* 提交您的改动 (git commit -m 'Added some features or fixed a bug or change a text')
 
* 将您的改动记录提交到远程 git 仓库 (git push origin new-feature)

* 然后到 github 网站的该 git 远程仓库的 new-feature 分支下发起 Pull Request


##目录（未链接，期待翻译）


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

* Integrating with DevTools

* Additional Resources

* Contributing
