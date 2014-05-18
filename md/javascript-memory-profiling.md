# JavaScript内存分析

**内存泄漏**是指计算机可用内存的逐渐流失。当程序持续无法释放其使用的临时内存时就会发生。JavaScript的web应用也会经常遇到在原生应用程序中出现的内存相关的问题，像**泄漏**和溢出，web应用也需要应对**垃圾回收停顿**。

尽管JavaScript使用垃圾回收做自动内存管理，但[有效的(effective)](http://www.html5rocks.com/en/tutorials/memory/effectivemanagement/)内存处理依然很重要。在这篇文章中我们将探讨分析JavaScript web应用中的内存问题。在学习有关特性时请确保尝试一下[相关案例](#supporting_demos)以提高你对这些工具在实践中如何工作的认识。

请阅读[内存 101(Memory 101)](https://developers.google.com/chrome-developer-tools/docs/memory-analysis-101)页面来帮助你熟悉这篇文章中用到的术语。

**注意：**我们将要用到的有些特性目前仅在[Chrome Canary版](http://www.google.com/intl/en/chrome/browser/canary.html)浏览器中有。我们推荐使用这个版本来获得最佳的工具，以分析你的应用程序的内存问题。


## 你需要思考的问题

总体来说，当你觉得你遇到了内存泄漏问题时，你需要思考三个问题：

* **我的页面是否占用了过多的内存?** - [Timeline内存查看工具(Timeline memory view)](#heading=h.3gfl4k8caz0k) 和 [Chrome任务管理(Chrome task manager)](#chrome-任务管理器) 能帮助你确认你是否使用了过多的内存。Memory view 能跟踪页面渲染过程中DOM节点计数，documents文档计数和JS事件监听计数。作为一个经验法则：避免对不再需要用到的DOM元素的引用，移除不需要的事件监听并且在存储你可能不会用到的大块数据时要留意。

* **我的页面有没有内存泄漏?** - [对象分配跟踪(Object allocation tracker)](#heading=h.8yjlf68i8qix)通过实时查看JS对象的分配来帮助你定位泄漏。你也可以使用[堆分析仪(Heap Profiler)](#heading=h.g0yxr1o33gky)生成JS堆快照，通过分析内存图和比较快照之间的差异，来找出没有被垃圾回收清理掉的对象。

* **我的页面垃圾强制回收有多频繁?** - 如果你的页面垃圾回收很频繁，那说明你的页面可能内存使用分配太频繁了。[Timeline内存查看工具(Timeline memory view)](#heading=h.3gfl4k8caz0k) 能够帮助你发现感兴趣的停顿。

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_0.png)


## 术语和基本概念

本小节介绍在**内存分析**时使用的常用术语，这些术语在为其它语言做内存分析的工具中也适用。这里的术语和概念用在了堆分析仪(Heap Profiler)UI工具和相关的文档中。

这些能够帮助我们熟悉如何有效的使用内存分析工具。如果你以前从来没有了解过像Java，.NET等语言的内存分析的话，那么本节介绍的术语对你来说可能就是些全新的概念了。

### 对象大小(Object sizes)

把内存想象成一个包含基本类型(像数字和字符串)和对象(类似数组)的图表。它可能看起来像下面这幅一系列相关联的点组成的图。

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/thinkgraph.png)

一个对象有两种使用内存的方法：

* 对象自身直接使用

* 隐含的保持对其它对象的引用，这种方式会阻止垃圾回收(简称GC)对这些对象的自动回收处理。

当你使用DevTools中的堆分析仪(Heap Profiler，用来分析内存问题的工具，在DevTools的"Profile"标签下)时，你可能会惊喜的发现一些显示各种信息的栏目。其中有两项是：**直接占用内存(Shallow Size)**和**占用总内存(Retained Size)**，那它们是什么意思呢？

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_1.png)


#### 直接占用内存(Shallow Size，不包括引用的对象占用的内存)

这个是对象本身占用的内存。

典型的JavaScript对象都会有保留内存用来描述这个对象和存储它的直接值。一般，只有数组和字符串会有明显的直接占用内存(Shallow Size)。但字符串和数组常常会在渲染器内存中存储主要数据部分，仅仅在JavaScript对象栈中暴露一个很小的包装对象。

渲染器内存指你分析的页面在渲染的过程中所用到的所有内存：页面本身的内存 + 页面中的JS堆用到的内存 + 页面触发的相关工作进程(workers)中的JS堆用到的内存。然而，通过阻止垃圾自动回收别的对象，一个小对象都有可能间接占用大量的内存。

#### 占用总内存(Retained Size，包括引用的对象所占用的内存)

一个对象一但删除后它引用的依赖对象就不能被**GC根(GC root)**引用到，它们所占用的内存就会被释放，一个对象占用总内存包括这些依赖对象所占用的内存。

**GC根**是由*控制器(handles)*组成的，这些控制器(不论是局部还是全局)是在建立由build-in函数(native code)到V8引擎之外的JavaScript对象的引用时创建的。所有这些控制器都能够在堆快照的**GC roots(GC根)** > **Handle scope** 和 **GC roots** > **Global handlers**中找到。如果不深入了解浏览器的实现原理，在这篇文章中介绍这些控制器可能会让人不能理解。GC根和控制器你都不需要过多关心。

有很多内部的GC根对用户来说都是不重要的。从应用的角度来说有下面几种情况：

* Window 全局对象 (所有iframe中的)。在堆快照中有一个distance字段，它是从window对象到达对应对象的最短路径长度。

* 由所有document能够遍历到的DOM节点组成的文档DOM树。不是所有节点都会被对应的JS引用，但有JS引用的节点在document存在的情况下都会被保留。

* 有很多对象可能是在调试代码时或者DevTools console中(比如：console中的一些代码执行结束后)创建出来的。

**注意：**我们推荐用户在创建堆快照时，不要在console中执行代码，也不要启用调试断点。

内存图由一个根部开始，可能是浏览器的`window`对象或Node.js模块`Global`对象。这些对象如何被内存回收不受用户的控制。

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/dontcontrol.png)

不能被GC根遍历到的对象都将被内存回收。

**注意：**直接占用内存和占用总内存字段中的数据是用字节表示的。

### 对象的占用总内存树

之前我们已经了解到，堆是由各种互相关联的对象组成的网状结构。在数字领域，这种结构被称为*图*或内存图。图是由*边缘(edges)*连接着的*节点(nodes)*组成的，他们都被贴了标签。

* **节点(Nodes)** (*或对象*) 节点的标签名是由创建他们的*构造(constructor)*函数的名称确定

* **边缘(Edges)** 标签名就是属性名

本文档的后面你将了解到如何使用堆分析仪生成快照。从下图的堆分析仪生成的快照中，我们能看到距离(distance)这个字段：是指对象到GC根的距离。如果同一个类型的所有对象的距离都一样，而有一小部分的距离却比较大，那么就可能出了些你需要进行调查的问题了。

![](http://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_2.png)

### 支配对象(Dominators)

支配对象就像一个树结构，因为每个对象都有一个支配者。一个对象的支配者可能不会直接引用它支配的对象，就是说，支配对象树结构不是图中的生成树。

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/dominatorsspanning.png)

在上图中：

* 节点1支配节点2
* 节点2支配节点3，4和6
* 节点3支配节点5
* 节点5支配节点8
* 节点6支配节点7

在下图的例子中，节点`#3`是`#10`的支配者，但`#7`也在每个从GC到`#10`的路经中都出现了。像这样，如果B对象在每个从根节点到A对象的路经中都出现，那么B对象就是A对象的支配对象。

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/dominators.gif)

### V8介绍

在本节，我们将描述一些内存相关的概念，这些概念是和**V8 JavaScript虚拟机**(V8 VM 或VM)有关的。当分析内存时，了解这些概念对理解堆快照是有帮助的。

#### JavaScript对象描述

有三个原始类型：

* 数字(Numbers) (如 3.14159..)
* 布尔值(Booleans) (true或false)
* 字符型(Strings) (如 'Werner Heisenberg')

它们不会引用别的值，它们只会是叶子节点或终止节点。

**数字(Numbers)**以下面两种方式之一被存储：

* 31位整数直接值，称做：**小整数(small integers)**(*SMIs*)，或

* 堆对象，引用为**堆值**。堆值是用来存储不适合用SMI形式存储的数据，像*双精度数(doubles)*，或者当一个值需要被*打包(boxed)*时，如给这个值再设置属性值。

**字符型**数据会以下面两种方式存储：

* **VM堆**，或

* 外部的**渲染器内存**中。这时会创建一个包装对象用来访问存储的位置，比如，Web页面包存的脚本资源和其它内容，而不是直接复制至VM堆中。

新创建的JavaScript对象会被在JavaScript堆上(或**VM堆**)分配内存。这些对象由V8的垃圾回收器管理，只要还有一个强引用他们就会在内存中保留。

**本地对象**是所有不在JavaScript堆中的对象，与堆对象不同的是，在它们的生命周期中，不会被V8垃圾加收器处理，只能通过JavaScript包装对象引用。

**连接字符串**是由一对字符串合并成的对象，是合并后的结果。**连接字符串**只在有需要时合并。像一连接字符串的子字符串需要被构建时。

比如：如果你连接**a**和**b**，你得到字符串(a, b)这用来表示连接的结果。如果你之后要再把这个结果与**d**连接，你就得到了另一个连接字符串((a, b), d)。

**数组(Arrays)** - 数组是数字类型键的对象。它们在V8引擎中存储大数据量的数据时被广泛的使用。像字典这种有键-值对的对象就是用数组实现的。

一个典型的JavaScript对象可以通过两种数组类型之一的方式来存储：

* 命名属性，和

* 数字化的元素

如果只有少量的属性，它们会被直接存储在JavaScript对象本身中。

**Map** - 一种用来描述对象类型和它的结构的对象。比如，maps会被用来描述对象的结构以实现对对象属性的[快速访问](https://developers.google.com/v8/design.html#prop_access)

#### 对象组

每个本地对象组都是由一组之间相互关联的对象组成的。比如一个DOM子树，每个节点都能访问到它的父元素，下一个子元素和下一个兄弟元素，它们构成了一个关联图。需要注意的是本地元素没有在JavaScript堆中表现－这就是它们的大小是零的原因，而它的包装对象被创建了。

每个包装对象都会有一个到本地对象的引用，用来传递对这些本地对象的操作。这些本地对象也有到包装对象的引用。但这并不会创造无法收回的循环，GC是足够智能的，能够分辨出那些已经没有引用包装对象的本地对象并释放它们的。但如果有一个包装对象没有被释放那它将会保留所有对象组和相关的包装对象。

## 先决条件和有用提示

### Chrome 任务管理器

**注意：** 当使用Chrome做内存分析时，最好设置一个[洁净的测试环境](https://developers.google.com/chrome-developer-tools/docs/clean-testing-environment)

打开Chrome的内存管理器，观察内存字段，在一个页面上做相关的操作，你可以很快定位这个操作是否会导致页面占用很多内存。你可以从Chrome菜单 > 工具或按Shift + Esc，找到内存管理器。

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_5.png)

打开后，在标头右击选用 JavasScript使用的内存 这项。

### 通过DevTools Timeline来定位内存问题

解决问题的第一步就是要能够证明问题存在。这就需要创建一个可重现的测试来做为问题的基准度量。没有可再现的程序，就不能可靠的度量问题。换句话说如果没有基准来做为对比，就无法知道是哪些改变使问题出现的。

**时间轴面版(Timeline panel)**对于发现程序什么时候出了问题很用帮助。它展示了你的web应用或网站加载和交互的时刻。所有的事件：从加载资源到解JavaScript，样式计算，垃圾回收停顿和页面重绘。都在时间轴上表示出来了。

当分析内存问题时，时间轴面版上的**内存视图(Memory view)**能用来观察：

* 使用的总内存 - 内存使用增长了么?

* DOM节点数

* 文档(documents)数

* 注册的事件监听器(event listeners)数

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_6.png)

更多的关于在内存分析时，定位内存泄漏的方法，请阅Zack Grossbart的[Memory profiling with the Chrome DevTools](http://coding.smashingmagazine.com/2012/06/12/javascript-profiling-chrome-developer-tools/)

#### **证明一个问题的存在**

首先要做的事情是找出你认为可能导致内存泄漏的一些动作。可以是发生在页面上的任何事件，鼠标移入，点击，或其它可能会导致页面性能下降的交互。

在时间轴面版上开始记录(Ctrl+E 或 Cmd+E)然后做你想要测试的动作。想要强制进行垃圾回收点面版上的垃圾筒图标(![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_8.png))。

下面是一个内存泄漏的例子，有些点没有被垃圾回收：

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/nodescollected.png)

如果经过一些反复测试后，你看到的是[锯齿](http://en.wikipedia.org/wiki/Sawtooth_wave)状的图形(在内存面版的上方)，说明你的程序中有很多短时存在的对象。而如果一系列的动作没有让内存保持在一定的范围，并且DOM节点数没有返回到开始时的数目，你就可以怀疑有内存泄漏了。

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_10.png)

一旦确定了存在内存上的问题，你就可以使用**分析面板(Profiles panel)**上的**堆分析仪(heap profiler)**来定位问题的来源。

例子: 尝试一下[memory growth](https://developers.google.com/chrome-developer-tools/docs/demos/memory/example1.html)的例子，能帮助你有效的练习通过时间轴分析内存问题。

### 内存回收

*内存回收器*(像V8中的)需要能够定位哪些对象是*活的(live)*，而那些被认为是*死的*(垃圾*)*的对象是*无法引用到的(unreachable)*。

如果**垃圾回收** (GC)因为JavaScript执行时有逻辑错误而没有能够回收到垃圾对象，这些垃圾对象就无法再被重新回收了。像这样的情况最终会让你的应用越来越慢。

比如你在写代码时，有的变量和事件监听器已经用不到了，但是却仍然被有些代码引用。只要引用还存在，那被引用的对象就无法被GC正确的回收。

当你的应用程序在运行中，有些DOM对象可能已经更新/移除了，要记住检查引用了DOM对象的变量并将其设null。检查可能会引用到其它对象(或其它DOM元素)的对象属性。双眼要盯着可能会越来越增长的变量缓存。

## 堆分析仪

### 拍一个快照

在Profiles面板中，选择** *Take Heap Snapshot* **，然后点击**Start**或者按Cmd + E或者Ctrl + E：

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_11.png)

快照最初是保存在渲染器进程内存中的。它们被按需导入到了DevTools中，当你点击快照按钮后就可以看到它们了。当快照被载入DevTools中显示后，快照标题下面的数字显示了[能够被引用到的(reachable)](https://developers.google.com/chrome-developer-tools/docs/memory-analysis-101.html#retaining_paths)JavaScript对象占有内存总数。

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_12.png)

例子：尝试一下[garbage collection in action](https://developers.google.com/chrome-developer-tools/docs/demos/memory/example2.html)的例子，在时间轴(Timeline)面板中监控内存的使用。

### 清除快照

点击Clear all按钮图标(![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_14.png))，就能清除掉所有快照：

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_15.png)

**注意：**关闭DevTools窗口并不能从渲染内存中删除掉收集的快照。当重新打开DevTools后，之前的快照列表还在。

记住我们之前提到的，当你生成快照时你可以强制执行在DevTools中GC。当我们拍快照时，GC是自动执行的。在时间轴(Timeline)中点击垃圾桶(垃圾回收)按钮(![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_8.png))就可以轻松的执行垃圾回收了。

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/force.png)

