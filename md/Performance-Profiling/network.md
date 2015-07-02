评估网络性能
===========
Network面板记录了页面中每一次网络操作，包括详细的时序数据、HTTP请求和应答Header、Cookies、WebSocket消息等。可以帮您分析页面的网络性能，如：
* 哪项资源从请求到开始加载花费的时间最长？
* 哪项资源加载花费的时间最长？
* 某个网络请求是由谁发起的？
* 获取某个资源时哪个阶段花费的时间最长？

----------------------------
###关于Resource Timing API
Network面板使用了[Resource Timing API](http://www.w3.org/TR/resource-timing)。这是一个JavaScript API，记录每个资源网络操作的时序数据。例如，该API可以精确的提供何时发出一个图片的HTTP请求和图片最后一个字节到达的时间。下图显示了该API提供网络时序点：
![](https://developer.chrome.com/devtools/docs/network-files/resource-timing-overview.png) 
Resource Timing API可以在任何网页上使用，不仅仅是DevTools。在Chrome中，可通过window.performance对象访问，performance.getEntries()方法返回一个`资源时序对象(resource timing objects)`的数组，每个对象记录了一个请求的网络时序数据。

试一试：打开Chrome的JavaScript控制台，输入如下字符，点击回车
```javascript
window.performance.getEntries()[0]
```
上述代码返回了资源时序对象的第一个元素，下图的控制台中显示了该元素的相关属性：
![](https://developer.chrome.com/devtools/docs/network-files/getentries.png) <br/>
时间戳都使用毫秒为单位，由[High ResolutionTime](http://www.w3.org/TR/hr-time/#sec-high-resolution-time)指定，Chrome中可通过window.performance.now()获得页面停留的毫秒。（译者注：High ResolutionTime是一个定义的接口，可以以毫秒为单位提供当前时间）
###Network面板
打开DevTools后，Network面板会自动记录所有的网络活动。第一次点击进入该面板时，如果是面板内容为空，你需要等待网络活动或重新加载页面。
![](https://developer.chrome.com/devtools/docs/network-files/network-overview.png)
表格每行代表一个请求的资源，注意：
* 并非所有列都默认显示，列头上右键可选择要显示的列
* 有些列包含了多个区域（如Time and Latency）。大资源会显示主区域和次区域，小资源只显示主区域。
* 左键点击列头，可进行排序。Timeline列有点不同，点击列头会显示待排序项。点击[Waterfall view](https://developer.chrome.com/devtools/docs/network#timeline-view)和[Sorting and filtering](https://developer.chrome.com/devtools/docs/network#sorting-and-filtering)可获取更多信息。
  * <b>Name and Path</b>&nbsp;&nbsp;      资源名和URL路径
  * <b>Method</b>&nbsp;&nbsp;      HTTP请求的方法，如: GET/POST.
  * <b>Name and Path</b>&nbsp;&nbsp;      资源名和URL路径
  * <b>Status and Text</b>&nbsp;&nbsp;     HTTP状态代码和文本信息
  * <b>Domain</b>&nbsp;&nbsp;    请求的域名
  * <b>Type</b>&nbsp;&nbsp;    请求资源的MIME类型
  * <b>Initiator</b>&nbsp;&nbsp;       发起请求的对象或进程,有如下几个值：
    * <b>Parser</b>&nbsp;&nbsp;       Chrome HTML解析，如img表情的src
    * <b>Redirect</b>&nbsp;&nbsp;     HTTP重定向
    * <b>Script</b>&nbsp;&nbsp;       有script发起，如document.write加入一个img
    * <b>Other</b>&nbsp;&nbsp;        其他发起请求的行为，如点击链接到其他页面或在地址栏输入URL
  * <b>Cookies</b>&nbsp;&nbsp;        请求携带的cookies数量，具体的cookies可以在[Cookies tab](https://developer.chrome.com/devtools/docs/network#cookies)查看
  * <b>Set-Cookies</b>&nbsp;&nbsp;        The number of cookies set in the HTTP request.请求设置的cookies数量
  * <b>Size and Content</b>&nbsp;&nbsp;        Size指response头(通常几百字节)加上body的大小。Content指资源解码后的内容。如果资源是从浏览器缓存中获得的，则这个字段显示cache
  * <b>Time and Latency</b>&nbsp;&nbsp; Time记录从请求到接收最后一个字节的时间，Latency记录加载相应第一个字节的时间。
  * <b>Timeline</b>&nbsp;&nbsp; Timeline列显示一个可视的网络请求瀑布流，点击列头显示待排序项。
  
####转向时保存日志
默认情况下，转向或重新加载时当前的网络日志会被丢弃。若想保存网络日志，点击Network面板下的按钮为黑色![](https://developer.chrome.com/devtools/images/recording-off.png)，新的日志会被添加到表格下方；该按钮为红色时![](https://developer.chrome.com/devtools/images/recording-on.png)，则不会保存日志。

####排序和过滤
默认情况下，Network面板表格中的内容，由资源请求顺序决定。您可以通过点击相关列的列头来进行排序，再次点击则反向排序。<br/>
![](https://developer.chrome.com/devtools/docs/network-files/sorting.png)<br/>
Timeline列与其他列不同，点击时，显示一个可排序项目的列表。
![](https://developer.chrome.com/devtools/docs/network-files/timeline-column.png)<br/>
该列表包含如下选项：
* <b>Timeline</b>&nbsp;&nbsp;— 根据网络请求的起始时间排序
* <b>Start Time</b>&nbsp;&nbsp;— 根据网络请求的起始时间排序（与Timeline相同）
* <b>Response Time</b>&nbsp;&nbsp;— 根据请求的响应时间排序
* <b>End Time</b>&nbsp;&nbsp;— 根据请求完成的时间排序
* <b>Duration</b>&nbsp;&nbsp;— 根据请求花费的时间排序
* <b>Latency</b>&nbsp;&nbsp;— 根据请求开始到开始响应的时间排序(传送首字节的时间).

如果只想显示某种类型的资源，可点击面板下方过滤按钮，选择`Documents`, `Stylesheets`, `Images`, `Scripts`, `XHR`, `Fonts`, `WebSockets`和`Other`。下面的截屏只显示了CSS资源，若想显示所有类型的资源，点击 `All`即可。
![](https://developer.chrome.com/devtools/docs/network-files/filter-type.png)

####高级过滤
除了可根据资源类型过滤外，您还可以进行过滤查询以缩小资源列表。例如，为了找出所有响应为200的资源，可以在输入框中输入`StatusCode:200`。
![](https://developer.chrome.com/devtools/docs/network-files/network-advanced-filter.png)

您需要注意：过滤查询包含一个类型(StatusCode)和值(200)。过滤查询不分大小写，输入类型时会有自动补全建议，点击Tab键可选择和切换。过滤查询的值会根据表格中的记录显示自动补全建议,点击上下键可选择建议项并预览结果。查询相反结果，可在查询字符串前加入-，如（ -StatusCode:200）。【译者注：我所使用的版本，自动补全功能未能看到，输入StatusCode:200也未进行过滤，可是我使用的最新版本啊。。。】

所有可用的过滤类型：
* <b>domain</b>&nbsp;&nbsp; 资源URL的域名部分，如：www.google-analytics.com。
* <b>has-response-header</b>&nbsp;&nbsp; 检查响应资源的header是否包含指定值.如：Access-Control-Allow-Origin。
* <b>is</b>&nbsp;&nbsp; 显示请求是否在执行中，如is:running。
* <b>larger-than</b>&nbsp;&nbsp; 请求传输内容大小是否大于指定值，默认单位为byte，也可指定kb和mb。如larger-than:50, larger-than:150k, larger-than:2m。
* <b>method</b>&nbsp;&nbsp; HTTP请求方法，如：GET。
* <b>mime-type</b>&nbsp;&nbsp; 资源类型Content-type 如：text/html。
* <b>scheme</b>&nbsp;&nbsp; URL中的协议类型，如：https。
* <b>set-cookie-name</b>&nbsp;&nbsp; 过滤包含的cookie名，如:loggedIn (假设cookie为loggedIn=true).
* <b>set-cookie-value</b>&nbsp;&nbsp; 过滤包含的cookie值。如：true (假设cookie为loggedIn=true)。
* <b>set-cookie-domain</b>&nbsp;&nbsp; 服务器设置cookie的域名，如：foo.com (假设假设cookie为loggedIn=true; Domain=foo.com; Path=/; Expires=Wed, 13 Jan 2021 22:23:01 GMT; HttpOnly)。
* <b>status-code</b>&nbsp;&nbsp; HTTP响应码。

使用上述列表的查询，需要组织成<类型>:<值>的格式。如果想要使用自动补全功能，请确保输入正确的类型。

####显示/隐藏列
您可以改变Network表格中所显示的列，在表头右键点击或者Control+click(Mac)，即可选择或反选列名，从而显示/隐藏相应的列。
![](https://developer.chrome.com/devtools/docs/network-files/add-remove-columns.png)

####改变行的大小
您在查看表格时，行的大小可以改变。点击Network面板下方的小行按钮可以进行切，为蓝色按钮时![](https://developer.chrome.com/devtools/images/small-resource-rows.png)显示大行，再次点击后，切换为灰色![](https://developer.chrome.com/devtools/images/large-resource-rows.png)显示小行。大行状态下，某些列将显示两个属性，小行时只显示主属性。
![](https://developer.chrome.com/devtools/docs/network-files/small-rows.png)

####瀑布流

瀑布流根据加载每项资源所花费的时间（从HTTP请求开始到接收到响应的最后一个字节为止）进行绘制。资源的加载时间通过一个条形栏表示，该条形栏以颜色来表示各种类型的时间花费，每种颜色表示一个获取资源的阶段，条形栏越长表示请求的数据越多。
![](https://developer.chrome.com/devtools/docs/network-files/network-timeline.png)
当鼠标移入条形栏时，会显示网络消耗时间的具体数据。与[Timing details tab view](https://developer.chrome.com/devtools/docs/network#resource-network-timing)显示的数据一致。
![](https://developer.chrome.com/devtools/docs/network-files/timeline-view-hover.png)


瀑布流分别用蓝色和红色的竖线高亮显示DOMContentLoaded和load事件，当引擎完成主document的解析时，DOMContentLoaded事件被触发；取回所有页面的资源后，load事件被触发。
![](https://developer.chrome.com/devtools/docs/network-files/dom-lines.png)


####保存/拷贝网络信息
在Network表格中点击右键或 Ctrl + Click (Mac)，会出现一个菜单，上面包含很多选项。这些选项应用于某行，比如拷贝一个HTTP请求头，有些用于整个网络记录，如saving a Network recording as a HAR file选项。
![](https://developer.chrome.com/devtools/docs/network-files/right-click.png)

以下菜单选项应用于选择的资源：
* <b>Open Link in New Tab</b>&nbsp;&nbsp; 在新选项卡中打开这个资源，您也可以双击资源名称打开。
* <b>Copy Link Address</b>&nbsp;&nbsp; 拷贝资源URL到系统剪切板。
* <b>Copy Request Headers</b>&nbsp;&nbsp;  拷贝HTTP请求头到系统剪切板。
* <b>Copy Response Headers</b>&nbsp;&nbsp; 拷贝HTTP响应头到系统剪切板。
* <b>Copy as cURL</b>&nbsp;&nbsp; 将网络请求作为[cURL](http://curl.haxx.se/)命令拷贝到系统剪切板。
* <b>Replay XHR</b>&nbsp;&nbsp; 如果请求为一个XMLHTTPRequest，重新发送这个请求。

####将请求拷贝为cURL命令
[cURL](http://curl.haxx.se/)是一个HTTP命令行工具。将请求拷贝为cURL命令重新创建一个HTTP请求（包括HTTP头，SSL证书和查询参数字符串），并将其转换为一个cURL命令字符串拷贝到系统剪切板，您可以将该字符串粘贴到cURL输入区域执行相同的请求。

下面是一个XHR请求转换的cURL命令行的例子：
```javascript
curl 'http://news.google.com/news/xhrd=us' -H 'Accept-Encoding: gzip,deflate,:sdch' 
-H 'Host: news.google.com' -H 'Accept-Language: en-US,en;q=0.8' 
-H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_8_3) 
AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1510.0 Safari/537.36' 
-H 'Accept: */*' 
-H 'Referer: http://news.google.com/nwshp?hl=en&tab=wn' 
-H 'Cookie: NID=67=eruHSUtoIQA-HldQn7U7G5meGuvZOcY32ixQktdgU1qSz7StUDIjC_
Knit2xEcWRa-e8CuvmADminmn6h2_
IRpk9rWgWMdRj4np3-DM_ssgfeshItriiKsiEXJVfra4n; PREF=ID=a38f960566524d92:U=af866b8c07132db6:FF=0:
TM=1369068317:LM=1369068321:S=vVkfXySFmOcAom1K' 
-H 'Connection: keep-alive' --compressed
```

####保存网络数据
您可以将网络记录保存为HAR文件(HTTP Archive) ，或者将这些记录组织为HAR结构并拷贝到剪切板。HAR文件是一个JSON数据，内容为网络的瀑布流信息，一些第三方工具可以根据这些数据重建瀑布流。<br/>
保存记录：<br/>
1.在Network面板表格上右键点击或Control+click。<br/>
2.在弹出的菜单中，选择:<br/>
* <b>Copy All as HAR</b>&nbsp;&nbsp;— Copies the network recording to the system clipboard in the HAR format.将网络日志以JSON格式进行拷贝。
* <b>Save as HAR with Content</b>&nbsp;&nbsp;将请求资源的所有网络日志到一个HAR文件中，包括二进制文件，包括图片，以Base64编码的文本。

###网络资源细节

当您在Network表格中点击一个资源名时，会显示一个多页选项卡，其中包含了如下细节：
* <b>HTTP request and response headers</b>&nbsp;&nbsp;HTTP请求和响应头
* <b>Resource preview</b>&nbsp;&nbsp;资源预览
* <b>HTTP response</b>&nbsp;&nbsp;HTTP响应
* <b>Cookie names and values</b>&nbsp;&nbsp;Cookie名称和值
* <b>WebSocket messages</b>&nbsp;&nbsp;WebSocket消息
* <b>Resource network timing</b>&nbsp;&nbsp;Resource网络计时

####HTTP请求和响应头

Headers选项卡显示资源请求的URL、HTTP方法和响应代码。另外，还列出了请求和响应的header及其值，还包括了查询字符参数。您可以通过点击Headers右边的`View parsed/View source`链接查看经过解析和格式化后的HTTP header或其源报文。您还可以通过点击query string parameters右边的`View decoded/View URL encoded`链接，查看解码后和解码后的请求参数。
![](https://developer.chrome.com/devtools/docs/network-files/network-headers.png)
您也可以通过使用`保存网络数据`中提到的方法，将Header拷贝到系统剪切板。

####资源预览

Preview选项卡在可预览时将会显示资源的状态。目前可以预览图像和JSON资源。
![](https://developer.chrome.com/devtools/docs/network-files/resource-preview-json.png)
![](https://developer.chrome.com/devtools/docs/network-files/network-image-preview.png)
您可以在`Response`选项卡查看未被格式化过的相应内容。

####HTTP响应

Response选项卡包含了未格式化过的资源内容。下面的截屏展现了一个为JSON数据的相应。
![](https://developer.chrome.com/devtools/docs/network-files/response.png)<br/>
您可以在`资源预览`选项卡中查看格式化后的JSON对象和图像。

####Cookie名称和值

Cookie选项卡将HTTP请求和响应Header中包含的所有Cookie用一个表格来展现，您可以清除所有Cookie
![](https://developer.chrome.com/devtools/docs/network-files/cookies.png)<br/>
Cookies表格包含如下列：<br/>
* <b>Name</b>&nbsp;&nbsp;cookie的名字
* <b>Value</b>&nbsp;&nbsp;cookie的值
* <b>Domain</b>&nbsp;&nbsp;cookie所属的域名
* <b>Path</b>&nbsp;&nbsp;cookie来源的URL路径
* <b>Expires / Max-Age</b>&nbsp;&nbsp;cookie过期/最大生命周期
* <b>Size</b>&nbsp;&nbsp;cookie的大小
* <b>HTTP</b>&nbsp;&nbsp;该属性表示cookie在HTTP请求中只可被浏览器设置，不能被JavaScript操作
* <b>Secure</b>&nbsp;&nbsp;该属性表示cookie只能传输在一个可靠连接中

####WebSocket消息

Frames选项卡显示通过WebSocket发送和接收到的消息。该选项卡只有在被选资源发起WebSocket连接时才被展现。表格包含如下列：<br/>
* <b>Data</b>&nbsp;&nbsp;有效的消息，若为纯文本，将会显示。对于二进制操作码，该列会显示操作码的名字和代码。支持的操作码如下：
 * <b>Continuation Frame</b>&nbsp;&nbsp;
 * <b>Binary Frame</b>&nbsp;&nbsp;
 * <b>Connection Close Frame</b>&nbsp;&nbsp;
 * <b>Ping Frame</b>&nbsp;&nbsp;
 * <b>Pong Frame</b>&nbsp;&nbsp;
* <b>Length</b>&nbsp;&nbsp;消息的字节数
* <b>Time</b>&nbsp;&nbsp;消息创建的时间戳
消息根据类型显示不同的颜色，发出的消息用淡绿色表示，收到的消息用白色表示。
![](https://developer.chrome.com/devtools/docs/network-files/websocket-text2.png)<br/>
WebSocket操作码用淡黄色表示：
![](https://developer.chrome.com/devtools/docs/network-files/frames-opcode.png)<br/>
错误用淡红色表示。<br/>

主要事项：<br/>
* 点击左边的资源名，可以刷新Frames表格，以显示最新收到的消息。
* Frames表格只能保存最多100条记录

####Resource网络计时

Timing选项卡绘制请求资源时网络上各个阶段所消耗的时间，与`瀑布流`中展现的内容一致。
![](https://developer.chrome.com/devtools/docs/network-files/timing.png)
* <b>Stalled/Blocking</b>&nbsp;&nbsp;请求在发出前的等待时间。该时间包括了代理服务器的时间，另外，也包括了浏览器的等待时间(如根据Chrome原始的最大6个TCP连接的原则，浏览器会将一个已经完成的连接进行复用，这期间会花费一定的时间)。
* <b>Proxy Negotiation</b>&nbsp;&nbsp;与代理服务器交互的时间
* <b>DNS Lookup</b>&nbsp;&nbsp; DNS查找IP的时间，页面上每个新域名需要一个完整路由才能完成查找。
* <b>Initial Connection / Connecting</b>&nbsp;&nbsp; 创建一个连接花费的时间，包括TCP握手/重试和SSL验证
* <b>SSL</b>&nbsp;&nbsp; SSL握手的时间
* <b>Request Sent / Sending</b>&nbsp;&nbsp; 代表网络请求花费的时间，一般不超过1毫秒。
* <b>Waiting (TTFB)</b>&nbsp;&nbsp; 等待响应的时间，一般称为`首字节时间Time To First Byte`，包括路由到服务器加上服务器发出响应的时间。
* <b>Content Download / Downloading</b>&nbsp;&nbsp; 接收数据所花费的时间。

###其他资源

为了优化WEB应用的网络性能，您可查阅如下资源：
* 使用[PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights)和[PageSpeed optimization tools](https://developers.google.com/speed/pagespeed/optimization)优化您的网站性能。
* [High Performance Networking in Google Chrome](http://www.igvita.com/posa/high-performance-networking-in-google-chrome/)讨论了Chrome网络内部原来和怎么运用在您的站点中。
* [How gzip compression works](https://developers.google.com/speed/articles/gzip)深入讨论了gzip压缩和为什么要用它。
* [Web Performance Best Practices](https://developers.google.com/speed/docs/best-practices/rules_intro)提供了一些帮助您优化网站性能的小技巧。
