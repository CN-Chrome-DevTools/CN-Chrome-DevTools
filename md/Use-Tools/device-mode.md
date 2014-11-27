# Device 模块 & 手机模拟器

随着你移动端用户的增加，移动端优化的响应式设计越来越重要。网页需要在各种各样的设备和网络环境下表现良好。因此，你测试移动端用户体验就需要很多时间而且非常复杂。

Device 模块使用 Mobile 模拟器为你提供强大的移动端测试功能。 

  <figure>
      <video id="gfyVid1" class="gfyVid" controls autoplay loop muted poster="//thumbs.gfycat.com/LeadingBlackandwhiteBorzoi-poster.jpg">
        <source id="webmsource" src="http://fat.gfycat.com/LeadingBlackandwhiteBorzoi.webm" type="video/webm">
        <source id="mp4source" src="http://fat.gfycat.com/LeadingBlackandwhiteBorzoi.mp4" type="video/mp4">
        <img src="http://zippy.gfycat.com/LeadingBlackandwhiteBorzoi.gif" alt="Demoing device mode.">
      </video>
  </figure>
  
从上面你可以看到 Device 模块可以：

* 通过模拟不同的屏幕尺寸和分辨率来测试你的响应式设计，包括 Retina 测试。
* 通过网络模拟器来测试你站点的性能（低速网络），不会影响其他标签或者软件。
* 很方便的查看 CSS Media Queries 代码。
* 准确模拟设备触摸事件、地理位置以及设备翻转。
* 与之前 DevTools 整合在一起，符合你的调试习惯的前提下增强功能。

**注意：**文档中的一些技术可能并不存在于当前稳定版本的 Chrome 中。如果你无法使用某个特性，请尝试 [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html)，这个版本总是包含最新的 DevTools。

## 启用 Device 模块