例子：尝试一下[scattered objects](https://developers.google.com/chrome-developer-tools/docs/demos/memory/example3.html)并用堆分析仪(Heap Profiler)分析它。你可以看到(对象)项目的集合。

### 切换快照视图

一个快照可以根据不同的任务切换视图。可以通过如图的选择框切换：

![](https://developers.google.com/chrome-developer-tools/docs/memory-profiling-files/image_17.png)

下面是三个默认视图：
* **Summary(概要) - **通过构造函数名分类显示对象；
* **Comparison(对照) - **显示两个快照间对象的差异；
* **Containment(控制) - **可用来探测堆内容；

**Dominators(支配者)**视图可以在Settings面板中开启 **- **显示[dominators tree.](https://developers.google.com/chrome-developer-tools/docs/memory-analysis-101.html#dominators) 可以用来找到内存增长点。

### 通过不同颜色区分对象

对象的属性和属性值有不同的类型并自动的通过颜么进行了区分。每个属性都是以下四种之一：

* **a:property —**通过名称索引的普通属性，由.(点)操作符，或\[\](中括号)引用，如\["foo bar"\]；

* **0:element -**通过数字索引的普通属性，由\[\](中括号)引用；

* **a:****context var - **函数内的属性，在函数上下文内，通过名称引用；

* **a:****system prop - **由JavaScript VM 添加的属性，JavaScript代码不能访问。


命名为`System `的对象没有对应的JavaScript类型。它们是JavaScript VM对象系统内置的。V8将大多数内置对象和用户JS对象放在同一个堆中。但它们只是V8的内部对象。

## 视图详解
### Summary view(概要视图)

打开一个快照，默认是以概要视图显示的，显示了对象总数，可以展开显示具体内容：
Initially, a snapshot opens in the Summary view, displaying object totals, which can be expanded to show instances:

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_19.png)

第一层级是"总体"行，它们显示了：

* **Constructor(构造函数)**表示所有通过该构造函数生成的对象

* **对象的实例数**在Objects Count列上显示

* **Shallow size**列显示了由对应构造函数生成的对象的[shallow sizes(直接占用内存)](https://developers.google.com/chrome-developer-tools/docs/memory-analysis-101.html#object_sizes)总数

* **Retained size**列展示了对应对象所占用的最大内存

* **Distance**列显示的是对象到达GC根的最短距离

展开一个总体行后，会显示所有的对象实例。没一个实例的直接占用内存和占用总内存都被相应显示。@符号后的数字不对象的唯一ID，有了它你就可以逐个对象的在不同快照间作对比。

例子：尝试这个[例子](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-summary)(在新tab标签中打开)来了解如何使用概要视图。

记住黄色的对象被JavaScript引用，而红色的对象是由黄色背景色引用被分离了的节点。

### Comparison view(对照视图)

该视图用来对照不同的快照来找到快照之间的差异，来发现有内存泄漏的对象。来证明对应用的某个操作没有造成泄漏(比如：一般一对操作和撤消的动作，像找开一个document，然后关闭，这样是不会造成泄漏的)，你可以按以下的步骤尝试：

1. 在操作前拍一个堆快照；

2. 执行一个操作(做你认为会造成泄漏的动作)；

3. 撤消之前的操作(上一个操作相反的操作，多重复几次)；

4. 拍第二个快照，将视图切换成对照视图，并同快照1进行对比。

在对照视图下，两个快照之间的不同就会展现出来了。当展开一个总类目后，增加和删除了的对象就显示出来了：

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_21.png)

例子：尝试[例子](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-comparison)(在新tab标签中打开)来了解如何使用对照视图来定位内存泄漏。

### Containment view(控制视图)

控制视图可以称作对你的应用的对象结构的"鸟瞰视图(bird's eys view)"。它能让你查看function内部，跟你的JavaScript对象一样的观察VM内部对象，能让你在你的应用的非常低层的内存使用情况。

该视图提供了几个进入点：

* **DOMWindow**** 对象** - 这些对象是JavaScript代码的"全局"对象；

* **GC根** - VM的垃圾回收器真正的GC根；

* **Native对象** - 浏览器对象对"推入"JavaScript虚拟机中来进行自动操作，如：DOM节点，CSS规则(下一节会有详细介绍。)

下图是一个典型的控制视图：

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_22.png)

例子：尝试[例子](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-containment)(在新tab标签中打开)来了解如何使用控制视图来查看闭包内部和事件处理。

**关于闭包的建议**

给函数命名对你在快照中的闭包函数间作出区分会很用帮助。如：下面的例子中没有给函数命名：

```
function createLargeClosure() {
  var largeStr = new Array(1000000).join('x');

  var lC = function() { // this is NOT a named function
    return largeStr;
  };

  return lC;
}
```
而下面这个有给函数命名：

```
function createLargeClosure() {
  var largeStr = new Array(1000000).join('x');

  var lC = function lC() { // this IS a named function
    return largeStr;
  };

  return lC;
}
```
![](https://developer.chrome.com/devtools/docs/memory-profiling-files/domleaks.png)

例子：尝试这个例子[why eval is evil](https://developer.chrome.com/devtools/docs/demos/memory/example7.html)来分析内存中闭包的影响。你可能也对尝试下面这个例子，记录[heap allocations(堆分配)](https://developer.chrome.com/devtools/docs/demos/memory/example8.html)有兴趣。

### 揭露DOM内存泄漏

这个工具独一无二的一点是展示了浏览器原生对象(DOM节点，CSS规则)和JavaScript对象之间的双向引用。这能帮助你发现因为忘记解除引用游离的DOM子节点而导致的难以发觉的内存泄漏。

DOM内存泄漏可能会超出你的想象。看下下面的例子 - #tree对象什么时候被GC呢？

```
  var select = document.querySelector;
  var treeRef = select("#tree");
  var leafRef = select("#leaf");
  var body = select("body");

  body.removeChild(treeRef);

  //#tree can't be GC yet due to treeRef
  treeRef = null;

  //#tree can't be GC yet due to indirect
  //reference from leafRef

  leafRef = null;
  //#NOW can be #tree GC
```

`#leaf`代表了对它的父节点的引用(parentNode)它递归引用到了`#tree`，所以，只有当leafRef被nullified后`#tree`代表的整个树结构才会被GC回收。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/treegc.png)

例子：尝试[leaking DOM nodes](https://developer.chrome.com/devtools/docs/demos/memory/example6.html)来了解哪里DOM节点会内存泄漏并如何定位。你也可以看一下这个例子：[DOM leaks being bigger than expected](https://developer.chrome.com/devtools/docs/demos/memory/example9.html)。

查看Gonzalo Ruiz de Villa的文章[Finding and debugging memory leaks with the Chrome DevTools](http://slid.es/gruizdevilla/memory)来阅读更多关于DOM内存泄漏和内存分析的基础。

原生对象在Summary和Containment视呼中更容易找到 - 有它们专门的类目：

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_24.png)

例子：尝试下这个[例子](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-dom-leaks)(在新tab标签中打开)来了解如何将DOM树分离。

### 支配者视图(Dominators view)

支配者视图显示了堆图的支配者树。支配者视图跟控制(Containment)视图很像，但是没有属性名。这是因为支配者可能会是一个没有直接引用的对象，就是说这个支配者树不是堆图的生成树。但这是个有用的视图能帮助我们很快的定位内存增长点。

注意：在Chrome Canary中，支配者视图能够在DevTools中的Settings > Show advanced heap snapshot properties 开启，重启DevTools生效。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_25.png)

例子：尝试这个[例子](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-dominators)(在新tab标签中打开)来练习如何找到内存增长点。可以进一步尝试下一个例子[retaining paths and dominators](https://developer.chrome.com/devtools/docs/demos/memory/example10.html)。

## 对象分配跟踪器

**对象跟踪器**整合了[heap profiler](https://developer.chrome.com/devtools/docs/javascript-memory-profiling#heap_profiler)的快照增量更新分析和Timeline面板的记录。跟其它工具一样，记录对象的堆配置需要启动记录，执行一系列操作，然后停止记录然后进行分析。

对象跟踪器不间断的记录堆快照(频率达到了每50毫秒！)，结束时记录最后一个快照。该堆分配分析器显示对象在哪被创建并定位它的保留路径。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_26.png)

**开启并使用对象分析器**

开始使用对象分析器：
1. 确认你使用的是最新版的[Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html)。

2. 打开DeveTools并点击齿轮图标(译者：没明白这步有什么用)。

3. 现在，打开Profiler面板，你就能看到"Record Heap Allocations"的选项。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_27.png)

上面的柱条表示在堆中生成的新对象。高度就对应了相应对象的大小，它的颜色表示了这个对象是否在最后拍的那个快照中还在：蓝色柱表示在timeline最后这个对象还在，灰色柱表示这个对象在timeline中生成，但结束前已经被内存回收了。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/collected.png)

上面的例子中，一个动作执行了10次。同一个程序保留了5个对象，所以最后5个蓝色柱条被保留了。但这最后留下的柱存在潜在的问题。你可以用timeline上的滑动条缩小到那个特定的快照并找到这个分配的对象。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_29.png)

