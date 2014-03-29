Command Line API Reference
==

The Command Line API is a collection of functions for performing common tasks with the Chrome Developer Tools. These include convenience functions for selecting and inspecting elements in the DOM, stopping and starting the profiler, and monitoring DOM events. This API complements the [Console API](console-api), the Command Line API is only available from within the console itself.

<nav class="inline-toc">

1.  [$_](#_)
2.  [$0 - $4](#0_-_4)
3.  [$(selector)](#selector)
4.  [$$(selector)](#selector_1)
5.  [$x(path)](#xpath)
6.  [clear()](#clear)
7.  [copy(object)](#copyobject)
8.  [dir(object)](#dirobject)
9.  [dirxml(object)](#dirxmlobject)
10.  [inspect(object)](#inspectobject)
11.  [getEventListeners(object)](#geteventlistenersobject)
12.  [keys(object)](#keysobject)
13.  [monitorEvents(object[, events])](#monitoreventsobject_events)
14.  [profile([name])](#profilename)
15.  [profileEnd([name])](#profileendname)
16.  [unmonitorEvents(object[, events])](#unmonitoreventsobject_events)
17.  [values(object)](#valuesobject)</nav>

## $_

Returns the value of the most recently evaluated expression. In the following example, a simple expression is evaluated. The `$_` property is then evaluated, which contains the same value:

![](commandline-api-files/last_expression.png)

In the next example, the evaluated expression is a call to the [`$$()`](#selector_1) method, which returns an array of elements that match the CSS selector. It then evaluates `$_.length` to get the length of the array (17), which becomes the most recently evaluated expression.

![](commandline-api-files/last_expression_2.png)

## $0 - $4

Dev Tools remembers the last five DOM elements (or JavaScript heap objects) that you've selected in the  tab (or Profiles panel). It makes those objects available as **`$0`**, **`$1`**, **`$2`**, **`$3`**, and **`$4`**. **`$0`** returns the most recently selected element or JavaScript object, **`$1`** returns the second most recently selected one, and so on.

In the following example, an element with the ID `gc-sidebar` is selected in the Elements tab. In the Console window `$0` has been evaluated, which displays the same element.

![Example of $0](commandline-api-files/$0.png)

The image below shows the `gc-content` element selected in the same page. The **`$0`** now refers to newly selected element, while **`$1`** now returns the previously selected one (`gc-sidebar`).

![image alt text](commandline-api-files/$1.png)

## $(selector)

Returns reference to the first DOM element with the specified CSS selector.This function is an alias for [`document.querySelector()`](http://docs.webplatform.org/wiki/css/selectors_api/querySelector) function.

The following example saves a reference to the first `&lt;img&gt;` element in the document and calls displays its `src` property:

    $('img').src;
    `</pre>

![image alt text](commandline-api-files/$img_src.png)

## $$(selector)

Returns an array of elements that match the given CSS selector. This command is equivalent to calling [`document.querySelectorAll()`](http://docs.webplatform.org/wiki/css/selectors_api/querySelectorAll).

The following example uses `$$()` to create an array of all `&lt;img&gt;` elements in the current document and displays the value of each element's `src` property.

<pre class="prettyprint notranslate" translate="no">`var images = $$('img');
for (each in images) {
    images[each].src;
}
`</pre>

![Example of using $$() to select all images in the document and display their sources.](commandline-api-files/$$img_src.png)

<aside class="note">**Note:**<span> Press Shift+Enter in the console to start a new line without executing the script.</span></aside>

## $x(path)

Returns an array of DOM elements that match the given XPath expression. For example, the following returns all the `&lt;p&gt;` elements that contain `&lt;a&gt;` elements:

<pre class="prettyprint notranslate" translate="no">`$x("//p[a]")
`</pre>

![Example of using an Xpath selector](commandline-api-files/$xpath.png)

## clear()

Clears the console of its history.

<pre class="prettyprint notranslate" translate="no">`clear();
`</pre>

Also see [Clearing the console](console#clearing_the_console_history).

## copy(object)

Copies a string representation of the specified object to the clipboard.

<pre class="prettyprint notranslate" translate="no">`copy($0);
`</pre>

## dir(object)

Displays an object-style listing of all the properties of the specified object. This method is an alias for the Console API's [`console.dir()`](console-api#consoledirobject) method.

The following example shows the difference between evaluating `document.body` directly in the command line, and using `dir()` to display the same element.

<pre class="prettyprint notranslate" translate="no">`document.body;
dir(document.body);
`</pre>

![Logging document.body with and without dir() function](commandline-api-files/document.body.png)

For more information, see the [`console.dir()`](console-api#consoledirobject) entry in the Console API.

## dirxml(object)

Prints an XML representation of the specified object, as seen in the Elements tab. This method is equivalent to the [`console.dirxml()`](console-api#consoledirxmlobject) method.

## inspect(object)

Opens and selects the specified element or object in the appropriate panel: either the Elements panel for DOM elements and the Profiles panel for JavaScript heap objects.

The following example opens the first child element of `document.body` in the Elements panel:

<pre class="prettyprint notranslate" translate="no">`inspect(document.body.firstChild);
`</pre>

![Inspecting an element with inspect()](commandline-api-files/inspect.png)

## getEventListeners(object)

Returns the event listeners registered on the specified object. The return value is an object that contains an array for each registered event type ("click" or "keydown", for example). The members of each array are objects that describe the listener registered for each type. For example, the following lists all the event listeners registered on the `document` object.

<pre class="prettyprint notranslate" translate="no">`getEventListeners(document);
`</pre>

![Output of using getEventListeners()](commandline-api-files/geteventlisteners_short.png)

If more than one listener is registered on the specified object, then the array contains a member for each listener. For instance, in the following example there are two event listeners registered on the `#scrollingList` element for the "mousedown" event:

![](commandline-api-files/geteventlisteners_multiple.png)

You can further expand each of these objects to explore their properties:

![Expanded view of listener object](commandline-api-files/geteventlisteners_expanded.png)

## keys(object)

Returns an array containing the names of the properties belonging to the specified object. To get the associated values of the same properties, use [values()](#valuesobject).

For example, suppose your application defined the following object:

<pre class="prettyprint notranslate" translate="no">`var player1 = {
    "name": "Ted",
    "level": 42
}
`</pre>

Assuming `player1` was defined in the global namespace (for simplicity), typing `keys(player1)` and `values(player1)` in the Console would result in the following:

![Example of keys() and values() methods](commandline-api-files/keys-values2.png)

## monitorEvents(object[, events])

When one of the specified events occurs on the specified object, the Event object is logged to the console. You can specify a single event to monitor, an array of events, or one of the generic events "types" that are mapped to a predefined collection of events. See examples below.

The following monitors all `resize` events on the `window` object.

<pre class="prettyprint notranslate" translate="no">`monitorEvents(window, "resize");
`</pre>

![Monitoring window resize events](commandline-api-files/monitor-resize.png)

The following defines an array to monitor both "resize" and "scroll" events on the `window` object:

<pre class="prettyprint notranslate" translate="no">`monitorEvents(window, ["resize", "scroll"])
`</pre>

You can also specify one of the available event "types", strings that map to predefined sets of events. The table below lists the available event types and their associated event mappings:

<table>
<thead>
<tr>
<th>Event type</th>
<th>Corresponding mapped events</th>
</tr>
</thead>
<tbody>
<tr>
<td>**mouse**</td>
<td>"`mousedown`", "`mouseup`", "`click`", "`dblclick`", "`mousemove`", "`mouseover`", "`mouseout`", "`mousewheel`"</td>
</tr>
<tr>
<td>**key**</td>
<td>"`keydown`", "`keyup`", "`keypress`", "`textInput`"</td>
</tr>
<tr>
<td>**touch**</td>
<td>"`touchstart`", "`touchmove`", "`touchend`", "`touchcancel`"</td>
</tr>
<tr>
<td>**control**</td>
<td>"`resize`", "`scroll`", "`zoom`", "`focus`", "`blur`", "`select`", "`change`", "`submit`", "`reset`"</td>
</tr>
</tbody>
</table>

For example, the following uses the "key" event type all corresponding key events on an input text field ("`#msg`").

<pre class="prettyprint notranslate" translate="no">`monitorEvents($("#msg"), "key");
`</pre>

Below is sample output after typing two characters in the text field:

![Monitoring key events](commandline-api-files/monitor-key-events.png)

![](commandline-api-files/monitor-key-events.png)

## profile([name])

Starts a JavaScript CPU profiling session with an optional name. To complete the profile call [`profileEnd()`](#profileendname).

To start profiling:

<pre class="prettyprint notranslate" translate="no">`profile("My profile")
`</pre>

To stop profiling and display the results in the Profiles panel:

<pre class="prettyprint notranslate" translate="no">`profileEnd("My profile")
`</pre>

For more examples, see [Controlling the CPU profiler](console#controlling_the_cpu_profiler).

## profileEnd([name])

Stops the current profiling session started with the [profile()](#profilename) method and displays the results in the Profiles panel.

## unmonitorEvents(object[, events])

Stops monitoring events for the specified object and events. For example, the following stops all event monitoring on the `window` object:

<pre class="prettyprint notranslate" translate="no">`unmonitorEvents(window);
`</pre>

You can also selectively stop monitoring specific events on an object. For example, following code starts monitoring all mouse events on the currently selected element, and then stops monitoring "mousemove" events (perhaps to reduce noise in the console output).

<pre class="prettyprint notranslate" translate="no">`monitorEvents($0, "mouse");
unmonitorEvents($0, "mousemove");
`</pre>

Also see [Monitoring events](console#monitoring_events).

## values(object)

Returns an array containing the values of all properties belonging to the specified object.

<pre class="prettyprint notranslate" translate="no">`values(object);
