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

## Views in detail

### Summary view

Initially, a snapshot opens in the Summary view, displaying object totals, which can be expanded to show instances:

![](memory-profiling-files/image_19.png)

Top-level entries are "total" lines. They display:

* the **Constructor represents **all objects created using this constructor

* the **number of object instances** is displayed in the # column

* the **Shallow size** column displays the sum of [shallow sizes](https://developers.google.com/chrome-developer-tools/docs/memory-analysis-101.html#object_sizes) of all objects created by a certain constructor function

* the **Retained size** column displays the maximum retained size among the same set of objects

* the **Distance **displays the distance to the root using the shortest simple path of nodes.

After expanding a total line in the upper view, all of its instances are displayed. For each instance, its shallow and retained sizes are displayed in the corresponding columns. The number after the @ character is the objects’ unique ID, allowing you to compare heap snapshots on per-object basis.

<p class="note"><strong>Example:</strong> Try this <a href="https://developers.google.com/chrome-developer-tools/docs/heap-profiling-summary">demo page</a> (opens in a new tab) to understand how the Summary view can be used.</p>

Remember that yellow objects have JavaScript references on them and red objects are detached nodes which are referenced from one with a yellow background.

### Comparison view

This view is used to compare multiple snapshots to each other so that you can see what the difference between them are in order to find leaked objects. To verify that a certain application operation doesn't create leaks (e.g. usually a pair of direct and reverse operations, like opening a document, and then closing it, should not leave any garbage), you may follow the scenario below:

1. Take a heap snapshot before performing an operation;

2. Perform an operation (interact with a page in some way that you believe to be causing a leak);

3. Perform a reverse operation (do the opposite interaction and repeat it a few times);

4. Take a second heap snapshot and change the view of this one to Comparison, comparing it to snapshot 1.

In the Comparison view, the difference between two snapshots is displayed. When expanding a total entry, added and deleted object instances are shown:

![](memory-profiling-files/image_21.png)

<p class="note"><strong>Example:</strong> Try this <a href="https://developers.google.com/chrome-developer-tools/docs/heap-profiling-comparison">demo page</a> (opens in a new tab) to get an idea how to use snapshot comparison for detecting leaks.</p>

### Containment view

The Containment view is essentially a "bird's eye view" of your application's objects structure. It allows you to peek inside function closures, to observe VM internal objects that together make up your JavaScript objects, and to understand how much memory your application uses at a very low level.

The view provides several entry points:

* **DOMWindow**** objects** — these are objects considered as "global" objects for JavaScript code;

* **GC roots** — actual GC roots used by VM's garbage collector;

* **Native objects** — browser objects that are "pushed" inside the JavaScript virtual machine to allow automation, e.g. DOM nodes, CSS rules (see the next section for more details.)

Below is the example of a populated Containment view:

![](memory-profiling-files/image_22.png)

<p class="note">
  <strong>Example:</strong> Try this <a href="https://developers.google.com/chrome-developer-tools/docs/heap-profiling-containment">demo page</a> (opens in a new tab) for finding out how to explore closures and event handlers using the view.
</p>

<strong>A tip about closures</strong>

It helps a lot to name the functions so you can easily distinguish between closures in the snapshot. For example, this example does not use named functions:

<pre>
function createLargeClosure() {
  var largeStr = new Array(1000000).join('x');

  var lC = function() { // this is NOT a named function
    return largeStr;
  };

  return lC;
}
</pre>

Whilst this example does:

<pre>
function createLargeClosure() {
  var largeStr = new Array(1000000).join('x');

  var lC = function lC() { // this IS a named function
    return largeStr;
  };

  return lC;
}
</pre>

<img src="memory-profiling-files/domleaks.png"/></a>

<p class="note">
    <strong>Examples:</strong>
    Try out this example of <a href="/chrome-developer-tools/docs/demos/memory/example7.html">why eval is evil</a> to analyze the impact of closures on memory. You may also be interested in following it up with this example that takes you through recording <a href="/chrome-developer-tools/docs/demos/memory/example8.html">heap allocations</a>.
</p>

### Uncovering DOM leaks

A unique ability of the tool is to reflect bidirectional dependencies between browser native objects (DOM nodes, CSS rules) and JavaScript objects. This helps to discover otherwise invisible leaks happening due to forgotten detached DOM subtrees floating around.

DOM leaks can be bigger than you think. Consider the following sample - when is the #tree GC?

<pre>
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
</pre>

<code>#leaf</code> maintains a reference to it's parent (parentNode) and recursively up to <code>#tree</code>, so only when leafRef is nullified is the WHOLE tree under <code>#tree</code> a candidate for GC.

<img src="memory-profiling-files/treegc.png"/>

<p class="note">
    <strong>Examples:</strong>
    Try out this example of <a href="/chrome-developer-tools/docs/demos/memory/example6.html">leaking DOM nodes</a> to understand where DOM nodes can leak and how to detect them. You can follow it up by also looking at this example of <a href="/chrome-developer-tools/docs/demos/memory/example9.html">DOM leaks being bigger than expected</a>.
</p>

<div class="drop-shadow extdoc">

<p>To read more about DOM leaks and memory analysis fundamentals checkout <a href="http://slid.es/gruizdevilla/memory">Finding and debugging memory leaks with the Chrome DevTools</a> by Gonzalo Ruiz de Villa.</p>

</div>

Native objects are most easily accessible from Summary and Containment views — there are dedicated entry nodes for them:

![](memory-profiling-files/image_24.png)

<p class="note">
    <strong>Example:</strong>
    Try this <a href="https://developers.google.com/chrome-developer-tools/docs/heap-profiling-dom-leaks">demo</a> (opens in a new tab) to play with detached DOM trees.
</p>

### Dominators view

The Dominators view shows the dominators tree for the heap graph. The Dominators view looks similar to the Containment view, but lacks property names. This is because a dominator of an object may lack direct references to it, that is, the dominators tree is not a spanning tree of the graph. But this only serves for good, as helps us to identify memory accumulation points quickly.

<p class="note"><strong>Note:</strong> In Chrome Canary, Dominators view can be enabled by going to Settings > Show advanced heap snapshot properties and restarting the DevTools.</p>

![](memory-profiling-files/image_25.png)

<p class="note">
    <strong>Examples:</strong>
    Try this <a href="https://developers.google.com/chrome-developer-tools/docs/heap-profiling-dominators">demo</a> (opens in a new tab) to train yourself in finding accumulation points. Follow it up with this example of running into <a href="/chrome-developer-tools/docs/demos/memory/example10.html">retaining paths and dominators</a>.
</p>

## Object allocation tracker

The **object tracker** combines the detailed snapshot information of the [heap profiler](#heading=h.xfxcns9xlif4) with the incremental updating and tracking of the Timeline panel. Similar to these tools, tracking objects’ heap allocation involves starting a recording, performing a sequence of actions, then stop the recording for analysis.

The object tracker takes heap snapshots periodically throughout the recording (as frequently as every 50 ms!) and one final snapshot at the end of the recording. The heap allocation profile shows where objects are being created and identifies the retaining path.

![](memory-profiling-files/image_26.png)

**Enabling and using the Object Tracker**

To begin using the Object Tracker:

1. Make sure you have the latest [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html).

2. Open the Developer Tools and click on the gear icon in the lower right.

3. Now, open the Profiler panel, you should see a profile called "Record Heap Allocations"

![](memory-profiling-files/image_27.png)

The bars at the top indicate when new objects are found in the heap.  The height of each bar corresponds to the size of the recently allocated objects, and the color of the bars indicate whether or not those objects are still live in the final heap snapshot: blue bars indicate objects that are still live at the end of the timeline, gray bars indicate objects that were allocated during the timeline, but have since been garbage collected.

<img src="memory-profiling-files/collected.png"/></a>

In the example above, an action was performed 10 times.  The sample program caches five objects, so the last five blue bars are expected.  But the leftmost blue bar indicates a potential problem. You can then use the sliders in the timeline above to zoom in on that particular snapshot and see the objects that were recently allocated at that point.

![](memory-profiling-files/image_29.png)

Clicking on a specific object in the heap will show its retaining tree in the bottom portion of the heap snapshot. Examining the retaining path to the object should give you enough information to understand why the object was not collected, and you can make the necessary code changes to remove the unnecessary reference.

## Memory Profiling FAQ

**Q: I don't see all the properties of objects, I don't see non-string values for them! Why?**

Not all properties are actually stored on the JavaScript heap. Some of them are implemented using getters that execute native code. Such properties are not captured in heap snapshots in order to avoid the cost of calling getters and to avoid possible program state changes if getters are not "pure" functions. Also, non-string values such as numbers are not captured in an attempt to reduce snapshot size.

**Q: What does the number after the ****@**** char mean — is this an address or an ID? Is the ID value really unique?**

This is an object ID. Displaying an object's address makes no sense, as objects are moved during garbage collections. Those object IDs are real IDs — that means, they persist among multiple snapshots taken and are unique. This allows precise comparison between heap states. Maintaining those IDs adds an overhead to GC cycles, but it is only initiated after the first heap snapshot was taken — no overhead if heap profiles aren't used.

**Q: Are "dead" (unreachable) objects included in snapshots?**

No. Only reachable objects are included in snapshots. Also, taking a snapshot always starts with doing a GC.

<p class="note"><strong>Note:</strong>At the time of writing, we are planning on avoiding this GC to reduce the drop in used heap size when taking heap snapshots. This has yet to be implemented but garbage would still be out of the snapshot.</p>

**Q: What comprises GC roots?**

Many things:

* built-in object maps;

* symbol table;

* stacks of VM threads;

* compilation cache;

* handle scopes;

* global handles.

![](memory-profiling-files/image_30.jpg)

**Q: I’ve been told to use the Heap Profiler and Timeline Memory view for detecting memory leaks. What tool should be used first?**

The Timeline. Use it to diagnose excessive memory usage when you first notice your page has slowed down after extended use. Slowdown was once a classic symptom of a memory leak but it could also be something else – maybe you have a paint or network bottleneck in your page, so make sure to fix the real issue in your page.

To diagnose whether memory is the issue, go to the Timeline panel and Memory view. Hit the record button and interact with your application, repeating any steps you feel may be causing a leak. Stop the recording. The graph you see will display the memory allocated to your application. If it happens to be consuming an increasing amount of this over time (without ever dropping), it’s an indication you may have a memory leak.

The profile for a healthy application should look more like a sawtooth curve as memory is allocated then freed when the garbage collector comes in. There’s nothing to worry about here – there’s always going to be a cost of doing business in JavaScript and even an empty `requestAnimationFrame` will cause this type of sawtooth, you can’t avoid it. Just ensure it’s not sharp as that’s an indication a lot of allocations are being made, which can equate to a lot of garbage on the other side.

![](memory-profiling-files/image_31.png)

It's the rate of increase in the steepness of this curve that you need to keep an eye on.There is also a DOM node counter, Document counter and Event listener count in the Memory view which can be useful during diagnosis. DOM nodes use native memory and do not directly affect the JavaScript memory graph.

![](memory-profiling-files/image_32.jpg)

Once you suspect you have a memory leak, the Heap profiler can be used to discover the source of the leak.

**Q: I noticed a number of DOM nodes in the heap snapshot where some are highlighted in red and indicated as a "Detached DOM tree" whilst others are yellow. What does this mean?
**

You'll notice nodes of a few different colors. Red nodes (which have a darker background) do not have direct references from JavaScript to them, but are alive because they’re part of a detached DOM tree. There may be a node in the tree referenced from JavaScript (maybe as a closure or variable) but is coincidentally preventing the entire DOM tree from being garbage collected.

![](memory-profiling-files/image_33.jpg)

Yellow nodes (with a yellow background) however do have direct references from JavaScript. Look for yellow nodes in the same detached DOM tree to locate references from your JavaScript. There should be a chain of properties leading from the DOM window to the element (e.g `window.foo.bar[2].baz`).

An animation of where detached nodes fit into the overall picture can be seen below:

<img src="memory-profiling-files/detached-nodes.gif" style="max-width:900px"/>

<p class="note">
    <strong>Example:</strong>
    Try out this example of <a href="/chrome-developer-tools/docs/demos/memory/example4.html">detached nodes</a> where you can watch node evolution in the Timeline then take heap snapshots to find detached nodes.
</p>

**Q: What do the Shallow and Retained Size columns represent and what are the differences between them?**

So, objects can be kept in memory (be alive) in two different ways – either directly by another alive object (window and document are always alive objects) or implicitly by holding references from native part of the renderer (like DOM objects). The latter is what ends up preventing these objects from being disposed by GC automatically, causing leaks. The size of memory held by an object itself is known as the shallow size (generally, arrays and strings have larger shallow sizes).

![](memory-profiling-files/image_36.jpg)

An object of any size can hold a ton of memory if it prevents other objects from being disposed. The size of memory that can be freed once an object is deleted (and this its dependents made no longer reachable) is called the retained size.

**Q: There's a lot of data in the constructor and retained views. Where should I start digging into to discover if I have a leak?**

It's generally a good idea to begin investigation from the first object retained in your tree as retainers are sorted by distance (well, distance to the window).

![](memory-profiling-files/image_37.jpg)

The object retained with the shortest distance is usually your first candidate for causing a memory leak.

**Q: What's the difference between the different Summary, Comparison, Dominators and Containment views?**

You may get some mileage by switching between the different data views available at the bottom of the screen.

![](memory-profiling-files/image_38.jpg)

* Summary view helps you hunt down objects (and their memory use) based on type grouped by constructor name. This view is particularly helpful for tracking down DOM leaks.

* Comparison view helps you track down memory leaks, by displaying which objects have been correctly cleaned up by the garbage collector. Generally used to record and compare two (or more) memory snapshots of before and after an operation. The idea is that inspecting the delta in freed memory and reference count lets you confirm the presence and cause of a memory leak.

* Containment view provides a better view of object structure, helping us analyse objects referenced in the global namespace (i.e. window) to find out what is keeping them around. It lets you analyse closures and dive into your objects at a low level.

* Dominators view helps confirm that no unexpected references to objects are still hanging around (i.e that they are well contained) and that deletion/garbage collection is actually working.

**Q: What do the various constructor (group) entries in the Heap profiler correspond to?****
**

![](memory-profiling-files/image_39.jpg)

* **(global property)** – intermediate objects between a global object (like 'window') and an object referenced by it. If an object is created using a constructor Person and is held by a global object, the retaining path would look like [global] > (global property) > Person. This contrasts with the norm, where objects directly reference each other. We have intermediate objects for performance reasons. Globals are modified regularly and property access optimisations do a good job for non-global objects aren't applicable for globals.

* **(roots)** – The root entries in the retaining tree view are the entities that have references to the selected object. These can also be references created by the engine for its own purposes. The engine has caches which reference objects, but all such references are weak and won't prevent an object from being collected given that there are no truly strong references.

* **(closure)** – a count of references to a group of objects through function closures

* **(array, string, number, regexp)** – a list of object types with properties which reference an Array, String, Number or regular expression

* **(compiled code)** – simply, everything related to compiled code. Script is similar to a function but corresponds to a &lt;script&gt; body. SharedFunctionInfos (SFI) are objects standing between functions and compiled code. Functions are usually have a context, while SFIs do not.

* **HTMLDivElement**, **HTMLAnchorElement**, **DocumentFragment** etc – references to elements or document objects of a particular type referenced by your code.

Many of the other objects you may see were likely generated during the lifecycle of your code and can include event listeners as well as custom objects, like the controllers below:

![](memory-profiling-files/image_40.jpg)

**Q: Is there anything I should be turning off in Chrome that might be influencing my figures?**

When performing any type of profiling using the Chrome DevTools, it is recommended that you either run in incognito mode with all extensions disabled or start Chrome with a [custom](http://www.chromium.org/developers/how-tos/run-chromium-with-flags) user data directory (`--user-data-dir=""`).

![](memory-profiling-files/image_41.jpg)

Apps, extensions and even console logging can have an implicit impact on your figures and you want to keep them as reliable as possible.

**Closing remarks**

The JavaScript engines of today are highly capable of automatically cleaning garbage generated by our code in a number of situations. That said, they can only go so far and our applications are still prone to memory leaks caused by logical errors. Use the tools available to find out your bottlenecks and remember, don't guess it - test it.

## Supporting Demos

### Debugging Memory Leaks

Although we've mentioned them throughout this guide, a good set of end-to-end examples for testing various memory issues, ranging from growing memory leaking DOM nodes can be found summarized below. You may wish to experiment with them before attempting to use the tooling on your own more complex page or application.

<ul>
<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example1.html">Example 1: Growing memory</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example2.html">Example 2: Garbage collection in
action</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example3.html">Example 3: Scattered objects</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example4.html">Example 4: Detached nodes</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example5.html">Example 5: Memory and hidden
classes</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example6.html">Example 6: Leaking DOM nodes</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example7.html">Example 7: Eval is evil (almost
always)</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example8.html">Example 8: Recording heap
allocations</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example9.html">Example 9: DOM leaks bigger than
expected</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example10.html">Example 10: Retaining path</a></li>

<li><a target="_blank" href="/chrome-developer-tools/docs/demos/memory/example11.html">Example 11: Last exercise</a></li>
</ul>

Additional demos are available for:

* [Gathering scattered objects](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-summary)

* [Verifying action cleanness](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-comparison)

* [Exploring the heap contents](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-containment)

* [Uncovering DOM leaks](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-dom-leaks)

* [Finding accumulation points](https://developers.google.com/chrome-developer-tools/docs/heap-profiling-dominators)


## Community Resources

There are a number of excellent resources written by the community on finding and fixing memory issues in web apps using the Chrome DevTools. Below are a selection of some you may find useful:

* [Finding and debugging memory leaks with the Chrome DevTools](http://slid.es/gruizdevilla/memory)

* [JavaScript profiling with the DevTools](http://coding.smashingmagazine.com/2012/06/12/javascript-profiling-chrome-developer-tools/)

* [Effective memory management at GMail scale](http://www.html5rocks.com/en/tutorials/memory/effectivemanagement/)

* [Chrome DevTools Revolutions 2013](http://www.html5rocks.com/en/tutorials/developertools/revolutions2013/)

* [Rendering and memory profiling with the DevTools](http://www.slideshare.net/matenadasdi1/google-chrome-devtools-rendering-memory-profiling-on-open-academy-2013)

* [Performance optimization with DevTools timeline and profile](http://addyosmani.com/blog/performance-optimisation-with-timeline-profiles/)