点击一个堆中的对象就能在堆快照的下面部分显示它的保留总内存树。检查这个对象的保留总内存树能够给你足够的信息来了解为什么这个对象没有被回收，然后你就能对代码做相应的修改来去掉不必要的引用。

## 内存分析FAQ

**问：我不能看到对象的所有属性，我也看到它们的非字符串值！为什么？**

并非所有属性都完整的保存在JavaScript堆中。其中有些是通过执行原生代码的getters方法来获取的。这些属性没有在堆快照中捕获，是为了防止对getters方法的调用和避免程序状态的改变，如果这些getters方法不是"纯(pure)"的functions。同样，非字符串的值，如数字，没有被捕获是为了减少快照的大小。

**问：****@****符号后面的数字是什么意思 - 是地址还是ID呢？这个ID值真的是唯一的么？**

这是对象ID。显示对象的地址没有意义，因为一个对象会在垃圾回收的时候被移除。这些对象IDs是真正的IDs - 就是说，它们在不同的快照间是唯一表示的。这样就可以的堆状态间进行精确的对比。维持这些IDs会给GC流程增加额外的开支，但这仅在记录第一次堆快照时分配 - 如果堆分析仪没有用到，就不会有额外的开支。

**问："死"(无法引用到的)对象被包含在快照中了么？**

