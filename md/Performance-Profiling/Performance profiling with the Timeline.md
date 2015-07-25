使用Timeline做性能分析
***
Timeline面板可以让你记录和分析web应用运行时的所有活动，这是你研究和查找性能问题的最佳途径。  
###Timeline面板概览
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
事件模式提供以类型组织的记录期间捕获的所有事件的概览。大致能看出你的应用在什么类型的任务上的主要的时间消耗。这个视图中的水平条的长度对应于事件完成花费的时间长度。  
![](https://developer.chrome.com/devtools/docs/timeline-images/events_mode.png)  
当你在事件视图选择一个时间段（参考[放大时间轴](https://developer.chrome.com/devtools/docs/timeline#zooming-in-on-a-timeline-section)），记录视图只展示对应时间段的记录。  
![](https://developer.chrome.com/devtools/docs/timeline-images/timeline_records.png)
#####帧模式
帧模式提供洞察你的应用的渲染性能的能力。“帧”代表浏览器渲染一帧要显示的内容必须要做的工作--运行JavaScript、处理事件、更新DOM、改变样式布局和绘制页面。你的应用的目标是运行在每秒60帧下，对应于大多数（但不是全部）视频显示器的60Hz的刷新速率。因此，你的应用程序有大约16.6毫秒（1000毫秒/60）对每一帧做准备。  

贯穿帧视图的水平线呈现60FPS和30FPS的帧速率目标。帧的高度对应于该帧渲染所花费的时间。每帧填充的颜色表明了每种类型的任务所花费的时间百分比。

渲染每帧花费的时间显示在记录试图的顶部。如果你把鼠标悬停在显示时间上，会显示帧的附属信息，包括每种类型的任务的时间、CPU时间、计算FPS的时间。
![](https://developer.chrome.com/devtools/docs/timeline-images/frames_mode.png)  
参考[使用帧模式诊断和修复强制同步布局](https://developer.chrome.com/devtools/docs/demos/too-much-layout/)的示例。
######关于透明或浅灰色的帧
也许你已经注意到有些区域的帧是浅灰色的或者透明的（空的）。这些区域分别表明：
* DevTools不仪表化的活动
* 显示刷新周期之间的空闲时间

下面的帧中展示了不仪表化的活动和空闲时间。  
![](https://developer.chrome.com/devtools/docs/timeline-images/clear-frames.png)  
######关于绿色条
绘图是一个两步的过程，包括：绘制调用和光栅扫描。  
* 绘制调用。 这是你想要绘制的东西的列表，它来自于你的元素的CSS应用。最后还有一些绘制调用和画布上的元素不同：moveTo, lineTo, and fillRect。尽管在[Skia](https://code.google.com/p/skia/)和Chrome绘制后端，他们有着不同的名字，但它们是类似的概念。
* 栅格化。

######Viewing frame rate statistics

#####内存模式

#####Making a recording

######Recording a page load

######Tips for making recordings


###Analyzing Timeline recordings

#####Viewing details about a record

#####DOMContentLoaded and Load event markers

#####Locating forced synchronous layouts

#####About nested events

#####Filtering and searching records

#####Zooming in on a Timeline section

#####Saving and loading recordings

#####User-produced Timeline events

#####View CPU time in recordings


###Timeline event reference

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
