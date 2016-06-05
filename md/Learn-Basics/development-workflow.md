#开发工作流程

开发者的工作流程中通常会涉及到一些步骤来完成某个任务。当使用“开发者工具”进行开发时，我们可以对其进行优化，以便更加快速地完成常见任务，例如定位文件或函数、即时编辑脚本与样式、保存常用的代码片段，或者为了使用方便而进行简单的布局调整。

在这一章节里，我们将探索一些技巧，它们将使你更高效地使用“开发者工具”。

##将工具栏垂直拆分至窗口右侧

你也许感觉位于窗口底部的“开发者工具”区域足够宽，但不够高，则此时可以选择将工具栏对齐并固定至窗口右侧，以便在左侧审查当前页面，同时在右侧进行调试。

这种做法在以下情况尤为有用：

- 想充分利用宽屏显示器的空间来查看与调试代码；
- 使分离区域的宽度小于400px（目前Chrome的最小宽度）来测试窗口大小改变时的布局；
- 纵向空间更便于调试较长的脚本。

打开需要调试的页面，然后通过**长按**“开发者工具”左下角的布局图标来进行切换。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/chrome_docktomain.jpg)

> **注意**：“开发者工具”会记住你上一次的选择，所以你可以在最常用的两项间来回切换。

长按图标会显示可选的布局方式，选择后布局将会立即改变生效。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/chrome_docktoright.jpg)

> **注意**：每个标签页中的“开发者工具”可以有其各自的位置。这意味着一个标签页中的“开发者工具”可以在右侧，同时在另一个标签页中位于底部。

##快速拖动至右侧

也可以按住并拖动“开发者工具”的工具栏来改变其位置，就像下面动画里所展示的那样。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/devtools-drag-to-right.gif)

相对于前一种通过布局按钮进行切换的方法，这种方法可以作为一个有用的替代，因为它仅需一个步骤。

##搜索、定位与过滤

###通过文件名过滤查找一个脚本、样式或代码片段

快速定位文件对开发者来说是个基础的功能。“开发者工具”允许你在所有的脚本、样式以及代码片段中用下面的快捷键进行查找：

- `Ctrl + O (Windows, Linux)`
- `Cmd + O (Mac OSX)`

这些快捷键无论在哪个面板中都会生效。例如对于这个[Todo app](http://todomvc.com/architecture-examples/angularjs/#/)，上面的快捷键将会打开“Source”面板，并展现一个显示了所有可用文件的搜索框。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_filter.jpg)

我们还可以定位到更具体的文件（例如文件名中包含“script”）或者选择一个文件来查看或编辑。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_basefind.jpg)

> **注意**：我们在所有的对话框中都支持驼峰式匹配。例如需要打开`FooBarScript.js`，为节省时间只需输入`FBaSc`即可。

###在当前文件中查找文本

在当前文件中查找特定字符串可以用下面的快捷键完成：

- `Ctrl + F (Windows, Linux)`
- `Cmd + F (Mac OSX)`

在搜索框中输入关键词后，按回车跳至第一项匹配的结果。接着按回车会跳到下一个匹配结果，同时可以点击（搜索框右侧的）上下箭头在结果间进行切换，如下图所示。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_findone.jpg)

###在当前文件中替换文本

除了支持在当前文件中查找文本，“开发者工具”也支持对单个匹配项或所有匹配项进行替换。选中“Replace”将会出现第二个文本框以便输入替换的内容。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_find.jpg)

###在所有文件中查找文本

如果希望在该页面已加载的所有文件中查找特定字符串，可以用下面的快捷键来打开搜索面板：

- `Ctrl + Shift + F (Windows, Linux)`
- `Cmd + Opt + F (Max OSX)`

这里支持正则表达式以及区分大小写的搜索。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_findall.jpg)

###用正则表达式进行搜索

如需用正则表达式进行搜索，仅需在搜索框中输入表达式，并勾选“正则表达式(Regular expression)”然后回车。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_regex.jpg)

上图是一个查找所有匹配在`<div></div>`标签内容中的例子。

###根据函数或选择器在文件内查找

如果需要更细粒度的操作，也可以根据函数名或CSS规则在文件中定位或者查找。

打开一个页面并切换至Sources面板，然后用下面的快捷键打开函数/选择器搜索框。

- `Ctrl + Shift + O (Windows, Linux)`
- `Cmd + Shift + O (Mac OSX)`

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/function_filter.png)

根据选择的文件类型，将会看到所有可用的JavaScript函数或者CSS声明。输入函数名或声明来过滤结果列表，或仅是选择一个结果来定位到其在文件中定义的位置。

###跳至行号

工具同样支持在编辑器内跳到指定行号。打开一个文件至编辑状态， 然后用下面的快捷键显示行号跳转框。