没有，只有可以引用到的对象才会显示在快照中。而且，拍快照前都会先自动执行GC操作。

**注意：**在写这篇文章的时候，我们计划在拍快照的时候不再GC，防止堆尺寸的减少。现在已经是这样了，但垃圾对象依然显示在快照之外。

**问：GC根是由什么组成的？**

由很多部分组成：

* 原生对象图；

* 符号表；

* VM线程中的栈；

* 编辑缓存；

* 控制器上下文；

* 全局控制器。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_30.jpg)

**问：我得知可以使用Heap Profiler和Timeline Memory view来检测内存泄漏。但我应该先用哪个工具呢？**

Timeline面版，是在你第一次使用你的页面发现速度变慢了时用来论断过多的内存使用。网站变慢是比较典型的内存泄漏的信号，但也可能是其它的原因 - 可能是有渲染或网络传输方面的瓶颈，所以要确保解决你网页的真正问题。

论断是否是内存问题，就打开Timeline面板和Memory标签。点击record按钮，然后在你的应用上重复几次你认为可能导致内存泄漏的操作。停止记录。你应用的内存使用图就生成出来了。如果内存的使用一直在增长(而没有相应的下降)，这就表明你的应用可能有内存泄漏了。

一般一个正常的应用的内存使用图形是锯齿状的，因为内存使用后又会被垃圾回收器回收。不用担心这种锯齿形 - 因为总是会因为JavaScript而有内存的消耗，甚至一个空的`requestAnimationFrame`也会造成这种锯齿形，这是无法避免的。只要不是那种分配了持续很多内存的形状，那就表明生成了很多内存垃圾。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_31.png)

