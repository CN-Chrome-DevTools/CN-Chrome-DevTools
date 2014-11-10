# Device Mode & Mobile Emulation

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

**注意**: 文档中的一些技术可能并不存在于当前稳定版本的 Chrome 中。如果你无法使用某个特性，请尝试 [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html)，这个版本总是包含最新的 DevTools。

## 启用 Device 模块

点击 **Toggle device mode** ![image](https://developer.chrome.com/devtools/docs/device-mode-files/icon-device-mode-off.png) 图标，即可打开 Device 模块。当 Device 模块启用后，这个图标会变成蓝色同时页面会放进一个 Device 模拟器里面。

你也可以用下面快捷键来开关 Device 模块：`Ctrl + Shift + M`（Mac 上 `Cmd + Shift + M` ）。

![启用 Device 模块](https://developer.chrome.com/devtools/docs/device-mode-files/device-mode-initial-view.png)

