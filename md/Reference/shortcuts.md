快捷键
===========

DevTools具有一些内建的快捷键，开发者可以在日常的开发过程中使用它们以节约时间。以下列举的是每个快捷方式在Windows / Linux和Mac下相应的快捷键。有些快捷键是在全局有效的，而有些只是在某一个面板生效，and are broken up based on where it can be used.

####打开DevTools


在Google Chrome的任何页面或者应用中，你可以通过以下的方式打开 DevTools:

*	打开浏览器窗口右上方的 Chrome 菜单 img，选择**工具 > 开发者工具(Tools > Developer Tools)**.
*	在任意的页面元素中鼠标右键，选择**审查元素(Inspect Element)**.

功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
打开 Chrome DevTools          | F12, Ctrl + Shift + I   | Cmd + Opt + I
打开／切换 审查元素模式和浏览模式   | Ctrl + Shift + C   | Cmd + Shift + C
打开 Chrome DevTools ,并聚焦在 console 上 | Ctrl + Shift + J   | Cmd + Opt + J
审查审查器 (取消第一个审查器的停靠后再按键)  | Ctrl + Shift + J   | Cmd + Opt + J

在Chrome DevTools 窗口是打开时，按下“?”或者“F1”键可以打开设置对话（General Settings）框。按下“Esc”可以关闭设置对话框。

####全部面板

