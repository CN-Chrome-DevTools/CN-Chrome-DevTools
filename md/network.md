Evaluating network performance
==

The Network panel records information about each network operation in your application, including detailed timing data, HTTP request and response headers, cookies, WebSocket data, and more. The Network panel helps you answer questions about the network performance of your web application, such as:

*   Which resource had the slowest time to first byte?
*   Which resources took the longest time to load (duration)?
*   Who initiated a particular network request?
*   How much time was spent in the various network phases for a particular resource?
<nav class="inline-toc">

1.  [About the Resource Timing API](#about_the_resource_timing_api)
2.  [Network panel overview](#network_panel_overview)

        1.  [Preserving the network log upon navigation](#preserving_the_network_log_upon_navigation)
    2.  [Sorting and filtering](#sorting_and_filtering)
    3.  [Adding and removing table columns](#adding_and_removing_table_columns)
    4.  [Changing resource row sizes](#changing_resource_row_sizes)
    5.  [Timeline view](#timeline_view)
    6.  [Saving and copying network information](#saving_and_copying_network_information)
3.  [Network resource details](#network_resource_details)

        1.  [HTTP headers](#http_headers)
    2.  [Resource previews](#resource_previews)
    3.  [HTTP response](#http_response)
    4.  [Cookies](#cookies)
    5.  [WebSocket frames](#websocket_frames)
    6.  [Resource network timing](#resource_network_timing)
4.  [Additional resources](#additional_resources)</nav>

## About the Resource Timing API

The Network panel uses the [Resource Timing API](http://www.w3.org/TR/resource-timing), a JavaScript API that provides detailed network timing data for each loaded resource. For example, the API can tell you precisely when the HTTP request for an image started, and when the image's final byte was received. The following illustration shows the network timing data points that the Resource Timing API provides.

![Resource timing overview](network-files/resource-timing-overview.png)

The API is available to any web page, not just DevTools. In Chrome, it's exposed as methods on the global `window.performance` object. The `performance.getEntries()` method returns an array of "resource timing objects", one for each requested resource on the page.

Try this: open the JavaScript console on the current page, enter the following at the prompt, and hit Return:

    window.performance.getEntries()[0]
    `</pre>

    This evaluates the first element in the array of resource timing objects and displays its properties in the console, as shown below.

    ![Performance resource timing](network-files/getentries.png)

    Each timestamp is in microseconds, following the [High Resolution
    Time](http://www.w3.org/TR/hr-time/#sec-high-resolution-time) specification. This API is [available in
    Chrome](http://updates.html5rocks.com/2012/08/When-milliseconds-are-not-enough-performance-now) as the `window.performance.now()` method.

    ## Network panel overview

    The Network panel automatically records all network activity while DevTools is open. The first time you open the panel it may be empty. Reload the page to start recording, or simply wait for network activity to occur in your application.

    ![Network overview](network-files/network-overview.png)

    Each requested resource is added as a row to the Network table, which contains the columns listed below. Note the following about the Network table:

*   Not all columns listed below are visible by default; you can easily [show or hide columns](#adding_and_removing_table_columns).
*   Some columns contain a primary field and a secondary field (**Time** and **Latency**, for example). When viewing the Network table with [large resource rows](#changing_resource_row_size) both fields are shown; when using small resource rows only the primary field is shown.
*   You can [sort](#sorting_and_filtering) the table by a column's value by clicking the column header. The [the Timeline column](#timeline_view) behaves a bit differently: clicking its column header displays a menu of additional sort fields. See [Timeline view](#timeline_view) and [Sorting and filtering](#sorting_and_filtering) for more information.
    <table>
    <thead>
    <tr>
    <th width="20%">Field</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td>**Name** and **Path**</td>
    <td>The name and URL path of the resource, respectively.</td>
    </tr>
    <tr>
    <td>**Method**</td>
    <td>The HTTP method used for the request (GET or POST, for example).</td>
    </tr>
    <tr>
    <td>**Status** and **Text**</td>
    <td>The HTTP status code and text message, respectively.</td>
    </tr>
    <tr>
    <td>**Domain**</td>
    <td>The domain of the resource request.</td>
    </tr>
    <tr>
    <td>**Type**</td>
    <td>The MIME type of the requested resource.</td>
    </tr>
    <tr>
    <td>**Initiator**</td>
    <td>The object or process that initiated the request. It can have one of the following values:
      <dl>
        <dt>Parser</dt>
        <dd>Chrome's HTML parser initiated the request.</dd>
        <dt>Redirect</dt>
        <dd>A HTTP redirect initiated the request.</dd>
        <dt>Script</dt>
        <dd>A script initiated the request.</dd>
        <dt>Other</dt>
        <dd>Some other process or action initiated the request, such as the user navigating to a page via a link, or by entering a URL in the address bar.</dd>
      </dl>
    </td>
    </tr>
    <tr>
    <td>**Cookies**</td>
    <td>The number of cookies transferred in the request. These correspond to the cookies shown in the [Cookies tab](#request_and_response_cookies) when viewing details for a given resource.</td>
    </tr>
    <tr>
    <td>**Set-Cookies**</td>
    <td>The number of cookies set in the HTTP request.</td>
    </tr>
    <tr>
    <td>**Size** and **Content**</td>
    <td>Size is the combined size of the response headers (usually a few hundred bytes) plus the response body, as delivered by the server.
    Content is the size of the resource's decoded content.
    If the resource was loaded from the browser's cache rather than over the network, this field will contain the text (from cache).</td>
    </tr>
    <tr>
    <td>**Time** and **Latency**</td>
    <td>Time is total duration, from the start of the request to the receipt of the final byte in the response.
    Latency is the time to load the first byte in the response.</td>
    </tr>
    <tr>
    <td>**Timeline**</td>
    <td>The Timeline column displays a [timeline view](#timeline_view) of all network requests. Clicking the header of this column reveals a menu of additional sorting fields. See Timeline view and Sorting and filtering for details.</td>
    </tr>
    </tbody>
    </table>

    ### Preserving the network log upon navigation

    By default, the current network record log is discarded when you navigate to another page, or reload the current page. To preserve the recording log in these scenarios, click the black **Preserve log upon navigation** button ![Don](network-files/keep-log-off.png) at the bottom of the Network panel; new records are appended to the bottom of the table. Click the same button again (now red ![Preserve resources on navigation](network-files/keep-log-on.png)) to disable log preservation.

    ### Sorting and filtering

    By default, resources in the Network table are sorted by the start time of each request (the network "waterfall"). You can sort the table by another column value by clicking the column header. Click the header again to change the sort order (ascending or descending).

    ![Sort by](network-files/sorting.png)

    The Timeline column is unique from the others in that, when clicked, it displays a menu of additional sort fields.

    ![Timeline column](network-files/timeline-column.png)

    The menu contains the following sorting options:

*   **Timeline** — Sorts by the start time of each network request. This is the default sort, and is the same as sorting by the Start Time option).
*   **Start Time** — Sorts by the start time of each network request (same as sorting by the Timeline option).
*   **Response Time** — Sorts by each request's response time.
*   **End Time** — Sorts by the time when each request completed.
*   **Duration** — Sorts by the total time of each request.
*   **Latency** — Sorts by the time between the start of the request and the beginning of the response (also known as the "time to first byte").

    To filter the Network table to only show certain types of resources, click one of the content types along the bottom of the panel: **Documents**, **Stylesheets**, **Images**, **Scripts**, **XHR**, **Fonts**, **WebSockets**, and **Other**. In the following screenshot only CSS resources are shown. To view all content types, click the **All** filter button.

    ![Filter type](network-files/filter-type.png)

    ### Adding and removing table columns

    You can change the default set of columns displayed by the Network table. To show or hide a column, Right+click or Control+click (Mac only) in the table header and select or deselect column names from the list.

    ![Add or remove columns](network-files/add-remove-columns.png)

    ### Changing resource row sizes

    You can view the Network table with large resource rows (the default), or small resource rows. Click the blue **Use small resource rows** toggle button ![Small resource rows](network-files/small-resource-rows.png) at the bottom of the panel to view small rows. Click the same button (now gray ![Large resource rows](network-files/large-rows.png)) to view large resource rows again. Large rows enable some columns to display two text fields: a primary field and a secondary field (Time and Latency, for instance). When viewing small rows only the primary field is displayed.

    In the following screenshot, the Network table is viewed with small resource rows and just the Timeline column.

    ![Resized resource rows](network-files/small-rows.png)

    ### Timeline view

    The Timeline view in the Network panel graphs the time it took to load each resource, from the start of the HTTP request to the receipt of the final byte of the response. Each resource loading time is represented as a bar, color-coded according to the resource type. The length of the lighter-shaded part of each bar represents the request's latency, while the length of the darker-shaded part represents the time spent receiving the response data.

    ![Network timeline view](network-files/network-timeline.png)

    When you hover your mouse over a timeline row (but not over an actual bar) the request's latency and receipt time are displayed above the corresponding bar's light- and dark-shaded areas, respectively, as shown below.

    ![Timeline view](network-files/timeline-view-1.png)

    If you hover your mouse over the timeline bar itself, the complete timing data for the resource is presented in a pop-up. This is the same information that's presented in the [Timing details tab](#resource_network_timing) for a given resource.

    ![Timeline view on hover](network-files/timeline-view-hover.png)

    The timeline indicates when the the [`DOMContentLoaded`](http://docs.webplatform.org/wiki/dom/events/DOMContentLoaded)
    and [`load`](http://docs.webplatform.org/wiki/dom/events/load) events were fired with blue and red vertical lines, respectively. The `DOMContentLoaded` event is fired when the main document had been loaded and parsed. The `load` event is fired when all of the page's resources have been downloaded.

    ![DOM event lines](network-files/dom-lines.png)

    Timeline bars are color-coded as follows:

    <style>
    #colortable {
      width: 50%;
      border: none;
    }

    #colortable td {
      border: none;
    }

    .doc { background: rgba(47, 102, 236, 0.6); width: 10%;}
    .css { background: rgba(157, 231, 119, 0.6);width: 10%;}
    .images { background: rgba(164, 60, 255, 0.6);width: 10%;}
    .scripts { background: rgba(255, 121, 0, 0.6);width: 10%;}
    .xhr { background: rgba(231, 231, 10, 0.6);width: 10%;}
    .fonts { background: rgba(255, 82, 62,0.6);width: 10%;}
    .other { background: rgba(187, 187, 188, 0.6);width: 10%;}
    </style>

    <!-- TODO: Fix formatting of cells -->

    <table id="colortable">
    <tr>
    <td class="doc"></td>
    <td>Documents</td>
    </tr>
    <tr>
    <td class="css"></td>
    <td>Stylesheets</td>
    </tr>
    <tr>
    <td class="images"></td>
    <td>Images</td>
    </tr>
    <tr>
    <td class="scripts"></td>
    <td>Scripts</td>
    </tr>
    <tr>
    <td class="xhr"></td>
    <td>XHR</td>
    </tr>
    <tr>
    <td class="fonts"></td>
    <td>Fonts</td>
    </tr>
    <tr>
    <td class="other"></td>
    <td>Other</td>
    </tr>
    </table>

    ### Saving and copying network information

    <span class="kbd">Right-clicking</span> or <span class="kbd">Ctrl</span> + <span class="kbd">Click</span> (Mac only) within the Network table a context menu appears with several actions. Some of these actions apply to the resource row under the mouse click (like [copying HTTP request headers](#copying_requests_as_curl_commands)), while others apply to the entire network recording (such as [saving a Network recording as a HAR file](#saving_network_data)).

    ![Right-click on Network](network-files/right-click.png)

    The following menu actions apply to the selected resource:

*   **Open Link in New Tab** — Opens the resource in a new tab. You can also double-click the resource name in the Network table.
*   **Copy Link Address** — Copies the resource URL to the system clipboard.
*   **Copy Request Headers** — Copies the HTTP request headers to the system clipboard.
*   **Copy Response Headers** — Copies the HTTP response headers to the system clipboard.
*   **Copy as cURL** — Copies the network request as a
      [cURL](http://curl.haxx.se/) command string to the system clipboard. See [Copying requests as cURL commands](#copying_requests_as_curl_commands).
*   **Replay XHR** — If the associated request is an XMLHTTPRequest, re-sends the original XHR.

    #### Copying requests as cURL commands

    [cURL](http://curl.haxx.se/) is a command line tool for making HTTP transactions. The Network panel's **Copy as cURL** command recreates an HTTP request (including HTTP headers, SSL certificates, and query string parameters) and copies it as a cURL command string to the clipboard. You can then paste the string into a terminal window (on a system with cURL) to execute the same request.

    Below is an example cURL command line string taken from a XHR request on the Google News home page.

    <pre class="prettyprint notranslate" translate="no">`curl 'http://news.google.com/news/xhrd=us' -H 'Accept-Encoding: gzip,deflate,:sdch' -H 'Host: news.google.com' -H 'Accept-Language: en-US,en;q=0.8' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_8_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1510.0 Safari/537.36' -H 'Accept: */*' -H 'Referer: http://news.google.com/nwshp?hl=en&amp;tab=wn' -H 'Cookie: NID=67=eruHSUtoIQA-HldQn7U7G5meGuvZOcY32ixQktdgU1qSz7StUDIjC_Knit2xEcWRa-e8CuvmADminmn6h2_IRpk9rWgWMdRj4np3-DM_ssgfeshItriiKsiEXJVfra4n; PREF=ID=a38f960566524d92:U=af866b8c07132db6:FF=0:TM=1369068317:LM=1369068321:S=vVkfXySFmOcAom1K' -H 'Connection: keep-alive' --compressed

#### Saving network data

You can save the data from a network recording as a HAR ([HTTP Archive](http://www.softwareishard.com/blog/har-12-spec/)) file, or copy the records as a HAR data structure to your clipboard. A HAR file contains a JSON data structure that describes the network "waterfall". Several [third-party](http://ericduran.github.io/chromeHAR/) [tools](https://code.google.com/p/harviewer/) can reconstruct the network waterfall from the data in the HAR file.

**To save a recording:**

1.  Right+click or Control+click on the Network table.
2.  In the context menu that appears, choose one of the following actions:

        *   **Copy All as HAR** — Copies the network recording to the system clipboard in the HAR format.
    *   **Save as HAR with Content** — Saves all network data to a HAR file along with each page resource. Binary resources, including images, are encoded as Base64-encoded text.

For more information, [Web Performance Power Tool: HTTP Archive (HAR)](http://www.igvita.com/2012/08/28/web-performance-power-tool-http-archive-har/).

## Network resource details

When you click a resource name in the Network table a tabbed window appears that contains the following additional details:

*   [HTTP request and response headers](#http_headers)
*   [Resource preview](#resource_previews)
*   [HTTP response](#http_response)
*   [Cookie names and values](#cookies)
*   [WebSocket messages](#websocket_frames)
*   [Resource network timing](#resource_network_timing)

### HTTP headers

The Headers tab displays the resource's request URL, HTTP method, and response status code. Additionally, it lists the HTTP response and request headers and their values, and any query string parameters. You can view HTTP headers parsed and formatted, or in their source form by clicking the **View parsed**/**View source** toggle button, respectively, located next to each header's section. You can also view parameter values in their decoded or URL encoded forms by clicking the **View decoded**/**View URL encoded** toggle button next to each query string section.

![Network headers](network-files/network-headers.png)

You can also [copy request and response headers](#saving_and_copying network_information) to your clipboard.

### Resource previews

The Preview tab displays a preview of the resource, when available. Previews are currently displayed for image and JSON resources, as shown below.

![Resource JSON preview](network-files/resource-preview-json.png)

![Resource image preview](network-files/network-image-preview.png)

You can view the resource's unformatted response on the [Response
tab](#http_response).

### HTTP response

The Response tab contains the resource's unformatted content. Below is a screenshot of a JSON data structure that was returned as the response for a request.

![Resource response preview](network-files/response.png)

You can also [view formatted previews](#resource_previews) of some resource types, including JSON data structures and images.

### Cookies

The Cookies tab displays a table of all the cookies transmitted in the
resource's HTTP request and response headers. You can also [clear all cookies](#right-click_menu_actions).

![Resource cookies](network-files/cookies.png)

The Cookies table contain the following columns:

<!-- TODO: Fix formatting of cells -->

<table>
<tr>
<th width="20%">Property</th>
<th>Description</th>
</tr>
<tbody>
<tr>
<td>**Name**</td>
<td>The cookie's name.</td>
</tr>
<tr>
<td>**Value**</td>
<td>The cookie's value.</td>
</tr>
<tr>
<td>**Domain**</td>
<td>The cookie's domain.</td>
</tr>
<tr>
<td>**Path**</td>
<td>The cookie's URL path.</td>
</tr>
<tr>
<td>**Expires / Max-Age**</td>
<td>The value of the cookie's expires or max-age properties.</td>
</tr>
<tr>
<td>**Size**</td>
<td>The size of the cookie in bytes.</td>
</tr>
<tr>
<td>**HTTP**</td>
<td>This indicates that the cookie should only be set by the browser in the HTTP request, and cannot be accessed with JavaScript. </td>
</tr>
<tr>
<td>**Secure**</td>
<td>The presence of this attribute indicates that the cookie should only be transmitted over a secure connection.</td>
</tr>
</tbody>
</table>

### WebSocket frames

The Frames tab shows messages sent or received over a WebSocket connection. This tab is only visible when the selected resource initiated a WebSocket connection. The table contains the following columns:

<table>
<tr>
<th width="20%">Name</th>
<th>Description</th>
</tr>
<tr>
<td>Data</td>
<td>The message payload. If the message is plain text, it's displayed here. For binary opcodes, this field displays the opcode's name and code. The following opcodes are supported:
  <dl>
    <dt>Continuation Frame</dt>
    <dt>Binary Frame</dt>
    <dt>Connection Close Frame</dt>
    <dt>Ping Frame</dt>
    <dt>Pong Frame</dt>
  </dl>
</tr>
<tr>
<td>Length</td>
<td>The length of the message payload in bytes.</td>
</tr>
<tr>
<td>Time</td>
<td>The time stamp when the message was created.</td>
</tr>
</table>

Messages are color-coded according to their type. Outgoing text messages are color-coded light-green; incoming text messages are white:

![Websocket text](network-files/websocket-text2.png) 

WebSocket opcodes are light-yellow:

![Websocket opcodes](network-files/frames-opcode.png) 

Errors are light-red.

**Notes about current implementation:**

*   To refresh the Frames table after new messages arrive, click the resource name on the left.
*   Only the last 100 WebSocket messages are preserved by the Frames table.

### Resource network timing

The Timing tab graphs the time spent on the various network phases involved loading the resource. This is the same data displayed when you hover over a resource bar in the [Timeline view](#timeline_view).

![Resource network timing graph](network-files/timing.png)

The table below lists the network phases shown in the Timing tab and their descriptions.

<!-- TODO: Fix formatting of cells -->

<table>
<tr>
<th style="width:20%">Property</th>
<th>Description</th>
</tr>
<tr>
<td>**Proxy**</td>
<td>Time spent negotiating with a proxy server connection.</td>
</tr>
<tr>
<td>**DNS Lookup**</td>
<td>Time spent performing the DNS lookup. You want to minimize DNS look ups.</td>
</tr>
<tr>
<td>**Blocking**</td>
<td>Time the request spent waiting for an already established connection to become available for re-use.</td>
</tr>
<tr>
<td>**Connecting**</td>
<td>Time it took to establish a connection, including TCP handshakes/retries, DNS lookup, and time connecting to a proxy or negotiating a secure-socket layer (SSL). </td>
</tr>
<tr>
<td>**Sending**</td>
<td>Time spent sending the request.</td>
</tr>
<tr>
<td>**Waiting**</td>
<td>Time spent waiting for the initial response.</td>
</tr>
<tr>
<td>**Receiving**</td>
<td>Time spent receiving the response data. </td>
</tr>
</table>

## Additional resources

To learn more optimizing the network performance of your application, see the following resources:

*   Use [PageSpeed Insights](/speed/pagespeed/insights) to identify performance best practices that can be applied to your site, and [PageSpeed optimization tools](/speed/pagespeed/optimization) to automate the process of applying those best practices.
*   [High Performance Networking in Google
  Chrome](http://www.igvita.com/posa/high-performance-networking-in-google-chrome/) discusses Chrome network internals and how you can take advantage of them to make your site faster.
*   [How gzip compression works](/speed/articles/gzip) provides a high level overview gzip compression and why it's a good idea.
*   [Web Performance Best Practices](/speed/docs/best-practices/rules_intro) provides additional tips for optimizing the network performance of your web page or application.