- `Ctrl + L (Windows)`
- `Cmd + L (Mac OSX)`
- `Ctrl + G (Linux)`

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_line.jpg)

##即时编辑脚本与样式

“开发者工具”支持即时编辑样式与脚本，并且不需要刷新整个页面。这在测试设计改变、调试脚本或代码片段时很有用。

###脚本

JavaScript可以在“开发者工具”的“Sources”面板里直接编辑。要打开一个脚本进行编辑可以：

1.点击“Elements”面板中结构视图里的脚本链接（例如`<script src="app.js"></script>`）：

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/styles_select.jpg)

2.或者在“Sources”面板里的“Sources”子面板中选择一个文件名：

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/styles_sources.jpg)

这将会在右侧打开一个新的标签显示语法高亮后的源代码。

修改的脚本仅在evaluation time执行，这意味着修改页面加载前或加载过程中执行的代码将没有效果。对于之后将会执行的代码的修改可以直接进行测试，例如鼠标移过或者点击时的回调函数。

想了解在“Sources”面板中调试JavaScript的更多信息，请阅读这篇相关文章：[JavaScript Debugging](https://developer.chrome.com/devtools/docs/javascript-debugging.html).

> 注意：Workspaces 功能允许持久地编辑（保存）本地文件，阅读[更多](https://developer.chrome.com/devtools/docs/workspaces.html)。

###样式

与编辑脚本类似，打开“开发者工具”并切换至Elements面板，在右边可以看到包括“Styles”在内的一些子面板。查看页面中元素的时候，将会在“Styles”子面板里显示由选择器分组的应用到当前节点的属性列表。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/styles_inspect.jpg)

“element.style”部分显示的是通过style属性设置在页面标记里的样式。

下面是“匹配的CSS规则(Matched CSS Rules)”，显示了匹配到选中节点的选择器、它们的属性与值，和定义规则的文件名与具体行数。匹配到选中节点的选择器会显示黑色，未匹配的则是灰色。这样做很大的好处是使得在阅读的时候更容易进行区分。

在子面板中改变任何CSS属性均会立即生效并在浏览器主窗口中显示，比如元素的边框或尺寸。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/styles_edit.png)

回到“匹配的CSS规则(Matched CSS Rules)”面板，点击规则旁边的样式文件链接将会转至“Source”面板，这里显示了完整的样式并会定位到相关CSS规则的行数。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/matched_css.png)

在这里可以像在一个普通的文本编辑器中一样做出修改，但区别是修改会立即生效。

##另存

修改妥当后，可以将修改后的文件保存下来。

如果需要这样做，首先确定正处在所修改文件的文本编辑视图，无论是通过在Sources选项下左边栏手动定位文件，

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/saveas_select.jpg)

还是在Elements的Style面板里点击文件名（比如styles.css，仅限SASS/CSS）。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/matched.png)

下一步，在文件名或者文本编辑器内单击右键，选择“另存为(Save as)”，这时候将会弹出一个对话框允许你覆盖存在的文件。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/saveas_saveas.jpg)

后续的保存（从相同菜单中选择“保存(Save)”，或使用`Ctrl/Cmd + S`快捷键）将保存在相同的位置。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/saveas_save.jpg)

##本地修改

“开发者工具”也会保存本地文件的所有修改记录。如果你编辑了一个脚本或者样式并进行了保存，那么可以在Sources面板里的文件名上单击右键（或者在源代码区域），然后选择“本地修改(Local modifications)”来查看历史记录。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/saveas_localmodifications.jpg)

出现的本地修改(Local modifications)面板将会显示：

- 修改内容的差异；
- 修改的时间；
- 修改的文件名和一些操作链接。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/saveas_history.jpg)

“还原(revert)”链接将会还原所有对文件的修改至初始状态，并删除历史记录。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/saveas_changed.jpg)

“恢复原始内容(apply original content)”将会完成相同的动作，但是会在视图里保留历史记录，以便于还原某个特定的修改。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/saveas_changed.jpg)

最后，“应用修改的内容(apply revision content)”将会使在某个时间发生的修改生效。

##自定义JavaScript片段

