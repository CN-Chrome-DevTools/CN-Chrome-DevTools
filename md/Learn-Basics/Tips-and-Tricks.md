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

还有一个可选的 `columns` 参数来选择。






















