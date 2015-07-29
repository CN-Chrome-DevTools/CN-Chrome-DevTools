使用Timeline做性能分析
***
Timeline面板记录和分析了web应用运行时的所有活动情况，这是研究和查找性能问题的最佳途径。  
###Timeline面板概览
Timeline面板主要有三个部分构成：顶部的概述部分、记录视图和工具栏。  
![](https://developer.chrome.com/devtools/docs/timeline-images/timeline_ui_annotated.png)
* 点击开始/停止切换按钮，开始或停止记录（参考[记录](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#记录)）
* 点击清理按钮来从Timeline中清除已有的记录
* 关联异步事件模式让你更容易的关联异步事件和这些异步事件的调用（参考[关于嵌套事件](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#关于嵌套事件)）
* 你可以根据记录中的类型、执行时长去过滤Timeline中的记录（参考[过滤和搜索记录](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#过滤和搜索记录)）

在记录期间，每个事件以“瀑布”的形式记录在记录视图中。记录类型有以下四种：加载、脚本执行、渲染和绘制。这些记录以颜色区分，如下：  
![](https://developer.chrome.com/devtools/docs/timeline-images/image01.png)  
以chrome加载一个html页面的记录为例。第一条记录（发送请求）是来自chrome的获取页面的HTTP请求，紧接着是一条接收响应的记录（获取响应的HTTP响应）、一些接收数据的记录（来自页面的数据），然后是完成加载的记录。Timeline的一个完整的时间记录和描述参考[Timeline事件参考](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#timeline事件参考)。  
![](https://developer.chrome.com/devtools/docs/timeline-images/image06.png)  
当你把鼠标悬停在Timeline的一条记录上时，弹出框显示关联事件的详细信息。比如下面的截图展示加载一张图片资源的细节。[Timeline事件参考](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#timeline事件参考)解释了适用于每一个记录类型的详细情况。  
![](https://developer.chrome.com/devtools/docs/timeline-images/image12.png)  
在记录试图除了详情，你可以在以下三种模式下检查记录：  
* 事件模式 以事件分类展示所有事件记录
* 帧模式 展示页面的渲染性能
* 内存模式 展示随着时间的推移内存使用情况

#####事件模式
事件模式提供以类型组织的记录期间捕获的所有事件的概览。大致能看出你的应用在什么类型的任务上的主要的时间消耗。这个视图中的水平条的长度对应于事件完成花费的时间长度。  
![](https://developer.chrome.com/devtools/docs/timeline-images/events_mode.png)  
当你在事件视图选择一个时间段（参考[在Timeline上放大某个事件段](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#在Timeline上放大某个事件段)），记录视图只展示对应时间段的记录。  
![](https://developer.chrome.com/devtools/docs/timeline-images/timeline_records.png)

#####帧模式
帧模式提供洞察应用的渲染性能的能力。一“帧”代表浏览器渲染一帧要显示的内容必须要做的工作--运行JavaScript、处理事件、更新DOM、改变样式布局和绘制页面。你的应用的目标是运行在每秒60帧下，对应于大多数（但不是全部）视频显示器的60Hz的刷新速率。因此，你的应用程序有大约16.6毫秒（1000毫秒/60）对每一帧做准备。  

贯穿帧视图的水平线呈现60FPS和30FPS的帧速率目标。帧的高度对应于该帧渲染所花费的时间。每帧填充的颜色表明了每种类型的任务所花费的时间百分比。

渲染每帧花费的时间显示在记录试图的顶部。如果你把鼠标悬停在显示时间上，会显示帧的附属信息，包括每种类型的任务的时间、CPU时间、计算FPS的时间。
![](https://developer.chrome.com/devtools/docs/timeline-images/frames_mode.png)  
参考[定位强制同步布局](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#定位强制同步布局)的示例。

######关于透明或浅灰色的帧
也许你已经注意到有些区域的帧是浅灰色的或者透明的（空的）。这些区域分别表明：
* DevTools不感知的活动
* 刷新周期间的空闲时间

下面的帧中展示了感知的活动和空闲时间。  
![](https://developer.chrome.com/devtools/docs/timeline-images/clear-frames.png)  

######关于绿色条
绘图是一个两步的过程，包括：绘制调用和栅格化。  
* 绘制调用。 这是你想要绘制的东西的列表，它来自于你的元素应用的CSS规则。最后还有一些绘制调用和画布上的元素不同：moveTo, lineTo, and fillRect。尽管在[Skia](https://code.google.com/p/skia/)和Chrome绘制后端，他们有着不同的名字，但它们是类似的概念。
* 栅格化。The process of stepping through those draw calls and filling out actual pixels into buffers that can be uploaded to the GPU for compositing.

在此背景下，实体绿色条和空绿色条之间有什么不同？  
![](https://developer.chrome.com/devtools/docs/timeline-images/hollow-green-bars.png)  
* 实体绿色条的绘制调用被Chrome记录。这些发生在JavaScript执行、样式计算和布局旁的主线程里。组合线程从绘制调用获取数据结构组。
* 空绿色条被栅格化。合成器产生一个工作线程处理这些。

两者都被绘制，它们只是代表工作的不同子任务。如果你有性能问题你看查找你正在改变什么属性。然后，查看是否有一个合成器，只有这样才能达到同样的目的。[CSS触发器](http://csstriggers.com/)可以帮助明确一个解决方案。

######查看帧率统计
被选中的帧范围的平均帧率和标准偏差显示在Timeline面板的底部。如果你把鼠标悬停在平均帧速率上，弹窗框会显示该帧的以下信息：  
* Selected range -- 选中的时间段内的帧数
* Minimum Time -- 选中的帧内的最小耗时，括号内是相应的帧速率。
* Average Time -- 选中的帧内的平均耗时，括号内是相应的帧速率。
* Maximum Time -- 选中的帧内的最大耗时，括号内是相应的帧速率。
* Standard Deviation -- 计算平均耗时的数量变化。
* Time by category -- 以颜色区分的处理每种类型事件的耗时。

![](https://developer.chrome.com/devtools/docs/timeline-images/average.png)  

#####内存模式
内存视图展示你的应用的随着时间内存使用情况，包括文档数、DOM节点数，以及在内存内的事件监听（未被GC的）。  
![](https://developer.chrome.com/devtools/docs/timeline-images/image20.png)  
内存视图无法直接告诉你哪里引起了内存泄露，但可以帮助你识别你的应用里的什么事件可能导致内存泄露。你可以接着使用[Heap Profiler](https://developer.chrome.com/devtools/docs/heap-profiling.html)来识别出导致内存泄露的代码。

#####记录
启动记录会话，访问你的应用，再停止记录，以此来获取一段记录。 It helps to know in advance the kind of activity you want to record — for example, page loading, scrolling performance of a list of images, and so forth, and then stick to that script.  
为了更好的记录：  
1.打开你要记录的页面。  
2.打开Timeline面板，使用下面的按钮来开始记录:  
  点击Timeline面板的这个圆形按钮：![](https://developer.chrome.com/devtools/images/recording-off.png)  
  使用快捷键Ctrl+E, Mac上是Cmd+E。  
  在记录期间，记录按钮会变成：![](https://developer.chrome.com/devtools/images/recording-on.png)  
3.在页面上执行你期望记录的活动。  
4.按下页面上当前是红色的记录按钮或使用快捷键来停止记录。  

######记录页面加载
一个常见的任务是来自网络初始化的页面加载，键盘快捷键在这种场景下很有用，让你能快速的启动记录、重新加载页面、停止记录。  
记录一个页面加载：  
1.在一个新的tab页或者窗口中打开web页面。  
2.打开Timeline面板按下Cmd+E(Mac)或者Ctrl+E(Windows/Linux)来开始记录。  
3.快速按下Cmd+R或者Ctrl+R来重新加载页面。  
4.页面加载完停止记录。（参考[记录](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#记录)）  
你会得到类似以下的页面，第一条记录（发送HTTP请求）是chrome为获取页面的HTTP请求，紧接着是获取HTTP相应相关的记录，接着是一条或者多条接收数据的记录、完成加载的记录、解析HTML的记录。  
![](https://developer.chrome.com/devtools/docs/timeline-images/image06.png)  
参考[Timeline事件参考](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#timeline事件参考)了解详细的记录类型。  

######更好获取记录的小技巧
这是一些更好获取记录的小技巧  
*  尽可能的缩短记录时间。短的记录更好分析。
*  避免不必要的动作。避免这些和你要记录和分析不相关的动作（鼠标点击、网络请求等）。比如你要记录点击“登录”以后的事件，不要滚动页面或者加载图片等。
*  禁用浏览器缓存。当记录网络操作时，在DevTools设置面板上禁用浏览器缓存是个不错的主意。
*  禁用扩展程序。Chrome的应用扩展会在Timeline里对你的应用产生一些干扰。你可以做以下操作：  
	以隐身模式打开Chrome窗口
	创建一个新的Chrome用户用来测试  

###分析Tineline面板里的记录
这部分是分析Timeline面板里的记录的一些小贴士。  

#####查看记录详情
当你选中Timeline面板里的一条记录，详情面板里会展示该事件相关的详细信息。  
![](https://developer.chrome.com/devtools/docs/timeline-images/frames_mode_event_selected.png)  
有些细节在所有类型的事件中多呈现，比如持续时间和CPU计算时间。关于每种类型事件的详细信息，参考[Timeline事件参考](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#timeline事件参考)。  

当你选择一条绘制记录，DevTools高亮了蓝色半透明的矩形更新，如下图所示画面的区域。  
![](https://developer.chrome.com/devtools/docs/timeline-images/paint-hover.png)  

#####DOMContentLoaded and Load event markers
The Timeline annotates each recording with a blue and a red line that indicate, respectively, when the DOMContentLoaded and load events were dispatched by the browser. Timeline面板以蓝线和红线标注浏览器发出DOMContentLoaded事件和load事件。DOMContentLoaded事件在页面的元素加载和解析完被触发，load事件在文档的所有资源（图片、CSS文件等）全部加载好时触发一次。  
![](https://developer.chrome.com/devtools/docs/timeline-images/event_markers.png)  

#####定位强制同步布局
布局是Chrome计算页面上所有元素的位置、大小的处理过程。通常Chrome在响应中“懒”处理应用中的CSS规则应用或者DOM更新。Chrome在攒一批样式和布局修改而不是每当有变化时就去处理。当然应用程序可以通过查询特定布局相关的元素属性，如element.offsetWidth的值强制Chrome去立即执行布局。所以这被称为“强制同步布局”，并在频繁执行或者在一棵大的DOM树上执行时有可能带来大的性能瓶颈。  

Timeline面板会以一个黄色叹号(![](https://developer.chrome.com/devtools/docs/timeline-images/image25.png))标识出应用中的强制同步布局，选中这条记录，详情面板包含响应的调用堆栈。  
![](https://developer.chrome.com/devtools/docs/timeline-images/forced_layout.png)  
如果一条记录中包含的子记录中有强制布局，父记录会被标识稍微暗一点的黄色叹号。展开父记录能定位到引起强制布局的子记录。  
参考[定位强制同步布局](https://github.com/zhangyaowu/CN-Chrome-DevTools/blob/master/md/Performance-Profiling/Performance-profiling-with-the-Timeline.md#定位强制同步布局)样例来参考检测和修复这类性能问题。  

#####关于嵌套事件
Timeline里的事件有时是嵌套在父事件的下方的。你可以展开父事件查看嵌套的子事件。有两个原因导致Timeline里有嵌套事件：  
* 在处理一个事件时先发生了同步事件。每个事件内部产生两个原子事件，一个用来启动一个用来结束时，被转换为一个单一的“连续”事件。任何其他发生在这两个原子事件之间的事件成为外部子事件。

下面的截图是一个嵌套同步事件的示例。在这个例子中，Chrome正在解析一些HTML时发现需要一些外部资源被加载。这些请求在解析完成前发出来，所以发送请求时间作为解析HTML的子事件被显示出来。  
![](https://developer.chrome.com/devtools/docs/timeline-images/sync_events.png)  

#####给Timeline里的嵌套事件着色
Timeline条按照以下着色：  
* 最前面的、颜色最深的条代表事件和它的所有的同步子事件花费的时间。
* 接着稍微淡一点的颜色代表时间和它的所有异步子事件的CPU计算时间。
* 最淡的条代表从第一个异步事件开始到最后一个异步事件结束所花费的时间。

![](https://developer.chrome.com/devtools/docs/timeline-images/image16.png)  
选中一条父记录将在详情面板上展示以下内容：  
* 以文本和饼图的形式的时间类型概览。
* 此时这条记录使用的JS堆大小，和这个操作对堆大小的影响
* 这个事件响应的其他详细信息。

![](https://developer.chrome.com/devtools/docs/timeline-images/parent_record.png)  

#####过滤和搜索记录
你可以按照事件类型去过滤展示哪些记录（比如只展示加载事件），或只展示大于等于1毫秒或者15毫秒的记录。你也可以过滤匹配某个字符串的记录。  
![](https://developer.chrome.com/devtools/docs/timeline-images/filters.png)  
当查看所有事件时，你可能想找某一条记录，但是它被淹没在众多记录中。这种情况你可以不使用过滤功能。按下Ctr+F(Window/Linux)或者Cmd+F(Mac)，此时Timeline中已经对角到包含搜索项的记录。  

#####在Timeline上放大某个事件段
为了更好的分析记录，你可以放大Timeline的某一段概览，以减少记录试图中相应的时间刻度。  
![](https://developer.chrome.com/devtools/docs/timeline-images/image03.png)  
放大时间段，可以按照以下做法：  
* 在概览区域，用鼠标拖动一个时间段。
* 规则区域调整灰色条。

这是更过的关于Timeline时间段的小贴士：  
* 通过拖动两个滑动条来"Scrub"当前时间段内的记录。

![](https://developer.chrome.com/devtools/docs/timeline-images/image26.png)  
* 触控板用户：
  * 向左或向右滑动两个手指移动当前时间轴选择。
  * 向上或向下滑动用两个手指扩大或减小当前时间轴选择。
* 悬停在一个时间段上向上或向下滚动鼠标滚轮来扩大或减小当前选择的时间段。  

#####保存和加载记录
你可以以json文件保存Timeline记录，后面可以在Timeline中再打开。  

######保存Timeline记录
1.在Timeline中右键或者按住Ctrl再单击鼠标左键(Mac中)，选择保存Timeline数据菜单或者在键盘上使用Ctrl+S。  
2.选择一段时间段来点击保存。 

######打开Timeline记录
1.右键或者按住Ctrl再单击鼠标左键选择加载要加载的Timeline数据，或者使用Ctrl+O键盘快捷键。  
2.定位到json文件，选中并打开。  
![](https://developer.chrome.com/devtools/docs/timeline-images/image14.png)  

#####用户产生的Timeline事件
应用可以添加它们的事件到Timeline记录中。你可以使用console.timeStamp()方法添加一个原子事件到记录中，或者使用console.time()和console.timeEnd()来标记处代码执行的时间范围。比如下图中console.timeStamp()被用来显示"Adding result"事件。参考在[Using the Console](https://developer.chrome.com/devtools/docs/console.md)中使用[Marking the Timeline](https://developer.chrome.com/devtools/docs/console.md#marking-the-timeline)来获取更多信息。  
![](https://developer.chrome.com/devtools/docs/timeline-images/adding-result.png)  

#####记录中的CPU计算时间
你会看到上面出现在Timeline记录的浅灰色条，表示CPU忙。鼠标悬停在在一个CPU条会高亮处Timeline部分在此期间，CPU是活动的（如下图所示）。一个CPU条的长度通常是Timeline中的它下面所有的（高亮）事件的总和。如果两者不匹配，这可能是由于以下之一：  
* 页面被检查期间其他页面运行在同一个线程里（比如两个标签页打开同一个网站，有一边在setTimeout()里执行一些代码）。
* 不感知的活动。

![](https://developer.chrome.com/devtools/docs/timeline-images/image24.png)  

###Timeline事件参考
本节列出并说明了各个类型的记录类型，和它们的属性。

#####Common event properties
某些详情存在于所有类型的事件，而有些只适用于某些事件类型。本节列出了常见的不同事件类型的属性。下面的参考中将列出特定于某些事件类型的属性。  

事件汇总  
    对于有嵌套事件的事件，每一层目录所花费的时间。  
调用堆栈  
    对于有子事件的事件，每一层目录所花费的时间。  
CPU计算时间  
    事件记录所消耗的CPU计算时间。  
详情  
    事件的其他详情。  
持续时间(时间戳)  
    事件和它的子事件完成所消耗的时间；时间戳是相对于记录开始事件发生的时间。  
自身时间  
    不包括任何子事件的事件自身消耗的时间。  
堆空间使用大小  
	事件记录期间应用占用的内存大小，和取样期间堆空间使用的变化。  
	
#####Loading事件

事件 | 描述 
:-- | :--
解析HTML | Chrome执行HTML解析算法
完成加载 | 一个网络请求完成
接收数据 | 请求的数据被接收，这会是一个或多个的接收数据事件
接收响应 | 来自请求的最初的HTTP响应
发送请求 | 一个已发送的网络请求  

#####Loading事件属性
资源  
	请求资源的URL。  
预览  
	请求资源的预览（仅支持图片）。  
请求方法  
	请求的HTTP方法(GET或POST等等)。  
状态码  
	HTTP响应状态码。  
MIME类型  
	请求的资源的MIME类型。  
数据大小  
	请求的资源的字节数。  

#####Scripting事件
这一节介绍Scripting类别的事件和它们的属性。  

事件 | 描述
:-- | :--
动画帧触发 | 一个预定的动画帧被触发，它的回调处理程序被调用。
取消动画帧 | 一个动画帧被取消。
GC事件 | 垃圾回收发生。
DOM内容加载 | DOM内容加载被浏览器触发。这个事件在所有的页面DOM元素加载和解析完被触发。
脚本执行 | 一段脚本被执行。
事件 | 一个JavaScript事件("鼠标按下",或按键事件等)。
函数调用 | 调用一个顶层的JavaScript函数（仅出现在当浏览器进入JavaScript引擎）。
设置定时器 | 一个定时器以setInterval()或者setTimeout()创建。
请求动画帧 | 调用requestAnimationFrame()函数来调度新的帧。
移除定时器 | 将之前创建的定时器清除。
开始计时 | 调用console.time()函数。
计时结束 | 调用console.timeEnd()函数。
定时任务触发 | 一个以setInterval()或者setTimeout()调度的定时器被触发。
XHR状态改变 | XMLHTTPRequest状态改变。
XHR加载| XMLHTTPRequest完成加载。

#####Scripting事件属性

Timer ID  
	定时器ID.  
Timeout  
	定时器指定的超时时间。  
Repeats  
	定时器是否循环的标识。  
Function Call  
	被调用的函数。  

#####Rendering事件
这一节列出渲染时间和它们的属性。

事件 | 描述
:-- | :--
无效布局 | 变化的DOM的布局无效
布局 | 页面执行布局
重新计算样式 | Chrome重新计算元素样式
滚动 | 嵌套视图的内容被滚动

#####Rendering事件属性

Layout invalidated  
	导致无效布局的代码堆栈。  
Nodes that need layout  
	重新布局前需要布局的节点数。这通常是由开发者的代码无效导致的。  
Layout tree size  
	根布局（Chrome开始布局的节点）下节点总数。  
Layout scope  
	可能的值是“部分”（重新布局的边界是DOM的一部分）或者整个文档。  
Elements affected
	重新计算样式，受影响的元素数量。   recalculation.
Styles invalidated
	重新计算样式时，导致无效样式的代码堆栈。  

#####Painting事件
这一节介绍Painting事件和它们的属性。 

事件 | 描述
:-- | :--
Composite Layers | Chrome的渲染引擎合成图像层。
Image Decode | 图像解码。
Image Resize | 图像还原为原始大小。
Paint | 复合层绘制到显示器的一个区域。鼠标悬停在在一条Paint纪录页面上会高亮显示更新的区域。

#####Painting事件属性
Location  
	绘制的矩形的x和y坐标。  
Dimensions  
	绘制区域的高度和宽度。  

***
Content available under the [CC-By 3.0 license](http://creativecommons.org/licenses/by/3.0/)