上图的增长线是需要你警惕的。在诊断分析的时候Memory标签中的DOM node counter，Document counter和Event listener count也是很有用的。DOM节点数是使用的原生内存不会影响JavaScript内存图。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_32.jpg)

一旦你确认你的应用有内存泄漏，堆分析仪就可以用来找到内存泄漏的地方。

**问：我发现堆快照中有的DOM节点的数字是用红色标记为"Detached DOM tree"，而其它的是黄色的，这是什么意思呢？**

你会发现有不同的颜色。红色的节点(有着深色的背景)没有从JavaScript到它们的直接的引用，但它们是分离出来的DOM结构的一部分，所以他们还是在内存中保留了。有可能有一个节点被JavaScript引用到了(可能是在闭包中或者一个变量)，这个引用会阻止整个DOM树被内存回收。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_33.jpg)

黄色节点(黄色背景)有JavaScript的直接引用。在同一个分离的DOM树中查看一个黄色的节点来定位你的JavaScript的引用。就可能看到从DOM window到那个节点的属性引用链(如：`window.foo.bar[2].baz`)。

下面的动态图显示了分离节点的处理过程：

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/detached-nodes.gif)

**例子**：尝试这个例子[detached nodes](https://developer.chrome.com/devtools/docs/demos/memory/example4.html)你可以查看节点在Timeline中的生命周期，然后拍堆快照来找到分离的节点。

**问：直接占用内存(Shallow Size)和占用总内存(Retained Size)分别代表什么，它们的区别是什么？**

是这样的，对象可以在内存中以两种方式存在(be alive) - 直接的被别一个可访问的(alive)对象保留(window和document对象总是可访问的)或被原生对象(象DOM对象)隐含的包留引用。后一种方式会因为阻止对象被GC自动回收，而有导制内存泄泥漏的可能。对象自身占用的内存被称为直接占用内存(通常来说，数组和字符串会保留更多的直接占用内存(shallow size))。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_36.jpg)