有时候你可能想保存一些小脚本、书签和工具，以便在浏览器中调试的时候使用。代码片段(Snippets)是“开发者工具”中实现这些操作的一个新功能，它允许你在Sources标签页下创建、存储和运行脚本。目前在[Chrome Canary](https://tools.google.com/dlpage/chromesxs)中可用。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_hero.jpg)

一些可以用得上“代码片段(Snippets)”的场景：

- **Bookmarklets** - 所有的Bookmarklets都可以被存储为代码片段，尤其是那些你希望编辑的；
- **工具** - 一些在当前页面使用的调试代码可以保存起来并用作调试。一个由社区维护的[列表](https://github.com/paulirish/devtools-addons/wiki/Snippets)提供了这样一些工具。
- **调试** - “代码片段”提供一个可以代码高亮和保存代码的多行编辑的控制台，使其调试多行代码时更加方便。
- **动态修改代码(Monkey-patching code)** - 可以通过“代码片段”在运行时修改代码，虽然大多数时候可以在Sources标签里动态编辑。

Brian Grinstead在[bgrins.github.io/devtools-snippets](bgrins.github.io/devtools-snippets)维护了一些有用的代码片段可供开发者使用。

###开始使用

定位到Sources标签以开始使用“代码片段”。如果此时还未在此标签页里做其它事情，你会看到如下所示的默认界面。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_default.jpg)

点击左上角的布局切换显示扩展的面板。现在应该可以看到源码(Sources)、内容脚本(Content Scripts)和一个新的标签——代码片段(Snippets)，点击进入。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_expand.jpg)

###创建代码片段

代码片段有两部分面板，左边的（类似Sources面板）是文件列表，当选中文件的时候会在右侧的编辑器中打开。这与在Sources面板中选择样式表和脚本的操作十分类似。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_creating.jpg)

在文件列表单击右键，选择“新建(New)”将会创建一个新的代码片段文件。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_new.png)

###代码片段的文件名

代码片段的文件名是自动生成的，但当代码片段创建后你可以选择自定义文件名。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_filename.png)

如希望重命名，则可以在文件上单击右键，然后选择“重命名(Rename)”。如果需要的话，也可以选择“删除(Remove)”代码片段。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_remove.png)

###编辑与执行代码片段

在文件列表中选择一个文件在右侧内建的编辑器中打开。这里你可以输入或粘贴任何JavaScript代码（例如你的代码片段），包括函数与表达式。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_editor.png)

如果一个文件名以“*”开头，意味着这个文件已被修改但并未保存。

如需运行一个代码片段，在列表里的文件名上单击右键然后选择“运行”。或者点击Run(>)按钮。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_run.png)

如果有任何控制台输出，则会显示在编辑器下方的控制台中。

> **注意**：也可以使用快捷键来快速执行代码片段，选择后按Ctrl/Cmd + Enter来运行。这与点击Run(>)按钮（目前在Sources控制台中，但将来会移至调试控制中）的作用相同。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_console.png)

如果只想运行代码片段的其中几行，可以选择需要运行的部分，然后单击右键选择“在控制台中运行(Evaluate in Console)”选项来执行。快捷键是`Ctrl + Shift + E`。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_evaluate.png)

与“运行(Run)”一样，表达式的输出可以在编辑器下方的控制台中查看。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_evaluated.png)

###本地修改

如源(Sources)一样，代码片段(Snippets)同样支持查看对文件做出的本地修改，也可以在历史记录里选择还原至某一次编辑。

在编辑器内单击右键，然后选择“本地修改(Local modifications)”，在保存了一些本地编辑后便可以使用该功能。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_local.png)

###断点、监视表达式以及更多

Sources面板中的一些功能同样也可以在代码片段(Snippets)中使用，比如添加表达式监视、断点、局部变量与保存文件。

您可以阅读关于Sources面板的文档来了解更多关于这些功能如何使用的信息。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/sources_breakpoints.jpg)

###保存代码片段

代码片段保存后可以通过“开发者工具”的代码片段(Snippets)标签来查看，或者导出为一个新的文件。如需显示保存选项，在文本编辑器内单击右键显示编辑菜单。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippets_contextmenu.png)

这里“保存(Save)”将会保存修改至现存的代码片段文件，而“另存为(Save as)”允许你将代码片段保存为一个新的文件至所选择的地方。

> 注意：代码片段保存在“开发者工具”的本地存储内。保存/另存后，可将代码片段绑定至其应被保存的位置，就像其它脚本一样。

###定位代码片段

与Sources面板中的脚本和样式类似，代码片段同样支持定位到某个特定的文件、函数或者行号，快捷键与之前提到的相同。

![](https://developer.chrome.com/devtools/docs/authoring-development-workflow/snippet_filter.png)

##参考资料
- [Chrome DevTools Revolutions 2013: Workspaces](http://www.html5rocks.com/en/tutorials/developertools/revolutions2013/#toc-workspaces)
- [My workflow: Never having to leave the DevTools | remy sharp](http://remysharp.com/2012/12/21/my-workflow-never-having-to-leave-devtools/)
- [The Breakpoint With Addy Osmani And Paul Lewis - Snippets | youtube](http://www.youtube.com/watch?feature=player_detailpage&v=ktwJ-EDiZoU#t=553s)
- [Chrome Dev Tools: JavaScript and Performance | nettuts](http://net.tutsplus.com/tutorials/tools-and-tips/chrome-dev-tools-javascript-and-performance/)
- [Iterating with the Chrome DevTools | jeremey kahn](http://www.youtube.com/watch?v=Phw6edMppKg)
