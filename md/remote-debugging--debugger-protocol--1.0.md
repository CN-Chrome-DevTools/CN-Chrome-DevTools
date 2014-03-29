Remote debugging protocol
==
Under the hood, Chrome Developer Tools is a web application written in HTML,
JavaScript and CSS. It has a special binding available at JavaScript runtime
that allows interacting with chrome pages and instrumenting them. Interaction
protocol consists of commands that are sent to the page and events that
the page is generating. Although Chrome Developer Tools is the only client of
this protocol, there are ways for third parties to bypass it and start
instrumenting browser pages explicitly. We will describe the ways it could be
done below.

**Note:** If you are interested in inspecting remote pages on Chrome for Android, please see the [remote debugging documentation](/chrome-developer-tools/docs/remote-debugging). For users wishing to implement custom inspection code using our debugger protocol instead, please use the instructions below.

#### Contents

1.  [Protocol](#protocol)
2.  [Debugging over the wire](#remote)
3.  [Using debugger extension API](#extension)

## Protocol

Interaction protocol consists of JSON commands that are sent to the page and
events that the page is generating. We define this protocol in Blink
("upstream") so that any Blink-based browser supported it.

*   As of Google Chrome M31, we commit to supporting the version
[1.1](/chrome-developer-tools/docs/protocol/1.1/index) of the protocol. All
subsequent 1.* versions of the protocol are going to be backwards compatible with 1.1.
*   As of Google Chrome M18, we commit to supporting the version
[1.0](/chrome-developer-tools/docs/protocol/1.0/index) of the protocol. All
subsequent 1.* versions of the protocol are going to be backwards compatible with 1.0.
*   As of Google Chrome M16, we commit to supporting the version
[0.1](/chrome-developer-tools/docs/protocol/0.1/index) of the protocol. All
subsequent 0.* versions of the protocol are going to be backwards compatible with 0.1.

Protocol backwards compatibility is defined as follows:

*   No commands or events are removed from the protocol
*   No required parameters are added to the commands
*   No required parameters are removed from command responses or events

There also is a [tip-of-tree](/chrome-developer-tools/docs/protocol/tot/index) version of the
protocol that reflects present state of the protocol in Blink repository. You can
use it with [Google Canary](http://tools.google.com/dlpage/chromesxs)
builds at your own risk. Note that unless tip of the tree revision is promoted to the
draft, there is no backwards compatibility support guaranteed for the capabilities it
introduces.

## Debugging over the wire

Today Developer Tools front-end can attach to a remotely running Chrome
instance for debugging.

For this scenario to work, you should start your
_host_ Chrome instance with the remote-debugging-port
command line switch:

<div class="summary">
chrome.exe --remote-debugging-port=9222
</div>

Then you can start a _client_ Chrome instance, using a separate user profile:

<div class="summary">
chrome.exe --user-data-dir=&lt;some directory&gt;
</div>

And now you can navigate to the given port from your _client_ and attach to
any of the discovered tabs for debugging.

<div class="summary">
http://localhost:9222
</div>

You will find Developer Tools interface identical to the embedded one and
here is why:

*   When you navigate your _client_ browser to the remote's Chrome port,
Developer Tools front-end is being served from the _host_ Chrome
as a Web Application from the Web Server.
*   It fetches HTML, JavaScript and CSS files over HTTP
*   Once loaded, Developer Tools establishes a Web Socket connection to its
host and starts interchanging JSON messages with it.

In this scenario, you can substitute Developer Tools front-end with your
own implementation. Instead of navigating to the HTML page at
http://localhost:9222, your application can discover available
pages by issuing the following HTTP request to the browser:

<div class="summary">
http://localhost:9222/json
</div>

and getting a JSON object with information about inspectable pages along
with the WebSocket addresses that you could use in order to start
instrumenting them.

Note that we are currently working on exposing an HTTP-based
protocol that does not require client WebSocket implementation.

Remote debugging is especially useful when debugging remote
instances of the browser or attaching to the embedded devices. Blink port
owners are responsible for exposing debugging connections to the external
users.

## Using debugger extension API

To allow third parties to interact with the protocol, we introduced
[
chrome.debugger](https://developer.chrome.com/extensions/debugger.html) extension API that exposes this JSON message
transport interface. As a result, you can not only attach to the remotely
running Chrome instance, but also instrument it from its own extension.

Chrome Debugger Extension API provides a higher level API where command
domain, name and body are provided explicitly in the `sendCommand`
call. This API hides request ids and handles binding of the request with its
response, hence allowing `sendCommand` to report result in the
callback function call. One can also use this API in combination with the other
Extension APIs.

If you are developing a Web-based IDE, you should implement an extension that
exposes debugging capabilities to your page and your IDE will be able to open
pages with the target application, set breakpoints there, evaluate expressions
in console, live edit JavaScript and CSS, display live DOM, network interaction
and any other aspect that Developer Tools is instrumenting today.

Note: opening embedded Developer Tools will terminate the
remote connection / detach the extension and will replace active debugger with
itself. We are working on allowing several clients to instrument the page
simultaneously.