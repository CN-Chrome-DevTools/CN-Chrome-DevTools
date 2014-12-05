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
* **测试浏览器时：** 安卓 4.0+ 和 [Chrome for Android](https://play.google.com/store/apps/details?id=com.android.chrome&hl=en)。
* **测试 APP 时：**安卓 4.0+ 和[配置 WebView 应用](#debugging-webviews)。

**注意：**远程调试要求你桌面版的 Chrome 版本比你安卓设备上的 Chrome 更新。所以最好的选择是使用 [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html) （Mac、Windows）或者 Chrome [Dev channel](http://www.chromium.org/getting-involved/dev-channel) 发行版（linux）。

无论何时你在使用远程调试的时候遇到问题，可以参考[疑问答疑](#troubleshooting)部分。

## 设置你的安卓设备

想要使用远程调试，先按照下面说明来设置你的安卓设备。

1. 启用 USB 调试

在你的安卓设备上，打开 **设置 > 开发者选项**。

![开发者选项](https://developer.chrome.com/devtools/docs/remote-debugging/settings-dev-options-on.png)

**注意：** 在 Android 4.2 及更新版， 开发者选项默认隐藏。你需要敲击 **设置 > 关于手机** 里面的 **Build number** 七次，才可以启用开发者选项。

![在 Android 4.2+ 启用开发者选项](https://developer.chrome.com/devtools/docs/remote-debugging/about-phone-build-num.png)

在**开发者选项**里面，勾选 **USB 调试**复选框。

![在安卓设备上启用 USB 调试](https://developer.chrome.com/devtools/docs/remote-debugging/usb-debugging-on.png)

会出现一个弹窗询问你是否允许 USB 调试，选择 OK。

![](https://developer.chrome.com/devtools/docs/remote-debugging/allow-usb-debugging.png)

2. 连接你的设备

使用 USB 连接线把你的安卓设备连接到你的电脑上。

**注意：**如果你在 Windows 系统下开发，需要安装对应你设备的 USB 驱动程序。详情请看 Android Developer 网站的 [OEM USB Drivers](http://developer.android.com/tools/extras/oem-usb.html)。

## 在 Chrome 里面寻找你的设备

在安卓设备上设置好远程调试之后，在你的 Chrome 里面寻找设备。

打开你的桌面版 Chrome，在网址栏输入 **chrome://inspect**。确认 **Discover USB devices** 已经被选中：

![](https://developer.chrome.com/devtools/docs/remote-debugging/chrome-discover-usb.png)

提示：你也可以通过 **Chrome 菜单 > 更多工具 > 查看设备** 来打开 **chrome://inspect**。

在你的设备上会弹出一个对话框询问是否允许 USB 调试，点击 OK。

![image](https://developer.chrome.com/devtools/docs/remote-debugging/rsa-fingerprint.png)

提示：勾选**对此电脑总是允许**下次就不会提示了。

这时候在你设备的消息栏就会出现 **USB 调试已经连接**信息。

**注意：**在远程调试期间，Chrome 会阻止你设备进入睡眠。这个功能对于调试非常有帮助，但是也会有隐私泄露风险。所以确保你一直盯着你的设备！

在你电脑上，**chrome://inspect** 会显示每一个连接上的设备，以及它们打开了的浏览器标签和启用调试的 WebViews。

![查看已经连接的设备](https://developer.chrome.com/devtools/docs/remote-debugging/chrome-inspect-devices.png)

如果你 **chrome://inspect** 页面寻找你设备的时候遇到了问题，可以参考[疑问答疑](#troubleshooting)部分。

## 调试远程浏览器标签下的内容

在 **chrome://inspect** 中，你可以启用 DevTools 来调试你远程浏览器标签下的内容。

点击你想调试标签下面得 **inspect** 按钮来开始调试。

![](https://developer.chrome.com/devtools/docs/remote-debugging/chrome-inspect-tabs.png)

你电脑上会弹出一个新的 Chrome DevTools 窗口。在这个窗口，你可以实时的调试你设备上的页面。

![使用 Chrome Devtools 调试你安卓手机上的一个网页](https://developer.chrome.com/devtools/docs/remote-debugging/remote-debug-overview.jpg)

举个例子，使用 DevTools 可以在你设备上实现如下功能：

* 在 **Elements** 面板上把鼠标移动到一个对象上时，DevTools 会在你设备上高亮这个对象。
* 你也可以点击 DevTools 上 **Inspect Element** ![](https://developer.chrome.com/devtools/images/inspect-icon.png) 图标，然后敲击你设备上屏幕上的某一个地方。DevTools 会在 **Elements** 面板上高亮你敲击的对象。

**注意：**远程调试的时候使用的 DevTools 版本取决于你设备上的 Chrome 版本。所以，远程调试的 DevTools 可能看起来跟你平时使用的版本不同。

### 调试技巧

下面试一下调试过程中的小技巧：

* 在 DevTools 窗口中使用 `F5` （在 Mac 上是 `Cmd+r`）来刷新远程页面。
* 让设备在蜂窝网络上，使用 [Network panel](https://developer.chrome.com/devtools/docs/network) 来查看在实际移动网络的网络情况。
* 使用 [Timeline panel](https://developer.chrome.com/devtools/docs/timeline) 来查看页面渲染和 CPU 使用情况。由于硬件差异，移动端设备通常你比电脑运行慢很多。
* 如果你运行了一个本地的 web 服务器，使用 [端口转接](#port-forwarding) 或者 [虚拟 host 映射](#virtual-host-mapping) 让你的设备可以访问本地网站。

## 调试 WebViews

在 Android 4.4（KitKat）或者更新版，你可以使用 DevTools 来调试原生安卓应用中得 WebVies 内容。

### 配置 WebViews 来启用调试

必须要在你应用中设置才可以启用 WebView 的调试。你可以调用一个 WebView 类的静态方法 [setWebContentsDebuggingEnabled](http://developer.android.com/reference/android/webkit/WebView.html#setWebContentsDebuggingEnabled)。 

```
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
      WebView.setWebContentsDebuggingEnabled(true);
  }

```
这样就在应用里面所有的 WebViews 中启用调试。

**提示：**WebView 调试并不会被应用 manifest 中 debuggable 标志符状态的影响。如果你仅仅在 debuggable 为 true 时启用 WebView 调试，需要判断一下。

```
  if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    if (0 != (getApplicationInfo().flags &= ApplicationInfo.FLAG_DEBUGGABLE))
    { WebView.setWebContentsDebuggingEnabled(true); }
  }
```

### 在 DevTools 打开一个 WebView

**chrome://inspect** 页面中会显示你设备中启用调试的 WebViews。

点击下面的 **inspect** 来启用调试。就可以像调试[远程浏览器标签下内容](#debugging-tabs)一样调试了。

![使用 Chrome DevTols 调试安卓应用中得 WebView](https://developer.chrome.com/devtools/docs/remote-debugging/webview-debugging.png)

上图中 WebView 旁边的灰色图表示相对于它相对于设备屏幕的尺寸和位置。如果你的 WebView 设置了 title，title 的内容也会列出来。

## 实时截屏

老是在两个屏幕中来回看并不是很方便。这个功能截取你设备的屏幕显示在电脑上 DevTools 的左边。你也可以操作这个截屏来与你的设备进行交互。

从 KitKat 4.4.3 开始，截屏功能在浏览器标签和安卓 WebViews 下都可以使用。

### 打开截屏功能

在远程调试 DevTools 窗口的右上角，点击 **Screencast** ![](https://developer.chrome.com/devtools/docs/remote-debugging/icon-screencast.png) 图标即可开启截屏功能。

![启用截屏按钮](https://developer.chrome.com/devtools/docs/remote-debugging/screencast-icon-location.png)

截屏面板就会在左边打开并且返回设备屏幕的实时状态。

![电脑和安卓设备具有一致的效果](https://developer.chrome.com/devtools/docs/remote-debugging/screencast.png)

截屏仅仅显示主体内容。工具栏、键盘或者其他设备上的界面会变成透明状。

**注意：**因为截屏会持续的截取屏幕帧，会带来一些性能方面的影响。如果你的测试对帧速率比较敏感，请禁用截屏功能。

### 使用截屏功能与你的设备交互

当你操作截屏视图的时候，单击会被改变成敲击，在设备上触发对应的 touch 时间。敲击键盘也会反映到你的设备上，这样你就可以在电脑上往手机上输入了内容了。

其他 DevTools 功能也可以在截屏视图上有所反应。例如，单击 **Inspect Element** ![](https://developer.chrome.com/devtools/images/inspect-icon.png) 图标，然后在截屏视图上也可以选择一个元素。

<iframe width="640" height="360" src="//www.youtube.com/embed/Q7rEFEMpwe4" frameborder="0" allowfullscreen></iframe>

提示：可以通过按住 `Shift` 同时拖动来模拟一个捏的操作。使用你的触摸板或者鼠标滚珠可以实现滚动。

## 端口转移

你的手机不一定每次都可以访问你的开发环境服务器。它可能在不同网络下使用。更多的，你还有可能在一个公共的公司网络开发。

Chrome 的端口转移功能可以非常简单的让你手机测试你开发的网站。工作原理就是在你移动端设备上创建一个监听的 TCP 端口然后映射到你开发机器上的某个 TCP 端口。两个端口的数据连接通过 USB 连接线，所以不会被你的网络情况影响。

按照如下操作启用端口转接：

1. 在你开发机器上打开 **chrome://inspect**。
2. 点击 **Port Forwarding**。会出现配置菜单
	![](https://developer.chrome.com/devtools/docs/remote-debugging/chrome-port-forwarding.png)

3. 在 **Device port** 表单，输入你想要你安卓设备监听的端口号。（默认端口 8080）
4. 在 **Host** 表单，填写你 Web 应用的 IP 地址（或者主机名）和端口号。这个地址可以是任何你开发机器可访问的本地地址。当然，端口号必须在 1024 - 65535 之间。
5. 点击 **Enable port forwarding**。
6. 点击 **Done**。
	![端口转移设置](https://developer.chrome.com/devtools/docs/remote-debugging/port-forwarding-dialog.png)

当端口转移正常工作的时候，**chrome://inspect** 页面中端口状态显示器会显示成绿色。

![使用端口转接让你的安卓设备访问你本地 Web 服务器的内容](https://developer.chrome.com/devtools/docs/remote-debugging/port-forwarding-on-device.png)

现在你可以打开一个新的标签，然后在你的设备上查看你本地服务器上的内容。

## 虚拟 host 映射

当你在 localhost 本地开发的时候，端口转接工作起来很好。但是当你使用自定义本地域名的时候，可能就会遇到一些问题。

举个例子，你需要的第三方 JavaScript SKD 只能工作在某个白名单域名列表里面，这样你需要添加绑定一条类似 127.0.0.1 production.com 这样的域名到你的 [hosts 文件](http://en.wikipedia.org/wiki/Hosts_(file)) 里面。或者是你在你的 Web 服务器（[MAMP](http://www.mamp.info/en/)）使用虚拟 hosts 功能设置了一个自定义域名。

如果你需要让你的手机能访问自定义域名的内容，你可以使用域名转接和代理服务器实现。这个代理服务器将你设备的请求映射到你正确的 Host 主机上面。

### 设置端口转接到一个代理服务

虚拟 host 映射需要你在 host 机器上运行一个代理服务器。所有你安卓设备的请求都会被转移到这个代理上。

按照如下步骤来设置端口转移到一个代理服务器上：

1. 在 host 机器上，安装代理软件，例如 [Charles](http://www.charlesproxy.com/) 或者 [Squid](http://www.squid-cache.org/)。
2. 启用代理服务器并且注意启用的端口是否被占用。
	**注意：**这个代理服务器和你本地开发服务器必须运行在不同的端口上。
3. 打开 **chrome://inspect** 页面。
4. 点击 **Port forwarding**。配置端口转移设置。
5. 在 **Device port** 表单，输入你想要你安卓设备监听的端口号。要使用一个安卓允许的端口，比如 9000。
6. 在 **Host** 表单，输入 localhost:xxxx，xxxx 就是你代理服务器运行的端口号。
7. 勾选 **Enable port forwarding**。
8. 点击 **Done**。

![端口转接到一个代理服务器](https://developer.chrome.com/devtools/docs/remote-debugging/port-forward-to-proxy.png)

The proxy on the host machine is set up to make requests on behalf of your Android device.

### 在你设备上配置代理选项

你的安卓设备需要连接你 host 机器上的代理服务器。

按照如下步骤在你设备上设置代理：

1. 找到 **设置 > Wi-Fi**。
2. 长按你现在连接的网络。**注意：**代理设置会对每个网络都启用。
3. 敲击 **Modify network**。
4. 勾选 **Advanced options**。打开代理设置选项。
	![设备上的代理选项](https://developer.chrome.com/devtools/docs/remote-debugging/phone-proxy-settings.png)
5. 敲击 **Proxy** 选项然后选择 **Manual**。
6. 在 **Proxy hostname** 表单，输入 localhost。
7. 在 **Porxy port** 表单，输入 9000.
8. 敲击 **Save**。

设置完之后，你的设所有得请求都会被转移到 host 机器的代理服务器上。这个代理服务器代表了你的设备，所以对你自定义域名的请求就被正确的返回了。

现在你就可以像在本地打开一样，在安卓的 chrome 上面打开一个本地的域名。

![在安卓设备上使用虚拟 host 映射来访问一个自定义的本地域名](https://developer.chrome.com/devtools/docs/remote-debugging/virtual-host-mapping.png)

提示：想要恢复设备正常的浏览，记住在与 host 断开连接后在你设备上恢复代理设置。


## 疑问解答

**在 chrome://inspect 页面无法看到我的设备。**

* 如果你基于 Windows 开发，检查你是否安装了你设备对应的 USB 驱动。详情见 Android Developers 网站上的 [OEM USB Drivers](http://developer.android.com/tools/extras/oem-usb.html)。
* 检查设备是否直接连接到你的电脑，避免通过集线器连接。
* 检查 **USB debugging** 是否在你的设备上启用了。别忘了在你设备上弹出的 是否允许 USB 调试对话框 上点击确认。
* 在你桌面版浏览器上打开 **chrome://inspect** 页面并检查是否勾选 **Discover USB devices**。
* 远程调试需要你桌面版的 Chrome 版本要比你安卓设备上的 Chrome 更新。试试 [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html)（Mac/Windows） 或者 Chrome [Dev channel](http://www.chromium.org/getting-involved/dev-channel) 桌面发行版（Linux）。

如果你仍然无法发现你的设备，拔下来。在你设备上，找到 **设置 > 开发者选项**，敲击 **Revoke USB debugging authorization**。然后重试[设置你的安卓设备](#setting-up-device)和[寻找 USB 设备](#discovering-devices)步骤。

**在 chrome://inspect 页面无法看到我浏览器的标签。**

* 在你的设备上，打开 Chrome 浏览器同时打开你想要调试的网页。然后刷新 **chrome://inspect** 页面。

**在 chrome://inspect 页面无法看到我 WebViews 内容**

* 查看你的 app 是否启用了 [WebView 调试](#debugging-webviews)。
* 在你设备上，打开你想要调试 WebView 的 app。然后刷新 **chrome://inspect** 页面。

**在我的安卓设备上无法访问我得 Web 服务器**

* 如果网络限制了你的移动端设备访问你开发服务器，试一下[端口转接](#port-forwarding)或者设置[虚拟 host 映射](#virtual-host-mapping);

最后，如果远程调试仍然不起作用，你可以通过 Android SDK 里面的 adb 程序使用[老方法](https://developer.chrome.com/devtools/docs/remote-debugging-legacy)来调试。

## 补充信息

### 远程调试和 ADB

你再也不需要配置 ADB 或者 ADB 插件来调试远程浏览器标签下内容和 WebViews。针对安卓的远程调试现在是 Chrome DevTools 的一部分了。而且可以在所有操作系统中使用：Windows，Mac，Linux 以及 Chrome OS。

如果你远程调试遇到了问题，你可以通过 Android SDK 里面的 adb 程序再试试[老方法](https://developer.chrome.com/devtools/docs/remote-debugging-legacy)。

**注意：**Chrome 和你设备的 USB 连接可能会大大unni的 adb 连接。在创建你 adb 连接之前，在 **chrome://inspect** 中取消掉 **Discover USB devices**。然后插拔一下设备。

### 针对 DevTools 扩展开发者的远程调试资料

想要获取更多的远程调试交互协议，可以参考 [Debugger Protocol](https://developer.chrome.com/devtools/docs/debugger-protocol) 文档和 [chrome.debugger](https://developer.chrome.com/extensions/debugger)。