一个任意大小的对象可以通过阻止其它对象内存被回收在保留很大的内存使用。当一个对象被删除后(它造成的一些依赖就无法被引用了)能够释放的内存的大小被称有占用总内存(retained size)。

**问：constructor和retained字段下有很多的数据。我应该从哪开始调查我是的否遇到了内存泄漏呢？**

一般来说最好是从通过retainers排序的第一个对象开始，retainers之间是通过距离排序的(是指到window对象的距离)。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_37.jpg)

距离最短的对象有可能是首选的可能导致内存泄漏的对象。

**问：Summary, Comparison, Dominators 和 Containment这些视图之间的不同是什么？**

你可以通过切换视图来体验它们的区别。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_38.jpg)

* Summary(概要)视图能帮你通过构造函数分组寻找对象(和对象的内存使用)。该视图对找出DOM内存泄漏很有帮助。

* Comparison(对照)视图能够通过显示哪些对象内存被正确的回收了来搜寻内存泄漏。通常在一个操作前后记录两个(或更多)的内存使用快照。它是通过察看释放的内存和引用数目的差导来察看是否有内存泄漏，并找到原因。

* Containment(控制)视图对对象结构有更好的展示，帮助我们分析全局作用域(如 window)中对象引用情况来找到是什么保留了这些对象。它能让你分析闭包并深入到对象更深层去查看。

