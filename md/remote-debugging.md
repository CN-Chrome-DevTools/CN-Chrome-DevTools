Remote Debugging Chrome on Android
==

#### **Contents**

*   [ Remote debugging overview ](#remote-debugging-overview)

        *   [ Setting up your device ](#setting-up-device)

                *   [ Connecting directly over USB ](#remote-debugging)
    *   [ Connect your device](#connect-device-via-usb)*   [ Debug your Application ](#debug-your-app)
*   [Screencasting your device's screen ](#screencasting)

        *   [Interacting with screencast](#interacting-with-screencast)
*   [Debugging Android WebViews ](#debugging-webviews)

        *   [ Configure WebViews for debugging](#configure-webview)*   [ Open a WebView in DevTools ](#open-webview)
*   [ Reverse port forwarding](#reverse-port-forwarding)

        *   [ Connect your mobile device ](#connect-your-mobile-device)*   [ Enable reverse port forwarding ](#enable-reverse-port-forwarding)*   [ Add a port forwarding rule](#add-a-port-forwarding-rule)

**Note**: For information on the interaction protocol we use for our remote debugging, please see the [Debugger Protocol](/chrome-developer-tools/docs/debugger-protocol) documentation and [chrome.debugger](http://developer.chrome.com/extensions/debugger.html).

## Remote debugging

The experience of your web content on mobile operates very differently than what users experience on the desktop.  The Google Chrome DevTools allow you to inspect, debug, and analyze the on-device experience with the full suite of tools you're used to, meaning you can use the Chrome DevTools on your development machine to debug a page on your mobile device.

<div class="screenshot">![Debugging Chrome for Android using the Chrome Developer Tools](remote-debugging/remote-debug-overview.jpg)</div>

Debugging occurs over USB and as long as your mobile device is connected to your development machine, you can view and change HTML, scripts and styles until you get a bug-free page that behaves perfectly on all devices.

To begin remote debugging, you will need:

*   An Android phone or tablet with [Chrome for Android](https://play.google.com/store/apps/details?id=com.android.chrome&amp;hl=en) 32 or later installed from Google Play.
*   A USB cable to plug in your device. (Windows users will also need to install an [appropriate USB device driver](http://developer.android.com/tools/extras/oem-usb.html).)
*   Chrome 32 or later installed on your development machine.

When debugging a web application served from your development machine, you can also use
[reverse port forwarding](#reverse-port-forwarding) to allow the mobile device to access a site from the development machine over USB.

## 1. Setting up your device

In order to debug over USB, you need to setup your Android device for
development. 

**To enable USB debugging:**

*   On Android 4.2 and newer, **Developer options** is hidden by default. To make it
available, go to **Settings > About phone** and tap **Build number** seven times. Yup, just tap it 7 times, even if it seems crazy. Then, return to the previous screen to find **Developer options**.
*   On Android 4.0 and 4.1, it's in **Settings > Developer options**.

<video loop muted controls src="remote-debugging/7tap-optimized.mp4" width=332 height=310 onended="this.play()"></video> ![USB debugging settings in Developer options](remote-debugging/usb_debugging_on.jpg)

If you are developing on Windows, you need to install the appropriate USB driver for your device. See [OEM USB Drivers](http://developer.android.com/tools/extras/oem-usb.html) on the Android Developers site.

<!--

For more information see [Setting up a Device for Development](http://developer.android.com/tools/device.html#setting-up) on the Android Developers site.
 -->

## 2.1 Connecting directly over USB

DevTools now supports **native USB debugging** of connected devices. You no longer need to configure [ADB](remote-debugging-legacy) or the ADB plugin to see all instances of Chrome and the Chrome-powered [WebView](/chrome-developer-tools/docs/remote-debugging#debugging-webviews) on devices connected to your system. This functionality works on all OS's: Windows, Mac, Linux and Chrome OS.

**Windows user?** You'll need to install  [device drivers](http://developer.android.com/tools/extras/oem-usb.html) to enable
communication between Windows and your device.

Just visit `about:inspect` and verify **Discover USB Devices** is checked.<p>

<div class="screenshot">![about:inspect Discover USB Devices](remote-debugging/discover-usb-devices.png)</div>

<p>This direct USB connection between Chrome and the device may interrupt an `adb` connection that you may be trying to establish. If you need to use the `adb` binary for other reasons, uncheck the "Discover USB Devices" checkbox, unplug the device, and plug it back in, before establishing your connection via `adb devices`.

<aside class="note">**Note: ** If you encounter problems with the  above technique, or are using an older version of Chrome, you can try the legacy workflow for connectivity which uses the `adb` binary from the Android SDK:
[Remote Debugging on Android (Legacy Workflow)](remote-debugging-legacy).</aside>

## 3. Connect your device

1.  Connect your mobile device to the development machine using a USB cable.2.  When connecting your device to your development machine, you may see
an alert on the device requesting permission for USB debugging from this computer.  To avoid seeing this alert each time you debug, check **Always allow from this computer** and click **OK**

<div class="screenshot">![USB debugging permission alert](remote-debugging/usb-debugging-dialog.png)</div>

Now, to see all connected devices, go to the **Chrome menu > Tools > Inspect Devices**:

<div class="screenshot">![](remote-debugging/menu.png)</div>

You can also navigate directly to  `about:inspect`.

<div class="screenshot">![](remote-debugging/about-inspect-stuff.png)</div>

This page displays each connected device and its tabs. You can have multiple devices simultaneously connected as well as multiple versions of Chrome open on each device. As indicated lower on the page, debuggable [WebViews appear here](#debugging-webviews), as well. 

Find the tab you're interested in and click the **inspect** link to open DevTools on it. You may also reload the page, bring it to the front, or close it. Lastly, you can open new links on the device through a text input field.

### Troubleshooting

*   On your device, verify you have **Developer Options** available, and **USB debugging** turned on. If it's working and connected, it'll set a notification on your device.
*   Verify you're using Chrome for Android 32 or later.
*   If USB debugging is on, but `about:inspect` doesn't show your device check that **Discover USB devices** is checked. If so, unplug the device and try revoking all USB authorizations in Developer Options to retry.
*   You can try Chrome Canary on desktop as new improvements land daily.

## 4. Debug Your Application

<div class="screenshot">![Inspecting a remote page using the Chrome Developer Tools](remote-debugging/elements-panel.png)</div>

For example, inspect an element in the page you have selected and the element highlights in Chrome mobile on your device in real time. In fact, you can turn on inspect mode by clicking the icon, and then tap on your device screen.

<div class="screenshot">![Element inspection on a remote device](remote-debugging/image_9.png)</div>

Similarly, editing scripts or executing commands from the DevTools console affects the page being inspected on your device. You can also also use all of the other panels, such as [Timeline](timeline) and [Profiles](cpu-profiling).

### Notes

*   Because we're connected over USB, you can keep the device on a real cellular
network, and view the network waterfall in the Network panel under actual
network conditions.
*   Chrome will prevent your screen from going to sleep while remote debugging. Be aware that whilst useful, this makes your device less secure.
*   The hardware on mobile devices often runs your content much slower, so use the [Timeline](timeline) to analyze how to optimize rendering and CPU for the best effect.
*   If you're running a web server on your development machine, and network restrictions prevent your mobile device from accessing the server, see [reverse port forwarding](#reverse-port-forwarding).
*   You may notice that the version of the DevTools you have access to during remote debugging differs to the version you have running on your development machine. This is because the tools are synchronized with the Chrome on Android version in use.

## Screencasting your device's screen

Screencasting lets you bring the experience of your device onto your machine. This allows you to keep your attention on one screen instead of switching back and forth between the device and the DevTools. 

<iframe width="640" height="360" src="//www.youtube.com/embed/Q7rEFEMpwe4" frameborder="0" allowfullscreen></iframe>

Clicking on the screencast icon ![Screencast](remote-debugging/screencast0.png) in the toolbar opens up a panel on your computer displaying your device's screen. As you navigate, click, scroll, the screencast display will provide a live view of what's on your device.

While you are screencasting your device, can you control the mobile browsers's back and forward buttons, reload, and change the URL directly.

![Screencast toolbar](remote-debugging/screencast2.png)

### Interacting with the screencast

You can interact with the screencast of your device in a number of ways.

*   **Type** on your machine's keyboard and these keystrokes are sent to the device
*   **Click** to tap. Clicks will be sent to the device as proper touch events.
*   **Scroll** by mousewheel, trackpad, or by flinging the content with your pointer.
*   **Inspect Element** by selecting the magnifying glass or by pressing <span class="kbd">Cmd</span> + <span class="kbd">Shift</span> + <span class="kbd">C</span>
*   **Zoom** with a simulated pinch gesture with <span class="kbd">Cmd</span> + <span class="kbd">Click with two fingers</span> + <span class="kbd">Drag</span>
*   **Resize** the pane that screencast is in to better size its contents.*   **Transparent** portions of the screencast are covered by things like the omnibox and keyboard. Only page content is being screencasted.

<div class="screenshot">![Screencasting device](remote-debugging/screencast1.png)</div>

**Note:** The screencast feature repeatedly snaps screenshots on the device to give you the live view, but this does have a performance overhead. Disable screencast if you're testing framerate-sensitive situations.

## Debugging Android WebViews

Starting Android 4.4 (KitKat), you can use the DevTools to debug the contents of
Android WebViews inside native Android applications. Debugging WebViews requires:

*   An Android device or emulator running Android 4.4 or later, with USB debugging enabled as
    described in
    [ 2. Enable USB debugging on your device ](#enable-usb-debugging).*   Chrome 30 or later. Enhanced WebView debugging UI is available in Chrome 31 or later.
*   An Android application with a WebView configured for debugging.

### Configure WebViews for debugging

The **Enable USB web debugging** setting in Chrome doesn't affect WebViews. To
debug the contents of your WebView, you need to enable it programmatically from within your application by calling
[
setWebContentsDebuggingEnabled](http://developer.android.com/reference/android/webkit/WebView.html#setWebContentsDebuggingEnabled(boolean)), a static method on the `WebView` class.

<pre>
if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    WebView.setWebContentsDebuggingEnabled(true);
}
</pre>

This setting applies to all of the application's WebViews. Note that web debugging is **not** affected
by the state of the `debuggable` flag in the application's manifest. If you want to enable web debugging only
when `debuggable` is `true`, test the flag at runtime.

<pre>
if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
    if ( 0 != ( getApplcationInfo().flags &= ApplicationInfo.FLAG_DEBUGGABLE ) ) {
        WebView.setWebContentsDebuggingEnabled(true);
    }
}
</pre>

### Open a WebView in DevTools

To debug a WebView in DevTools:

1.  Connect your mobile device to the development machine using a USB cable.
<p>When connecting your device to your development machine, you may see
an alert on the device requesting permission for USB debugging from this computer.

To avoid seeing this alert each time you debug, check **Always allow from this computer** and click **OK**.

3.  In Chrome on your development machine, open **about:inspect**.
4.  You should see the name of your application and a list of debuggable WebViews. Click the `inspect` link
next to one of the tabs to inspect the WebView's contents in DevTools.

<aside class="note">**Note:** In Chrome 31 and later, the **about:inspect** page provides a graphic representing the WebView's
size and position relative to the device's screen. Prior to Chrome 31, the **about:inspect** page only supplies the WebView's title.
Setting a title on all of your WebViews simpifies the process of picking the correct WebView.</aside>

<div class="screenshot">![](remote-debugging/about-inspect-webview.gif)</div>

## Port Forwarding

Commonly you have a web server running on your local development machine, and you want to
connect to that site from your device. If the mobile device and the development machine are
on the same network, this is straightforward. But this may be difficult in some cases, like
on a restricted corporate network.

Chrome for Android supports port fowarding making this workflow very simple to do.
It works by creating a listening TCP port on your mobile device that maps to a particular TCP
port on your development machine. The traffic through the forwarded port travels over USB, so
it doesn't depend on the mobile device's network configuration.

This procedure assumes that you already have remote debugging configured and working.

### 1. Connect your mobile device

1.  Connect your device to your development machine over USB.
2.  Stop all instances of Chrome currently running on the mobile device.
3.  Open Chrome for Android.

### 2. Enable port forwarding

Perform the following steps on Chrome on your development machine:

1.  Open **about:inspect**. You should see your mobile device and a list of its open tabs.
<li>Click **Port Forwarding** button at the top.
2.  In the Device port field, enter the port number the Android should device listen on (defaults to 8080).
<li>In the Host field, add the IP and port number where your web application is running on localhost.

### 3. Profit

On **about:inspect** you should now see a green circle indicating your port forwarding is succssful.
Now, enter in your local URL into the **Open tab** field and hit **Go** to open it on your device's browser.

You should see the content being served by your development machine.

<div class="screenshot">![](remote-debugging/portforward.png)</div>