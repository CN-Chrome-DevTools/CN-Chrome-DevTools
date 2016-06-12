#Tips and Tricks(小技巧)

##console(控制台)
###运行多行的命令
`Shift` + `Enter` 允许您在控制台输入多行。
![编写多行的javascript程序](https://developer.chrome.com/devtools/docs/tips-and-tricks/consolemultiline.png)
![编写多行的javascript程序](https://developer.chrome.com/devtools/docs/tips-and-tricks/consolerun.png)

> [Snippets](https://developer.chrome.com/devtools/docs/authoring-development-workflow.html#snippets)了解更多关于多行代码持久使用的方法。这个功能可以执行自定义的javascript片段，并将其存储在DevTools中。

###启动元素审查的快捷方式

`Ctrl` + `Shift` + `C` 或 `Cmd` + `Shift` + `C` 将快速启动 `DevTools` 检查元素的模式。

![快速启动审查元素的模式](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_10.png)

[更多关于Console|Devtools Doc](https://developer.chrome.com/devtools/docs/console.md)

###支持 console.table 命令
使用如下：
``` javascript
console.table([{a:1, b:2, c:3}, {a:"foo", b:false, c:undefined}]);
console.table([[1,2,3], [2,3,4]]);
```
![console.table 命令](https://developer.chrome.com/devtools/docs/tips-and-tricks/consoleg1.png)

还有一个可选的 `columns` 参数来选择需要输出的列。
如下：
``` javascript 
function Person(firstName, lastName, age) {
  this.firstName = firstName;
  this.lastName = lastName;
  this.age = age;
}

var family = {};
family.mother = new Person("Susan", "Doyle", 32);
family.father = new Person("John", "Doyle", 33);
family.daughter = new Person("Lily", "Doyle", 5);
family.son = new Person("Mike", "Doyle", 8);

console.table(family, ["firstName", "lastName", "age"]);
```
![console.table 命令](https://developer.chrome.com/devtools/docs/tips-and-tricks/consoleperson.png)
如果只想输出出前两列，可以：
``` javascript
console.table(family, ["firstName", "lastName"]);
```
###预览在控制台打印出的对象
可以直接使用console.log()预览打印出的对象，不需要做额外的工作。

![预览打印出的对象](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_12.png)

###传递多个参数控制打印日志的样式
你可以通过 `%c` 将样式添加到您的控制台日志,就像你可以在Firebug中做的一样。如：

``` javascript
console.log('%cBlue! %cRed!', 'color: blue;', 'color: red;');
```
![酷酷的控制台日志啊](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_13.png)

[More: Styled Console Logging In The DevTools | G+](https://plus.google.com/115133653231679625609/posts/TanDFKEN9Kn)

###清空控制台历史的快捷键
`Ctrl` + `L` and `Cmd` + `L` [快捷键](https://developer.chrome.com/devtools/docs/shortcuts.html). 在 **shell** 中 `clear()` 命令也可以。 javascript中的[console.clear()](https://developer.chrome.com/devtools/docs/console.md#clearing-the-console-history)也是可以的。

###做一个键盘侠

`Devtools` 打开并获得焦点时时，`Shift` + `?` 命令打开一个设置面板，选择 `shortcuts` 可以看到所有可用的[快捷键](https://developer.chrome.com/devtools/docs/shortcuts.html)

![查看快捷键](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_14.png)

###从控制台快速的选择元素

Select an element and type $0 in the console, it will be used by the script. If you have jQuery on the page, you can then use $($0) to reselect the element in the page.

![快速选择元素](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_15.png)

You can also right click on any element output to the console and click 'Reveal in Elements Panel' to find it in the DOM.

![快速选择元素](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_16.png)

###用XPath表达式选择DOM
XPath is a query language for selecting nodes from documents and generally returns a node-set, string, boolean or number. You can use XPath expressions to query the DOM from the DevTools JavaScript console.

The $x(xpath) command will allow you to execute a query - see below for an example of how to search for the images in a page using $x('//img'):

![利用XPath选择元素](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_17.png)

However, the function also accepts an optional second argument for the path context - i.e $x(xpath, context). This lets us select a specific context (e.g an iframe) and run an XPath query against it.

```javascript
var frame = document.getElementsByTagName('iframe')[0].contentWindow.document.body;
$x('//'img, frame);
```
which queries the images within the specified iframe.

###得到控制台最后一个输出的结果
使用 `$_`  得到控制台最后一个输出的结果.

![得到控制台最后一个输出的结果](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_17a.png)

###console.dir()
[console.dir(object)](https://developer.chrome.com/devtools/docs/console-api.md#consoledirobject) 提供的命令可以列出一个JavaScript对象所有属性。

![console.dir()](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_18.png)

###在指定的 iframe 中运行控制台中命令
Along the bottom bar of the DevTools are drop-down options that change depending on the context of your current tab. When you’re in the Console panel, there’s a drop-down that allows you to select the frame context that the console will operate in. Select your frame in the drop-down and you’ll find yourself in the right context in no time.

![在指定的 iframe 中运行控制台中命](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_19.png)

###当浏览器切换到另一个页面时保存控制台中历史
在控制台版面右键，选择 `save` 即可

![保存控制台的历史](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_20.png)

### console.time() 和 console.timeEnd()
可以 `console.time()` 和 `console.timeEnd()` 计算程序运行的时间.如下：
![console.time() 和 console.timeEnd()](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_21.png)

### console.profile() 和 console.profileEnd()
DevTools打开时,调用 `console.profile()` JavaScript CPU的分析。`console.profileEnd()` 结束此次分析。如下：

![console.profile() 和 console.profileEnd()](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_22.png)

每次分析都被加到 `Profiles`面板和 `console.profilses'中:

![console.profile() 和 console.profileEnd()](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_23.png)

![console.profile() 和 console.profileEnd()](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_24.png)

** 更多使用控制台的技巧见[using the console](https://developer.chrome.com/devtools/docs/console.md)**:
* [Console Api](https://developer.chrome.com/devtools/docs/console-api.md)
* [Command Line Api](https://developer.chrome.com/devtools/docs/commandline-api.md)

![using the console](https://developer.chrome.com/devtools/docs/tips-and-tricks/preview_console.png)

##Elements
###打开尺标线
打开 **Devtools**  `Settings` > `General` > `Show rulers`即可

![Show rulers](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_53.png)

###css 属性的自动补全

![css](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_55.png)

![css](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_56.png)

![css](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_57.png)

###颜色选择器
在Devtools中点击颜色时可以调出一个颜色选择器。用 `shift` + 'click' 可以改变颜色的样式。

![css](https://developer.chrome.com/devtools/docs/tips-and-tricks/colorpickercanary.png)

###新增CSS 样式


###在Elements面板中拖曳元素
在Elements中拖曳元素可以改变该元素在文档流中的未知和显示的样式。

![在Elements面板中拖曳元素](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_66.png)

###改变元素的状态
* Right click on a child element and select 'Inspect Element'
* In the Elements panel right-click the parent element and choose "Force Element State"
* You can now choose one of :active, :hover, :focus or :visited to force the element into one of these states.

![改变元素的状态](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_67.png)

###调试 less 或者 sass 代码

`Settings` > `General` > `Sources` > `Enable CSS source maps`

![enable source](https://developer.chrome.com/devtools/docs/tips-and-tricks/autoreload.png)

** 更多关于样式和DOM操作的技巧[Editing Styles And The DOM](https://developer.chrome.com/devtools/docs/dom-and-styles.html)**

![dom and styles](https://developer.chrome.com/devtools/docs/tips-and-tricks/preview_elements.png)

##Timeline