* Dominators(支配者)视图能用来帮助我们确认没有出乎意料的对象引用还挂在某个位置(如那些很好的包含了的)，和确认对象的删除/垃圾回收真正发挥了作用。
* Dominators view helps confirm that no unexpected references to objects are still hanging around (i.e that they are well contained) and that deletion/garbage collection is actually working.

** 问：堆分析仪中的constructor(一组)内容代表什么？**

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_39.jpg)

* **(global property)** - 全局对象(像 'window')和引用它的对象之间的中间对象。如果一个对象由构造函数Person生成并被全局对象引用，那么引用路径就是这样的：[global] > (global property) > Person。这跟一般的直接引用彼此的对象不一样。我们用中间对象是有性能方面的原因，全局对象改变会很频繁，非全局变量的属性访问优化对全局变量来说并不适用。

* **(roots)** - 保留树中root对象是引用了选中的对象的对象。它们也可以因为自身的目地而由引擎创建后被引用。这个引擎有引用。。。
* **(roots)** – The root entries in the retaining tree view are the entities that have references to the selected object. These can also be references created by the engine for its own purposes. The engine has caches which reference objects, but all such references are weak and won't prevent an object from being collected given that there are no truly strong references.

* **(closure)** - 一些函数闭包中的一组对象的引用

