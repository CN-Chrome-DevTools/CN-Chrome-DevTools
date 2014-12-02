# Remote Debugging on Android with Chrome

https://developer.chrome.com/devtools/docs/remote-debugging

手机上浏览你的 Web 网页的感觉跟你在桌面版的体验可能完全不同。Chrome DevTools 的远程调试功能可以让你用电脑连接你的安卓设备实时调试。

![](https://developer.chrome.com/devtools/docs/remote-debugging/remote-debug-banner.png)

安卓远程调试支持：

* 在[浏览器标签](#debugging-tabs)里面调试网站。
* 在原生安卓应用里面调试[WebView](#debugging-webviews)。
* 可以实时[截取](#screencasting)安卓设备屏幕显示在你电脑上。
* 可以通过[端口连接](#port-forwarding)和[虚拟 host 映射](#virtual-host-mapping)来连接你的电脑和安卓设备。


## 必要条件

想要开始远程调试，你需要：

* Chrome 32 或者更新版。
* 一根 USB 线，用来连接你的安卓设备。
* **测试浏览器：** 安卓 4.0+ 和 [Chrome for Android](https://play.google.com/store/apps/details?id=com.android.chrome&hl=en)。
* **测试 APP：**安卓 4.0+ 和[配置 WebView 应用](#debugging-webviews)。

**注意：**远程调试要求你桌面版的 Chrome 版本比你安卓设备上的 Chrome 更新。所以最好的选择是使用 [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html) （Mac、Windows）或者 Chrome [Dev channel](http://www.chromium.org/getting-involved/dev-channel) 发行版（linux）。

无论何时你在使用远程调试的时候遇到问题，可以参考[疑问答疑](#troubleshooting)部分。