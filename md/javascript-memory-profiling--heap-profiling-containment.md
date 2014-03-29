Exploring the Heap Contents
==
<iframe
  data-src="/chrome-developer-tools/docs/heap-profiling-containment_9d71f223d2170caf4de1ed091de85474.frame"
  class="framebox inherit-locale "
  style="width: 100%; height: 100px;">

[This section requires a browser that supports JavaScript and iframes.]

</iframe>

<link rel="stylesheet" type="text/css" href="/chrome-developer-tools/css/local_extensions.css"/>

This page demonstrates how object contents can be explored using
the Heap
Profiler. The entire contents can be viewed, regardless of the
current context (as opposed to how debugger works). Moreover, it is
possible to inspect variables of closures, and peek into objects
internals.

Below is the source code of the script, for reference:

<pre class="prettyprint">
function createClosure(a, b, c)
{
  var d = a + b;
  return function() { return c + d; };
}

var closure = createClosure("a", "b", "c");
closure.a = "property a";
closure.d = "property d";

var consString = "aaa / " + document.URL + document.inputEncoding;

var top_in_page = "inside page";

function init()
{
  var iframe = document.createElement("iframe");
  iframe.setAttribute("src", "heap-profiling-containment-files/iframe.html");
  document.getElementById("iframe-host").appendChild(iframe);
}

</pre>

Try this:

*   Take a heap snapshot
*   Open the **Containment** view

The **Containment** view shows several top-level
entries, including `DOMWindow` objects.  By expanding
a `DOMWindow` object, it is possible to examine all of its
properties. This page contains several `iframe`, and it has its
own `DOMWindow` object.

There are several interesting things to explore in the `DOMWindow`
object of the page:

*   Find and expand the `closure` property. It will show
the contents of the closure, as well as properties of the closure
object itself. Color coding is used to distinguish closure variables
from properties.
*   Find and expand the `consString` property. It will
    exhibit, that a concatenated string is stored as a linked list of
    its parts.

Simultaneously, you can view contents of an iframe
   without the need to switch
  contexts.