功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
打开 General Settings 对话框   |  ?, F1  |  ?
下一个面板 |  Ctrl + ]  |   Cmd + ]
上一个面板 |  Ctrl + [  |   Cmd + [
标签历史中后退    |  Ctrl + Alt + [    | Cmd + Alt + [
标签历史中前进    |   Ctrl + Alt + ]    | Cmd + Alt + ]
跳转至标签页 1-9 (需要在设置对话框中开启服务)    |   Ctrl + 1-9    | Cmd + 1-9
打开/关闭 Console 或  关闭设置对话框   |   Esc  |  Esc
刷新页面     |  F5, Ctrl + R     |  Cmd + R
强制刷新页面，清除缓存内容    |   Ctrl + F5, Ctrl + Shift + R   | Cmd + Shift + R
当前文件或标签页搜索文字   |   Ctrl + F  |     Cmd + F
所有资源中搜索文字 |  Ctrl + Shift + F    |   Cmd + Alt + F
搜索文件(除了 Timeline面板)  |  Ctrl + O, Ctrl + O  |   Cmd + O, Cmd + O
恢复默认字体大小  |    Ctrl + 0   |    Shift + 0
放大  |  Ctrl + +     |  Shift + +
缩小   |    Ctrl + -     |  Shift + -


####元素(Elements) 面板

功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
撤销改动 | Ctrl + Z    | Cmd + Z
恢复改动 | Ctrl + Y    | Cmd + Y, Cmd + Shift + Z
导航    |  Up, Down   |  Up, Down
伸缩展开元素  | Right, Left | Right, Left
展开元素 | 单击某个html标签 | 单击某个html标签
编辑元素属性|   Enter, 双击属性 |   Enter, 双击属性
隐藏元素   |  H |   H
切换编辑为HTML|  F2|

右键单击某个元素时你可以做：
*   强制元素在某个伪类状态: (:active, :hover, :focus, :visited)
*	为元素设置断点（子元素修改，属性更改，元素删除）
*	清除 console

####样式(Styles) 侧边栏

功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
编辑规则   | 单击  |   单击
添加新属性|  空白处单击   | 空白处单击
跳转到样式规则属性在样式表的行数|  Ctrl + 单击某个CSS属性 |   Cmd + 单击某个CSS属性
跳转到属性值在样式表的行数 |  Ctrl + 单击某个CSS属性值 |  Cmd + 单击某个CSS属性值
循环颜色定义值|  Shift + 单击颜色选择器  | Shift + 单击颜色选择器
查看自动填充建议 |  Ctrl + 空格 |   Cmd + 空格
编辑下一个 / 上一个属性  |  Tab, Shift + Tab  |   Tab, Shift + Tab
增大 / 减小属性值 | Up, Down   |  Up, Down
增大 / 减小属性值 （最小单位 10 ） |  Shift + Up, Shift + Down  |   Shift + Up, Shift + Down
增大 / 减小属性值 （最小单位 10 ） |  PgUp, PgDown  |   PgUp, PgDown
增大 / 减小属性值 （最小单位 100）  | Shift + PgUp, Shift + PgDown    | Shift + PgUp, Shift + PgDown
增大 / 减小属性值 （最小单位 0.1）  | Alt + Up, Alt + Down  |   Opt + Up, Opt + Down

(img)[img]仿真元素伪类 (:active, :hover, :focus, :visited)

(img)[img]添加新的CSS样式规则

####资源(Sources)面板


功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
中断/恢复脚本执行 | F8, Ctrl + \    | F8, Cmd + \
跳过下一个函数   | F10, Ctrl + '   | F10, Cmd + '
跳入下一个函数    | F11, Ctrl + ;   | F11, Cmd + ;
跳出当前函数    | Shift + F11, Ctrl + Shift + ; |   Shift + F11, Cmd + Shift + ;
Select next call frame |  Ctrl + .  |   Opt + .
Select previous call frame |  Ctrl + ,  |   Opt + ,
切换断点状态 | 单击行数, Ctrl + B |  单击行数, Cmd + B
编辑断点调节  |  右键单击行数 |  右键单击行数
Delete individual words | Alt + Delete  |   Opt + Delete
注释某行或选择文字  | Ctrl + /   |  Cmd + /
保存本地的更改| Ctrl + S  |   Cmd + S
保存所有的更改| Ctrl + Shift + S  |   Cmd + Shift +  S
跳转到某行 |  Ctrl + G  |   Ctrl + G
按文件名搜索文件|  Ctrl + O  |   Cmd + O
跳转到某行（Jump to line number） | Ctrl + P + :<number>   |  Cmd + P + :<number>
跳转到某列 | Ctrl + O + :<number> + :<number>   |  Cmd + O + :<number> + :<number>
打开 member  |   Ctrl + Shift + O  |   Cmd + Shift + O
切换 console 并评估（ evaluate？） Sources 面板中选中的代码|  Ctrl + Shift + E   |  Cmd + Shift + E
关闭当前激活的标签 | Alt + W   |  Opt + W
运行代码片段 | Ctrl + Enter   |  Cmd + Enter
切换注释  | Ctrl + /   |  Cmd + /

(img)[IMG] Don't 暂停 on exceptions

(img)[IMG] 暂停 on All Exceptions  (包括那些在try/catch块中被捕获的)

(img)[IMG] 暂停 on Uncaught Exceptions  (正常情况下是你想要的那个)

####Timeline Panel


功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
开启/停止   记录 |  Ctrl + E  |   Cmd + E
保存时间轴数据 |  Ctrl + S  |   Cmd + S
加载时间轴数据 | Ctrl + O   |  Cmd + O

####Profiles Panel


功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
开启/停止   记录 |  Ctrl + E  |   Cmd + E

####Console


功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
下一个建议| Tab | Tab
上一个建议 | Shift + Tab | Shift + Tab
接受建议 |  Right |   Right
上一个命令/行 | Up |  Up
下一个命令/行 | Down  |   Down
清除控制台记录   | Ctrl + L   |  Cmd + K, Opt + L
多行输入   |  Shift + Enter  |  Ctrl + Return
执行|  Enter  |  Return

console 中右键单击:

*	XMLHTTPRequest 记录: 打开后可查看 XHR 记录
*	Preserve log upon navigation
*	过滤: 隐藏或显示所有来自脚本文件的消息
*	清除 console: 清除所有的 console 消息


####Screencasting

功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
视图微调 放大或缩小 | Alt + Scroll,Ctrl + Cick and drag with two fingers  |Opt + Scroll, Cmd + Cick and drag with two fingers
审查元素工具 |  Ctrl + Shift + C   | Cmd + Shift + C

####模拟


功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
视图微调 放大或缩小  | Shift + Scroll | Shift + Scroll

####其他 Chrome 快捷键

以下的 Chrome 快捷键在日常使用中非常有用，它并不是特意为 DevTools开发的，[查看所有的 Chrome 快捷键](https://support.google.com/chrome/answer/157179?hl=en&topic=25799&rd=1).

功能       | Windows / Linux | Mac
:------------- | :--- | :-------------------------------------
寻找下一个   |Ctrl + G    |Cmd + G
寻找前一个  | Ctrl + Shift + G  |  Cmd + Shift + G
在隐身模式下打开一个新窗口|Ctrl + Shift + N   | Cmd + Shift + N
开启或关闭书签栏|Ctrl + Shift + B  |  Cmd + Shift + B
查看历史记录   |Ctrl + H   | Cmd + Y
查看下载记录 |Ctrl + J  |  Cmd + Shift + J
查看任务管理器   |Shift + ESC |Shift + ESC
标签浏览历史中的下一个页面  |  Alt + Right |Alt + Right
标签浏览历史中的上一个页面 |  Backspace, Alt + Left   |Backspace, Alt + Left
高亮地址栏内容 | F6, Ctrl + L, Alt + D  | Cmd + L, Alt + D
在地址栏输入一个 ? 后可以将它作为你的默认搜索引擎使用 |Ctrl + K, Ctrl + E  |Cmd + K, Cmd + E