点击 **Toggle device mode** ![image](https://developer.chrome.com/devtools/docs/device-mode-files/icon-device-mode-off.png) 图标，即可打开 Device 模块。当 Device 模块启用后，这个图标会变成蓝色同时页面会放进一个 Device 模拟器里面。

你也可以用下面快捷键来开关 Device 模块：`Ctrl + Shift + M`（Mac 上 `Cmd + Shift + M` ）。

![启用 Device 模块](https://developer.chrome.com/devtools/docs/device-mode-files/device-mode-initial-view.png)

## 使用屏幕仿真器

Device 模块的屏幕仿真器可以帮助你做响应式测试，而不需要拿着各种设备测试。

### 使用 Device 预设快速测试

为了方便调试，Device 模块预置了大量的设备的相关参数。在设备下拉列表里面选在一个设备预设，即可快速开始模拟测试。

![通过选择预设方案快速测试](https://developer.chrome.com/devtools/docs/device-mode-files/device-and-network-tools.png)

每一个预设包含以下几个配置：

* 相应的 UA 请求字符串
* 该设备的分辨率和像素比
* 启用 touch 模拟（如果该设备支持的话）
* 模拟手机滚动条和视口（viewport）
* 自动缩放（增大）页面上的文字

**提示：**开关屏幕分辨率模拟器使用 **Emulate screen resolution** ![](https://developer.chrome.com/devtools/docs/device-mode-files/icon-emulate-resolution.png) 复选框。切换横屏还是竖屏可以点击 **Swap dimensions** ![](https://developer.chrome.com/devtools/docs/device-mode-files/icon-swap-dimensions.png) 图标。勾选 **Fit** 复选框可以使整个模拟器都出现在你浏览器窗口中，会等比缩放以适应窗口大小。（这个功能只是为了方便观察，本质上没有什么变化。）

### 自定义屏幕设置

需要更详细的屏幕模拟，你可以自行修改 Device 预设下拉列表下面的分辨率设置。

![分辨率设置](https://developer.chrome.com/devtools/docs/device-mode-files/screen-controls.png)

在 width 和 height 表单中手动填写 CSS 像素数值来自定义屏幕尺寸。

如果你还想在非 Retina 设备测试 Retina 设备或者反过来测试，可以修改 **Device pixel ratio** ![](https://developer.chrome.com/devtools/docs/device-mode-files/icon-DPR.png) 表单。这个设像素比（DPR）就是逻辑像素和物理像素的比例。像 iPhone5 这样的 Retina 屏幕设备，有比普通设备更高的像素密度，因此会影响内容的尺寸和锐利度。

网络上一些与 DPR 有关的例子：

* CSS media queries 例如 `@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) { ... }`
* CSS [image-set](http://dev.w3.org/csswg/css-images/#image-set-notation) 规范。
* 图片上的 [srcset](http://www.w3.org/html/wg/drafts/html/master/embedded-content.html#attr-img-srcset) 属性。
* BOM 属性 `window.devicePixelRatio`。

**注意：**如果你有一个本身带有 Retina 屏幕的设备，你会看到低 dpi 时看起来有像素颗粒感高 dpi 时看起来锐利。在标准显示器上模拟这个效果，勾选 **Fit** 复选框并且设置 DPR 为 2。

### 保存自定义预设

保存你自定义的设置为预设可以让你下次很方便的使用这个。

你可以这样设置，点击浏览器视图右上角的 More overrides ![](https://developer.chrome.com/devtools/docs/device-mode-files/icon-open-emulator-drawer.png) 图标， 打开 DevTools emulation drawer。

![](https://developer.chrome.com/devtools/docs/device-mode-files/emulation-drawer-UI-location.png)

提示：你也可以摁下快捷键 `Esc` 来打开这个。

在 Device 面板，点击 Save as 然后命名一下即可。

![Device 面板](https://developer.chrome.com/devtools/docs/device-mode-files/emulation-drawer-device.png)

现在你可以快速的从 device 预设下拉列表里选择你自定义的模拟设置。

提示：这个 emulation 面板的功能与 device 模块界面上基本一致，增加了一些功能（media 类型模拟和传感器模拟功能）。

## 模拟网络情况

面对移动端用户，在各种复杂的网络情况下优化你的网站性能是十分关键的。

Device 模块的网络模拟功能可以让你测试你站点在复杂多变网络下的状态，包括 Edge，3G，以及离线。从预设列表里面选择一种连接方式，就可以启用网络丢包（network throttling）和强制等待（latency manipulation）功能。

![选择一个网络预设来模拟测试环境](https://developer.chrome.com/devtools/docs/device-mode-files/network-throttling.png)

网络丢包会限制最大的下载吞吐量（数据传输率）来模拟慢网速。强制等待会在连接中强制产生一个延迟（RTT）。

## 查看 media queries

Media queries 是响应式设计中必不可少的一部分。Device 模块让你可以轻松的查看 media queries 代码。

<video class="gfyVid" controls autoplay loop muted style="display: block;" poster="//thumbs.gfycat.com/OilyHarmlessAffenpinscher-poster.jpg">
            <source id="webmsource" src="//zippy.gfycat.com/OilyHarmlessAffenpinscher.webm" type="video/webm">
            <source id="mp4source" src="//fat.gfycat.com/OilyHarmlessAffenpinscher.mp4" type="video/mp4">
            <img src="http://zippy.gfycat.com/OilyHarmlessAffenpinscher.gif" alt="Inspecting media queries.">
        </video>

启用 media queries 查看器，你可以点击视图左上角的 Media queries ![Media query 查看器](https://developer.chrome.com/devtools/docs/device-mode-files/media-query-inspector-ruler.png) 图标。DevTools 会检测你样式表里面的 media queries 代码并且在上面标尺中用不同颜色条显示出来。

Media queries 的颜色定义遵循下面规则：

* （蓝色）查询一个最大宽度
* （绿色）查询某一个范围内的宽度
* （橙色）查询一个最小宽度

### 预览屏幕效果

点击 media query 工具栏来调整模拟器的分辨率就可以预览到目标屏幕尺寸下的样式效果。

### 查看CSS

右击工具栏可以查看这条 media query 是在哪里定义的，并且可以跳转到对应源代码位置。

![使用 media query 查看器来预览有关源 CSS 代码](https://developer.chrome.com/devtools/docs/device-mode-files/reveal-source-code.png)

提示：当你使用 media query 查看器的时候，你可能并不需要每次都使用手机模拟器。不退出 device 模块而关掉手机模拟器，你可以点击 Reset all overrides ![](https://developer.chrome.com/devtools/docs/device-mode-files/icon-reset-overrides.png) 图标然后刷新页面。

### 预览其他 media 类型的效果

Media query 查看器查看的目标是应用在屏幕上的 CSS。如果你想预览其他 media 类型的样式效果，例如 print，你可以在 Emulation 面板中的 media 设置。

通过点击浏览器视图右上角 More overrides ![](https://developer.chrome.com/devtools/docs/device-mode-files/icon-open-emulator-drawer.png) 图标打开 DevTools Emulation 面板。然后选择 Media 选项。

![media 面板](https://developer.chrome.com/devtools/docs/device-mode-files/emulation-drawer-media.png)

勾选 CSS media 复选框，然后选择下面下拉列表的一个 media 类型。

## 模拟设备传感器

因为大部分电脑都没有触摸屏幕，GPS，或者加速计，所以在你电脑上测试这些功能会很麻烦。Device 模块的传感器可以模拟常见的传感器功能来帮你测试。

打开 DevTools 模拟器面板，选择 Sensors 即可配置传感器参数。

![传感器面板](https://developer.chrome.com/devtools/docs/device-mode-files/emulation-drawer-sensors.png)

注意：如果你的 app 使用 JavaScript 探测传感器功能（例如 Modernizr），在设置启用传感器模拟之后，需要重新加载页面。

### 触发触摸（touch）事件

触摸屏模拟可以让你测试触摸时间和一系列你想用触摸设备测试的东西。

在传感器面板勾选 Emulate touch screen 复选框即可启用。

启用后，当你把鼠标放在模拟器视图的时候，手机图标会变成一个指尖大小的圆圈，同时触摸事件（诸如 touchstart，touchmove，和 touchend）会像在手机设备上那样被触发。

注意：如果想要触发 elem.ontouch * handlers，你必须在启动 chrome 的时候加上 --touch-events 命令行符。触摸模拟器目前并不支持默认触发这些 handlers。

<video class="gfyVid" controls autoplay loop muted poster='//thumbs.gfycat.com/DiligentEducatedAfricanhornbill-poster.jpg'>
            <source id="webmsource" src="//zippy.gfycat.com/DiligentEducatedAfricanhornbill.webm" type="video/webm">
            <source id="mp4source" src="//fat.gfycat.com/DiligentEducatedAfricanhornbill.mp4" type="video/mp4">
            <img src="http://zippy.gfycat.com/DiligentEducatedAfricanhornbill.gif" alt="Emulating pinch to zoom">
        </video>

提示：摁住 `Shift` 然后拖动鼠标可以模拟“捏”的操作。

因为鼠标事件在触摸设备上仍然可以被触发，所以触摸模拟器没有完全禁用鼠标事件。

### 模拟多点触控

你可以模拟设备上的多点触控事件，例如带触摸板的笔记本电脑等。了解更多关于设置多点触控模拟的信息，可以查看 [HTML5 Rocks](http://www.html5rocks.com/en/mobile/touch/#toc-touchdev) 上多点触控 web 开发指南中的“Developer Tools”章节。

提示：你可以使用 [fingerpaint touch screen demo](http://www.paulirish.com/demo/multi) 来测试这个功能。

### 设置地理数据

区别于桌面电脑，移动端设备通常使用 GPS 硬件来检测方位。在 Device 模块，你可以使用 [Geolocation API](http://www.w3.org/TR/geolocation-API/) 来模拟地理坐标。

勾选 Emulate geolocation coordinates 复选框可以启用地理模拟器。

![启用地理模拟器](https://developer.chrome.com/devtools/docs/device-mode-files/emulation-drawer-geolocation.png)

你可以使用这个模拟器为 navigator.geolocation 设置一个位置参数，这样当地理位置数据获取不到的时候，就可以使用这个模拟数据。

提示：使用这个 [maps demo](http://html5demos.com/geo) 来测试地理模拟器。

### 模拟设备倾斜

如果你需要测试 [Orientation API](http://www.w3.org/TR/screen-orientation/) ，需要使用加速计数据，你可以使用加速计模拟器来模拟这个数据。

勾选 Accelerometer 复选框可以启用加速计模拟器。

![](https://developer.chrome.com/devtools/docs/device-mode-files/emulation-drawer-accelerometer.png)

你可以设置下面几个方位参数：

* **α** z 轴的旋转
* **β** 从左向右倾斜
* **γ** 从前到后倾斜

你也可以直接点击并拖动右边模型来直接设置参数。

提示：你可以使用 [device orientation demo](http://www.html5rocks.com/en/tutorials/device/orientation/deviceorientationsample.html) 来测试加速计模拟器。

## 缺陷

尽管 Chrome 的 Device 模块提供了很多功能强大的模拟工具，但是还是有一些缺陷的。下面是已知问题。

* Device 硬件
	* GPU 和 CPU 行为并没有模拟。
* 浏览器 UI
	* 设备系统自带的元素，例如地址栏等，没有被模拟。
	* 系统原生效果，例如 `<select>` 标签，并没有被模拟成在设备上的效果。
	* 一些其他效果，例如在设备上输入数字的时候会弹出一个键盘等。这些效果在不同设备上可能会有不同的交互和行为，所以没法模拟。
* 浏览器功能
	* WebGL 可以在模拟器里模拟，但是在 iOS7 设备上它不再被支持。
	* MathML 在 Chrome 里不支持，但是在 iOS7 设备上支持。
	* [iOS 5 orientation zoom bug](https://github.com/scottjehl/device-bugs/issues/2) 并没有被模拟。
	* line-height CSS 属性不被 Opera mini 浏览器支持。
	* 部分 CSS 没有模拟，例如 [Internet Explorer](http://blogs.msdn.com/b/ieinternals/archive/2011/05/14/10164546.aspx) 里面的[这些属性](http://blogs.msdn.com/b/ieinternals/archive/2011/05/14/10164546.aspx)。
* 应用缓存
	* 模拟器还不能为应用缓存的 [manifest files](https://code.google.com/p/chromium/issues/detail?id=334120) 设置 UA，以及查看它的[源请求](https://code.google.com/p/chromium/issues/detail?id=119767)。

尽管有这些缺陷，但是 Device 模块的模拟器们已经足够应对大部分工作。当你需要测试真机的时候，你可以使用 DevTools 的[远程测试工具](https://developer.chrome.com/devtools/docs/remote-debugging)。





