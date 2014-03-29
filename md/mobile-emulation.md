Mobile emulation
==

#### **Contents**

*   [Enabling the Emulation Panel](#enable-emulation-panel)
*   [Emulating Touch Events](#emulate-touch-events)
*   [Emulating Devices](#emulate-devices)
*   [Useragent Spoofing](#useragent-spoofing)
*   [Network Bandwidth Throttling](#network-throttling)
*   [Geolocation Overrides](#device-geolocation-overrides)
*   [Device Orientation Overrides](#device-orientation-overrides)
*   [CSS Media Type Emulation](#css-media-type-emulation)
*   [Frequently Asked Questions](#faq)

The mobile web has evolved to enable increasingly sophisticated applications,
which we often wish to debug during development on the desktop. The DevTools
include support for a number of features that can help with this which we will
walk through in this section.

**Note**: Some of our documentation may be ahead of the stable version of Chrome. If you are unable to locate a feature listed, we recommend trying [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html) which contains the latest version of the DevTools.

## Enabling the Emulation Panel

[Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html "Chrome Canary") has received a number of improved mobile emulation tools which can be accessed by enabling the Emulation panel through the Settings panel. To enable:

1.  Open the Settings panel within the DevTools.
2.  Enable "Show 'Emulation' view in console drawer."

<div class="screenshot">![Enabling emulation](mobile-emulation/enable-emulation.png)</div>

Bring up the DevTools console drawer by hitting the <span class="kbd">Esc</span> key and then select the Emulation panel.

<div class="screenshot">![Emulation panel](mobile-emulation/emulation-panel.png)</div>

**Note**: Emulation tools within DevTools were previously contained within an **Overrides** pane inside the Settings panel. The Emulation panel is the new Overrides pane.

## Emulating Devices

It's often easier to start prototyping on the desktop and then tackle the
mobile-specific parts on the devices you intend to support. Device emulation can
make this process more straightforward.

<iframe width="640" height="360" src="//www.youtube.com/embed/z7sTRdSpA04" frameborder="0" allowfullscreen=""></iframe>

DevTools contains built in presets for a number of devices, this allows you to emulate and debug mobile viewport issues like CSS media query breakpoints. Selecting a device preset automatically enables a number of settings:

*   **User agent** - a string available at `navigator.userAgent` and also sent as a request header for page resources. See [Useragent Spoofing](#useragent-spoofing).
*   **Screen resolution** - matches the actual dimensions (width and height) of the selected device. For example, selecting the Nexus S will emulate a resolution of 480x800.
*   **Device Pixel Ratio** - matches the DPR of the selected device. Allows emulation of a retina device from a non-retina machine. This also means media queries such as `@media only screen and (min-device-pixel-ratio: 2)` can be tested.
*   **Emulate viewport** - zooms the page out to the physical default viewport of that device. In the case of the Nexus 4 this is 768x1280.*   **Text autosizing** - emulate font boosting which occurs on mobile devices.

*   **Android font metrics** - Android artificially increases the font metrics used by text autosizing based on the system settings and screen size. Enabled by default only when emulating an Android device.*   **Touch screen** (part of the Sensors pane) - uses touch events, see [Emulating Touch Events](#emulate-touch-events).

<div class="screenshot">![Viewport emulation](mobile-emulation/viewport-emulation.gif)</div>

Emulate a device via a preset using the following steps:

1.  Open the Emulation panel within the DevTools
2.  With the Device pane selected, select "Google Nexus S".
3.  Click Emulate.

<div class="screenshot">![Viewport Overrides](mobile-emulation/image_3.png)</div>

You are now in device emulation mode. The Screen, User Agent, and Sensors panes reflect the device settings which are now enabled. 

<div class="screenshot">![Viewport Overrides](mobile-emulation/device_metrics.png)</div>

#### Notes

<ul>
<li>The **Swap dimensions** button in between the _Resolution_ values (![](mobile-emulation/image_6.png)) will swap the width and height.</li>
<li>**Shrink to fit** ensures the emulated device screen is completely visible within your browser window. This setting does not emulate the device differently.</li>
<li>Device media queries (e.g `@media only screen and (min-device-width:768px)`) will be enabled according to the values defined in the Resolution settings.</li>
<li>You may want to undock DevTools or dock it to the right while working with emulated viewport settings.
<li>Device settings can be configured independently of a device preset.</li>
<li>If you would like to contribute patches to support new device presets, please see our [contribution docs](contributing).</li>

## Emulating Touch Events

Touch is an input method that's difficult to test on the desktop, since most
desktops don't have touch input. Having to test on mobile can lengthen your
development cycle, since every change you make needs to be pushed out to a
server and then loaded on the device.

A solution to this problem is to simulate touch events on your development
machine. For single-touches, the Chrome DevTools supports single[
](http://www.w3.org/TR/touch-events/)[touch
event](http://www.w3.org/TR/touch-events/) emulation to make it easier to debug
mobile applications on the desktop.

**To enable support for touch event emulation:**

1.  Open the Emulation panel in the DevTools.
2.  Enable "Emulate touch screen" in the Sensors pane.

<div class="screenshot">![Emulating touch events in the Overrides panel](mobile-emulation/image_0.png)</img></div>

Your mouse actions will now also trigger the relevant touch events: `touchstart`, `touchmove` and `touchend`.

#### Notes

*   Feature detects such as `[Modernizr.touch](http://modernizr.com)` will now
succeed on page refresh.
*   This feature, like many other overrides, will only work while the DevTools are open.
*   The cursor will change to a small circle to emulate a fingertip size.
*   Use <span class="kbd">Shift</span> + Drag to emulate a "pinch".
*   Enabling "Emulate touch screen" does not disable mouse events entirely, as they are fired on touch devices. Try this [touch event listener test page](http://patrickhlauke.github.io/touch/tests/event-listener.html), touch is another option we can debug with.
*   On click, the order of events fired is currently: `touchstart > mousedown > touchmove > touchend > mouseup > click`. On touch devices, this order is slightly different. The tools will shortly be [updated with the right order.

<div class="screenshot">![Emulating pinch to zoom](mobile-emulation/scrolling-emulation.gif)</img></div>

**Debugging touch events**

1.  Open up the[ ](http://paulirish.com/demo/multi)[Canvas Fingerpaint
Demo](http://paulirish.com/demo/multi)
2.  Navigate to the Sources panel
3.  Expand the "Event Listener Breakpoints" sub-panel
4.  Check the "touchstart" and "touchmove" events under "Touch"
5.  Move your cursor over the paint area
6.  The debugger should successfully pause on the <span class=
  "source-code">draw()</span> method

<div class="screenshot">![Debugging touch events](mobile-emulation/image_2.png)</div>

You may also monitor touch events as they fire on an element in the console. Use [`monitorEvents` from the command line API](https://developers.google.com/chrome-developer-tools/docs/commandline-api#monitoreventsobject_events):

    monitorEvents(document.body, 'touch')

**Multi-touch**

Multi-touch events can be simulated if you have a device with touch input, such
as a modern Apple MacBook. For further assistance with multi-touch event
simulation, see the "Developer tools" chapter of the [**Multi-touch web
development** guide on HTML5 Rocks](http://www.html5rocks.com/en/mobile/touch/).

## Useragent Spoofing

**Emulating The User Agent**

1.  Navigate to the [Google](http://www.google.com) homepage.
2.  In the DevTools, open up the User Agent pane within the Emulation panel.
3.  Check "Spoof user agent" and select "Android 2.3 - Nexus S".
4.  Refresh the page.

An updated user-agent field is now sent as part of the request headers for page resources. Some websites may decide to serve optimized versions of the page depending on the user-agent, this is one case where spoofing a user-agent may be useful.

<table>
<thead>
<tr><th>
Before:
</th><th>
After:
</th></tr>
</thead>
<tbody> <tr>
<td>![Before user agent and device metric emulation have been applied](mobile-emulation/image_4.png)</td>
<td>![After user agent and device metric emulation are applied](mobile-emulation/image_5.png)</td>
</tr></tbody>
</table>

## Network Bandwidth Throttling

The DevTools does not currently support network throttling, however it's important to understand and test the impact of slower connections on your site.

 <p> On Mac, we recommend using the [Network Link Conditioner](http://nshipster.com/network-link-conditioner/) that is available via [Xcode](https://developer.apple.com/xcode/). It has presets for network conditions like EDGE, 3G, DSL, WiFi, High Latency DNS, Very Bad Network, and 100% Loss. Changes made within Network Link Conditioner will affect all system network traffic, including Chrome or any running emulators and simulators.

![Network Link Conditioner preference panel](mobile-emulation/network-link-conditioner-system-preference.png)

On Windows, we recommend using [Clumsy](http://jagt.github.io/clumsy/index.html), which can add extra lag, drop packets, throttle and manipulate other network conditions.

![Clumsy Options](http://jagt.github.io/clumsy/clumsy-demo.gif)

On Linux, there are many options for traffic shaping; [dummynet](http://info.iet.unipi.it/~luigi/dummynet/) is the recommended option. 

## Geolocation Overrides

When working with HTML5 geolocation support in an application, it can be useful
to debug the output received when using different values for longitude and
latitude. The DevTools support both overriding position values for _navigator.geolocation
_and simulating geolocation not being available via the Sensors pane.

**Overriding geolocation positions**

1.  Navigate to the [Geolocation](http://html5demos.com/geo) demo and allow the page access to your position.
2.  In the DevTools, open up the Sensors pane within the Emulation panel.
3.  Check "Emulate geolocation coordinates" and enter 41.4949819 in the Lat field and -0.1461206 in the Lon field.

<div class="screenshot">![](mobile-emulation/image_11.png)</div>

1.  Refresh the page. The demo will now use your overridden position for geolocation.

<div class="screenshot">![](mobile-emulation/image_12.png)</div>

1.  Check the "Emulate position unavailable" option.
2.  Refresh the page. The demo will now inform you that finding your location has failed.

<div class="screenshot">![](mobile-emulation/image_13.png)</div>

## Device Orientation Overrides

Many new mobile devices are now shipping with accelerometers, gyroscopes,
compasses and other hardware designed to determine capture motion and
orientation data. Many web browsers have access to that new hardware,
such as via the [HTML5
DeviceOrientation](http://dev.w3.org/geo/api/spec-source-orientation) events.
These events provide developers with information about the orientation, motion
and acceleration of the device.

If your application is taking advantage of device orientation events, it can
also be useful to override the values received by these events during debugging
to avoid the need to test them on a physical mobile device.

**Overriding orientation values**

1.  Navigate to the[
](http://www.html5rocks.com/en/tutorials/device/orientation/deviceorientationsample.html)[Device
Orientation](http://www.html5rocks.com/en/tutorials/device/orientation/deviceorientationsample.html) demo and notice the standard HTML5 logo along with the current orientation
values listed above it.
2.  Open the Emulation panel in the DevTools and click the Sensors pane.
3.  Check "Accelerometer".
4.  You will see three fields:

        *   **α**: how much the device has been rotated around the
z-axis.
  <li>**β**: how much the device is tilted left-to-right.
  <li>**γ**: how much it's
tilted front-to-back.
<li>Change the values to the following:

    1.  α - 0
    2.  β - 60
    3.  γ - 60

<div class="screenshot">![Enabling device orientation overrides](mobile-emulation/image_14.png)</div>

We have altered the left/right tilt and front/back tilt, in this case resulting
in our application being emulated as rotating in a clockwise direction.

<div class="screenshot">![Device orientation allows us to emulate the directions a device may be turned](mobile-emulation/image_15.png)</div>

## CSS Media Type Emulation

CSS media types allow you to apply different styles to a page depending on the
medium it is being used through (e.g print, screen, tv, braille and so on).

**Emulating media types**

1.  Navigate to [HTML5 Rocks](http://www.html5rocks.com/en/tutorials/developertools/novdigest/).
2.  Open the Emulation panel in the DevTools.
3.  Enable "CSS media" on the "Screen" pane and select the "print" media type option from the drop-down
box.
4.  The page will adjust to using a stylesheet for the chosen CSS media type if
one is available.

<table>
<thead>
<tr><th>
Before:
</th><th>
After:
</th></tr>
</thead>
<tbody> <tr>
<td>![Before enabling CSS media type emulation](mobile-emulation/emulatecss_before.jpg)</td>
<td>![After enabling CSS media type emulation](mobile-emulation/emulatecss_after.jpg)</td>
</tr></tbody>
</table>

## Frequently Asked Questions

**Q: Do the DevTools supports remote debugging?**

A: Yes. Please see our [remote debugging](/chrome/mobile/docs/debugging)
documentation for more information.

**Q: Can the DevTools emulate lower GPU memory limits or slower CPUs, as found on mobile devices?**

A: Currently there is no means within DevTools or Chrome to emulate these characteristics. 