* **(array, string, number, regexp)** - 一组属性引用了Array,String,Number或正则表达式的对象类型

* **(compiled code)** - 简单来说，所有东西都与compoled code有关。Script像一个函数，但其实对应了&lt;script&gt;的内容。SharedFunctionInfos (SFI)是函数和compiled code之间的对象。函数通常有内容，而SFIS没有(什么鸟意思)。
* **(compiled code)** – simply, everything related to compiled code. Script is similar to a function but corresponds to a &lt;script&gt; body. SharedFunctionInfos (SFI) are objects standing between functions and compiled code. Functions are usually have a context, while SFIs do not.

* **HTMLDivElement**, **HTMLAnchorElement**, **DocumentFragment** 等 - 你代码中对elements或document对象的引用。

在你的程序的生命周期中生成的很多其它的对象，包括事件监听器或自定义对象，可以在下面的controllers中找到：

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_40.jpg)

** 问：我在做内存分析时需要关闭Chrome里可能会产生影响的什么功能么？**

我们建议在用Chrome DevTools做内存分析时，你可以使用所有扩展功能都关闭了的隐身模式，或[设置](http://www.chromium.org/developers/how-tos/run-chromium-with-flags)用户文件夹为(`--user-data-dir=""`)后再打开Chrome。

![](https://developer.chrome.com/devtools/docs/memory-profiling-files/image_41.jpg)

应用，扩展甚至console中的记录都会对你的分析有潜在的影响，如果你想让你的分析可靠的话，禁用这些吧。

**写在最后的话**

今天的JavaScript引擎已经有很强的自动回收我们的代码产生的内存垃圾的能力了。就是说，它们只能做到这样了，但我们的应用仍然被证明了会因为逻辑错误而产生内存泄漏。使用相应的工具来找到你的应用的瓶颈，记住，不要靠猜 - 测试它。

## 帮助实例

### 诊断内存泄漏

尽管很多内容在本文章中已经提到了，但一系列测试内存相关的问题的例子还是很有用的，下面是一组DOM节点内存泄漏的例子。你可能希望在测试你的更复杂的页面或应用前先用这些例子做试验。

* [Example 1: Growing memory](https://developer.chrome.com/devtools/docs/demos/memory/example1.html)

* [Example 2: Garbage collection in action](https://developer.chrome.com/devtools/docs/demos/memory/example2.html)

* [Example 3: Scattered objects](https://developer.chrome.com/devtools/docs/demos/memory/example3.html)

* [Example 4: Detached nodes](https://developer.chrome.com/devtools/docs/demos/memory/example4.html)

* [Example 5: Memory and hidden classes](https://developer.chrome.com/devtools/docs/demos/memory/example5.html)

* [Example 6: Leaking DOM nodes](https://developer.chrome.com/devtools/docs/demos/memory/example6.html)

* [Example 7: Eval is evil (almost always)](https://developer.chrome.com/devtools/docs/demos/memory/example7.html)

* [Example 8: Recording heap allocations](https://developer.chrome.com/devtools/docs/demos/memory/example8.html)

* [Example 9: DOM leaks bigger than expected](https://developer.chrome.com/devtools/docs/demos/memory/example9.html)

* [Example 10: Retaining path](https://developer.chrome.com/devtools/docs/demos/memory/example10.html)

* [Example 11: Last exercise](https://developer.chrome.com/devtools/docs/demos/memory/example11.html)

**更多例子：**

* [Gathering scattered objects](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-summary)

* [Verifying action cleanness](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-comparison)

* [Exploring the heap contents](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-containment)

* [Uncovering DOM leaks](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-dom-leaks)

* [Finding accumulation points](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-dominators)


## 社区资源

有很多社区写的杰出的资源关于用Chrome DevTools来定位和解决web apps内存问题。下面的一组资源可能对你有帮助：

* [Finding and debugging memory leaks with the Chrome DevTools](http://slid.es/gruizdevilla/memory)

* [JavaScript profiling with the DevTools](http://coding.smashingmagazine.com/2012/06/12/javascript-profiling-chrome-developer-tools/)

* [Effective memory management at GMail scale](http://www.html5rocks.com/en/tutorials/memory/effectivemanagement/)

* [Chrome DevTools Revolutions 2013](http://www.html5rocks.com/en/tutorials/developertools/revolutions2013/)

* [Rendering and memory profiling with the DevTools](http://www.slideshare.net/matenadasdi1/google-chrome-devtools-rendering-memory-profiling-on-open-academy-2013)

* [Performance optimization with DevTools timeline and profile](http://addyosmani.com/blog/performance-optimisation-with-timeline-profiles/)
