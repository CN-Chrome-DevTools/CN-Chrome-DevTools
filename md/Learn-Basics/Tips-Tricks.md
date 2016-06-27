#小窍门

##控制台

####写跨行命令

处于控制台的多行编辑模式时，你可以像在一个标准文本编辑器中那样进行文本块的处理。在控制台中你可以通过**Shift + Enter**可进入跨行模式。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/consolemultiline.png)

当写一段比单行程序复杂得多的JavaScript程序时这非常有用。当你完成一个文本块的输入，在命令行的末尾按回车键即可运行。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/consolerun.png)

关于支持持久化的多行控制台，阅读[Snippets](https://developer.chrome.com/devtools/docs/authoring-development-workflow.html#snippets)-一个可以保存和执行客户端JavaScript代码片段的功能(该功能可以在开发者工具中找到)。

####开启元素查看模式的快捷键

**Ctrl + Shift + C**或者**Cmd + Shift + C**将在元素查看模式中打开开发者工具(开发者工具已经打开的情况下将切换到元素查看模式)，以便你可以马上查看当前网页的元素。再次按快捷键将把焦点移到网页上(如果你使用的事Mac，快捷键为Cmd + Shift + C)。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_10.png)

[更多:使用控制台 | 开发者工具文档](https://developer.chrome.com/devtools/docs/console.md)

####支持console.table命令

该命令以表格形式打印任何数据。有几个如何使用它的例子:

    console.table([{a:1, b:2, c:3}, {a:"foo", b:false, c:undefined}]);
    console.table([[1,2,3], [2,3,4]]);

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/consoleg1.png)

还有一个可选的“列”参数，该参数为字符串数组的形式。下面，我们定义一个包含多个人的家庭类，然后使用该参数选择展示哪些列。

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

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/consoleperson.png)

假如你只想输出前列，使用:

    console.table(family, ["firstName", "lastName"]);

[更多:关于console.table命令 | G+](https://plus.google.com/u/0/115133653231679625609/posts/PmTC5wwJVEc)

####预览在控制台打印出来的对象

在开发者工具中可以直接查看使用console.log()打印出来的对象，不用做额外的工作。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_12.png)

####传入多个参数来格式化控制台输出

正如我们已经用文档说明过的，你可以通过**%c**给你的控制台输出添加样式，就像在firebug中所能做的那样。

    console.log("%cBlue!", "color: blue;");

还支持给多块文本指定不同样式:

    console.log('%cBlue! %cRed!', 'color: blue;', 'color: red;');

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_13.png)

[更多:开发者工具中的样式化的控制台打印 | G+](https://plus.google.com/115133653231679625609/posts/TanDFKEN9Kn)

####清除控制台历史记录的快捷键

控制台打开状态下，你可以使用[快捷键](https://developer.chrome.com/devtools/docs/shortcuts.html)**Ctrl + L**或者**Cmd + L**轻易地清除历史记录。在提示符中使用clear()命令，就像通过控制台API使用[ console.clear()](https://developer.chrome.com/devtools/docs/console.md#clearing-the-console-history)一样。

####成为一个键盘忍者

在开发者工具开启状态下，你可以仅仅通过“?”来打开通用设置，然后你可以找到快捷键面板来通览所有[支持的快捷键](https://developer.chrome.com/devtools/docs/shortcuts.html)。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_14.png)

####在控制台中访问元素

选择一个元素，然后在控制台中输入$0，它就可以被用于写脚本了。如果你在网页中引入了jQuery，你可以使用$($0)来重选网页中该元素。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_15.png)

你也可以通过在任意输出到控制台元素上点击鼠标右键然后选择“Reveal in Elements Panel”来在DOM面板中定位该元素。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_16.png)

####使用XPath表达式查询DOM

xPath是一种查询语言，它被用来从文档中选择节点并返回一个节点集合、字符串、布尔值或数字。你可以在调试工具的JavaScript控制台中使用xPath表达式来查询DOM。

$x(xpath)命令允许你执行查询。看下面使用$x('//img')查询图片元素的例子：

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_17.png)

这个函数还接收第二个可选的参数，该参数指定xpath的上下文。$x(xpath, context)。这可以允许我们在一个特定上下文(比如一个iframe)内运行XPath进行元素选择。

    var frame = document.getElementsByTagName('iframe')[0].contentWindow.document.body;
    $x('//'img, frame);

这是一个在特定的上下文中查询图片元素的例子。

####访问最近的控制台结果

使用$_可以帮你快速查看最近一次控制台结果。我们使用另一个xpath示例来演示一下:

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_17a.png)

####使用console.dir

[console.dir(object)](https://developer.chrome.com/devtools/docs/console-api.md#consoledirobject)命令将以可展开的JavaScript对象的形式罗列出给定的对象的所有属性。下面是一个展示可展开的对象的例子，该对象代表document.body所有的属性。

![](https://developer.chrome.com/devtools/docs/tips-and-tricks/image_18.png)

####在特定的iframe中运行JS控制台

(尚未翻译完，待续……)
