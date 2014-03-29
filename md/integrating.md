Integrating with DevTools
==

The Chrome DevTools are built to be extensible. So if the DevTools are missing a 
feature you need, you may be able to find an existing extension, or write one 
yourself. Or you can integrate DevTools capabilities into your application.

There are two basic ways to build a custom solution using the DevTools:

*   **DevTools Extension**. A [Chrome  extension](http://developer.chrome.com/extensions/) that plugs into the  DevTools to add functionality and extend its UI.
*   **Debugging Protocol Client**. A third-party application that uses the Chrome [  remote debugging protocol](/chrome-developer-tools/docs/debugger-protocol) to  plug into the low-level debugging support in Chrome.

The following sections discuss both approaches.

#### Contents

<nav class="inline-toc">

1.  [DevTools Chrome extensions](#devtools_chrome_extensions)
2.  [Debugging protocol clients](#debugging_protocol_clients)</nav>

## DevTools Chrome extensions

The DevTools UI is a web application embedded inside Chrome. 
DevTools extensions use the [Chrome extensions 
system](http://developer.chrome.com/extensions/) to add features to the 
DevTools. A DevTools extension can add new panels to the DevTools, add new 
panes to the Elements and Sources panel sidebar, examine the resources and 
network events, as well as evaluate JavaScript expressions in the browser tab 
that's being inspected.

If you want to develop a DevTools extension:

*   If you haven't developed a Chrome extension before, see [Overview of Chrome  Extensions](http://developer.chrome.com/extensions/overview.html).
*   See [Extending DevTools](http://developer.chrome.com/extensions/devtools.html)  for the specifics of creating a Chrome DevTools extension.

For a list of sample DevTools extensions, see [Sample 
DevTools Extensions](sample-extensions). These samples include many open source extensions that 
can be used for reference.

## Debugging protocol clients

Third-party applications, such as IDEs, editors, continuous integration 
harnesses, and test frameworks can integrate with the Chrome debugger in order 
to debug code, live-preview code and CSS changes, and control the browser. 
Clients use the [Chrome debugging 
protocol](/chrome-developer-tools/docs/debugger-protocol) to interact with an 
instance of Chrome, which can be running on the same system or remotely. 

<aside class="note">**Note:**<span> Currently, the Chrome debugging protocol supports only _one_ client per 
page. So you can use the DevTools to inspect a page, or use a third-party 
client, but not both at the same time.</span></aside>

There are two ways to integrate with the debugging protocol:

*   Applications that run in Chrome (such as web-based IDEs) can create a Chrome  extension using the debugger module,  [chrome.debugger](http://developer.chrome.com/extensions/debugger.html). This  module lets the extension interact with the debugger directly, bypassing the  DevTools UI. See [Using the debugger extension  API](/chrome-developer-tools/docs/debugger-protocol#extension) for more  information.
*   Other applications can use the  [wire protocol](/chrome-developer-tools/docs/debugger-protocol#remote) to  integrate directly with the debugger. This protocol involves exchanging JSON  messages over a WebSocket connection.

For some example integrations, see [Sample Debugging 
Protocol Clients](debugging-clients).