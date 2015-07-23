使用Timeline做性能分析
***
Timeline面板可以让你记录和分析web应用运行时的所有活动，这是你研究和查找性能问题的最佳途径。  
####Timeline面板概览
Timeline面板主要有三个部分构成：顶部的概述部分、记录视图和工具栏。  
![](https://developer.chrome.com/devtools/docs/timeline-images/timeline_ui_annotated.png)
* 点击开始/停止切换按钮，开始或停止记录（参考[记录](https://developer.chrome.com/devtools/docs/timeline#making-a-recording)）
* 点击清理按钮来从Timeline中清除已有的记录
* 关联异步事件模式让你更容易的关联异步事件和这些异步事件的调用（参考[嵌套事件](https://developer.chrome.com/devtools/docs/timeline#about-nested-events)）
* 你可以根据记录中的类型、执行时长去过滤Timeline中的记录（参考[过滤和搜索记录](https://developer.chrome.com/devtools/docs/timeline#filtering-and-searching-records)）

在记录期间，每个事件以“瀑布”的形式记录在记录视图中。记录被分为四个基本组之一：加载、脚本执行、渲染和绘图。这些记录以颜色区分，如下：  
![](https://developer.chrome.com/devtools/docs/timeline-images/image01.png)

以chrome加载一个html页面的记录为例。第一条记录（发送请求）是来自chrome的获取页面的HTTP请求，紧接着是一条接收响应的记录（获取响应的HTTP响应）、一些接收数据的记录（来自页面的数据），然后是完成加载的记录。Timeline的一个完整的时间记录和描述参考[Timeline事件参考](https://developer.chrome.com/devtools/docs/timeline#timeline-event-reference)。  
![](https://developer.chrome.com/devtools/docs/timeline-images/image06.png)  
当你把鼠标悬停在Timeline的一条记录上时，弹出框显示关联时间的详细信息。比如下面的截图展示加载一张图片资源的细节。[Timeline事件参考](https://developer.chrome.com/devtools/docs/timeline#timeline-event-reference)解释了适用于每一个记录类型的详细情况。  
![](https://developer.chrome.com/devtools/docs/timeline-images/image12.png)  

在记录试图除了详情，你可以在以下三种模式下检查记录：  
* 事件模式 以事件分类展示所有事件记录
* 帧模式 展示页面的渲染性能
* 内存模式 展示随着时间的推移内存使用情况

#####事件模式
######About clear or light-gray frames

######About the green bars

######Viewing frame rate statistics

#####帧模式

#####内存模式

#####Making a recording

######Recording a page load

######Tips for making recordings


####Analyzing Timeline recordings

#####Viewing details about a record

#####DOMContentLoaded and Load event markers

#####Locating forced synchronous layouts

#####About nested events

#####Filtering and searching records

#####Zooming in on a Timeline section

#####Saving and loading recordings

#####User-produced Timeline events

#####View CPU time in recordings


####Timeline event reference

#####Common event properties

#####Loading events

#####Loading event properties

#####Scripting events

#####Scripting event properties

#####Rendering events

#####Rendering event properties

#####Painting events

#####Painting event properties





***
Content available under the [CC-By 3.0 license](http://creativecommons.org/licenses/by/3.0/)
