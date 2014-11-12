# Device 模块 & 手机模拟器

https://developer.chrome.com/devtools/docs/device-mode

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
* 通过 Network Emulator 来测试你站点的性能（低速网络），而不会影响其他标签。
* 很方便的查看 CSS Media Queries 代码。
* 准确模拟设备触摸事件、地理位置以及设备翻转。
* 与之前 DevTools 整合在一起，保留你的调试习惯，增强功能。

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

**提示：**开关屏幕分辨率模拟器使用 **Emulate screen resolution** ![](https://developer.chrome.com/devtools/docs/device-mode-files/icon-emulate-resolution.png) 复选框。切换横屏还是竖屏可以点击 **Swap dimensions** ![](https://developer.chrome.com/devtools/docs/device-mode-files/icon-swap-dimensions.png) 图标。勾选 **Fit** 复选框可以使整个模拟器都出现在你浏览器窗口中，会等比缩放以适应窗口大小。（这个功能只是为了方便，效果跟之前一样。）



