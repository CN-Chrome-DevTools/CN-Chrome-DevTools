#Chrome DevTools   
*此頁 <https://developer.chrome.com/devtools> 文章內容（Chrome DevTools Overview） 
即 **“Learn Basics 了解基礎知識”** 中的 **“Overview (概述) :** <https://developer.chrome.com/devtools/index>"*
  
---
  
##Learn Basics 了解基礎知識 
1. ####Overview (概述)  
<https://developer.chrome.com/devtools/index>  
本頁章節標題：  
	* `#access` : How to access the DevTools （如何存取 DevTools）
	* `#devtools-window` : The DevTools window	（ DevTools 視窗）
	* `#dom-and-styles` : Inspecting the DOM and style （檢查 DOM 和樣式）
	* `#console` : Working with the console（使用控制台工作）
	* `#debugging-javascript` : Debugging JavaScript（JavaScript 的偵錯）
	* `#improving-network-performance` : Improving network performance （提升網路效能）
	* `#audits` : Audits（審核）
	* `#timeline` : Improving rendering performance（提升渲染的性能）
	* `#javascript-performance` : JavaScript CSS performance（CSS JavaScript 的性能）
	* `#inspecting-storage` : Inspecting storage（檢測存儲器）
	* `#further-reading` : Further reading	（進一步閱讀）
	* `#further-resources` : Further resources +（進一步的資源 +）
		* `#get-more` : Get more（獲得更多）
		* `#take-the-course` : Take the course（取得課程）
		* `#get-involved` : Get involved（參與）
		* `#debugging-extensions` : Debugging extensions（除錯延伸）
    
2. ####Development Workflow	 (開發流程)  
<https://developer.chrome.com/devtools/docs/authoring-development-workflow>  

	* `#dock-to-right` : Dock-To-Right For Vertical-Split Editing （將 devtool 界面區分割在瀏覽器右方，與網頁畫面呈左右對照狀態編輯）  
	 *“dock” 為碼頭，即 devtool 界面區要靠（分割）在瀏覽器的哪個邊界（下部或是右半部）。*
	* `#dock-to-right` : Drag-To-Right For Quicker Dock-Positioning （快速拖拉定位 devtools 界面區位置）
	 
	* `#search-navigate-filter` : Search, Navigate And Filter + （搜索，導覽和過濾 +）
		* `#filter-by-filename` : Filter For A Script, Stylesheet Or Snippet By Filename (藉檔案名稱來過濾 Script，樣式表 或片段)
		* `#textsearch-current-file` : Text Search Within The Current File (在當前檔案中搜尋文字)
		* `#replacetext-current-file` : Replace Text Within The Current File (在當前檔案中取代文字)
		* `#textsearch-across-files` : Text Search Across All Files (在所有檔案中搜尋文字)
		* `#search-regular-expression` : Search Using A Regular Expression (使用正則表達式搜尋)
		* `#filter-function-within-file` : Filter For A Function Or Selector Within A File (在一個檔案中過濾一個 function 或 selector)
		* `#search-line-number` : Jump To Line Number (跳到特定行號)
		
	* `#live-editing` : Live Editing Scripts Styles + （在線編輯腳本和樣式 +）
		* `#scripts` : Scripts (腳本)
		* `#styles` : Styles (樣式)
	* `#save-as` : Save As (另存為) 
	* `#local-modifications` : Local Modifications (本機修改) 
	* `#snippets` : Custom JavaScript Snippets + (自定義 JavaScript 代碼片段 +)
		* `#snippets-getting-started` : Getting Started (入門) 
		* `#snippets-create` : Creating Snippets (創建片段) 
		* `#snippets-filenames` : Snippet Filenames (片段檔名) 
		* `#snippets-editing` : Editing And Executing Snippets (編輯和執行片段) 
		* `#snippets-local-modifications` : Local Modifications (本機修改)
		* `#snippets-more` : Breakpoints, Watch Expressions And More (斷點，觀看表達式和更多)
		* `#snippets-saving` : Saving Snippets (保存片段)
		* `#snippets-navigation` : Navigating Snippets (片段導引)
	* `#resources` : Resources （資源）
	 
  
3. ####Using the Console (使用控制台)   
<https://developer.chrome.com/devtools/docs/console>  
	* Basic operation +	(基本操作 +)
		* Opening the Console	打開控制台 
		* Clearing the console history	清除控制台歷史記錄 
		* Console settings	控制台設置
	
	* Using the Console API +	(使用控制台 API +)
		* Writing to the console	(寫入到控制台)
		* Errors and warnings	(錯誤和警告 	)
		* Assertions	(斷言) 	
		* Filtering console output	(過濾控制台輸出 )
		* Grouping output	(編組輸出) 	
		* String substitution and formatting	(字符串替換和格式 )
		* Formatting DOM elements as JavaScript objects (格式化 DOM 元素作為JavaScript 對象) 	
		* Styling console output with CSS	(使用CSS樣式控制台輸出 )
		* Measuring how long something takes	(測量的東西需要多長時間 )
		* Marking the Timeline	(時間軸標記) 	
		* Setting breakpoints in JavaScript	(在 JavaScript 中設置斷點)
		
	* Using the Command Line API +	(使用命令行 API +)
		* Evaluating expressions	(計算表達式) 
		* Selecting elements	(選擇元素) 
		* Inspecting DOM elements and JavaScript heap objects (檢測 DOM 元素和JavaScript 對象堆) 
		* Accessing recently selected elements and objects (最近訪問選定的元素和對象 )
		* Monitoring events	(監控事件) 
		* Controlling the CPU profiler	(控制CPU分析器)
    
4. ####Tips & Tricks (提示和技巧)
<https://developer.chrome.com/devtools/docs/tips-and-tricks>  
	* Console +	(主機 +) 	
		* Write multi-line commands (寫多行命令)
		* A shortcut to launch in inspect-element mode (快捷方式啟動的檢查元素模式)
		* Support for the console.table command(支持的console.table命令 )
		* Preview logged console objects (預覽登錄控制台對象 )
		* Pass multiple arguments to styled console logs (傳遞多個參數來稱呼控制台日誌) 
		* Shortcut to clear the console history	(快捷清除控制台歷史記錄) 
		* Become a keyboard ninja	(成為一名忍者鍵盤 )
		* Accessing elements from the console	(從控制台訪問元素 )
		* Querying the DOM using XPath expressions (查詢使用XPath表達式中的DOM )
		* Access the last console result (訪問的最後一個控制台結果 )
		* Using console.dir	(使用console.dir) 
		* Running the JS console in a specific iframe(在一個特定的 IFRAME 運行 JS 控制台)
		* Stop the console clearing when navigating to a new page (導航到一個新的頁面時停止控制台結算) 
		* Benchmark loops using console.time() and console.timeEnd() (使用console.time（）和console.timeEnd基準環路（）) 
		* Profiling with console.profile() and console.profileEnd() (與console.profile（）和console.profileEnd分析（）) 
		
	* Timeline + (時間軸+)
		* Frame profiling with Timeline Frames Mode (框架分析與時間軸幀模式 )
		* Spot forced layout events using warnings (使用警告現貨強迫佈局事件 )
		* Share and analyze a Timeline recorded by someone else (分享和分析記錄由別人時間軸)
		* Annotating Timeline Recordings	(註解時間軸錄音)
		* FPS Counter/HUD Display	(FPS 計數器/ HUD 顯示)
		
	* Profiles + (型材 +) 	
		* Finding JavaScript memory leaks with the '3 snapshot' technique (找到 JavaScript 的內存洩漏與“3 快照”技術)
		* Understanding Nodes In The Heap Profiler (了解節點堆中探查 )
		* Understanding time spent in CPU profiler (在CPU分析器花時間了解) 
		* Additional insights with Heap profiler views (與堆分析器視圖的附加見解)
	
	* Sources +	來源+ 	
		* Debug on DOM modifications	(調試DOM的修改) 
		* Tracking uncaught exceptions	(追踪未捕獲的異常)
		* Conditional breakpoint actions with console.log (用的console.log 條件斷點行動)
		* Pretty Print JavaScript (漂亮的打印JavaScript的 )
		* ‘Favorite’ an expression or variable value (“鍾情”的表達式或變量值) 
		* Get insights into internal properties (找見解的內部屬性 )
		* Easily debug XHRs	(輕鬆地調試XHRs )
		* Retrieve event handlers registered on elements (檢索登記的元素的事件處理程序) 
		* Escape to see the console	 (逃到看到控制台 )
		* Become more effective when paused at a breakpoint (當在斷點處停下變得更有效 )
		* Pause on exceptions (暫停例外 )
		* Text search across all scripts (在所有的腳本文本搜索 )
		* Debugging CoffeeScript With DevTools And Source Maps	偵錯的CoffeeScript隨著DevTools和源地圖 
	* Elements +	(元素+) 	
		* Enable rulers	(使統治者) 
		* Autocompletion of CSS properties	(CSS屬性的自動完成功能) 
		* Color picker in the DevTools	(拾色器中DevTools) 
		* Adding new CSS styles	 (添加新的CSS樣式 )
		* Drag and drop in the Elements panel	(拖放的元素面板 )
		* Force Element State	(力元狀態 )
		* Writing and debugging Sass with the DevTools (編寫和調試薩斯與 DevTools)
	* Network +	(網絡+)
		* Replay XHRs	(重播XHRs )
		* Clear the network cache or cookies	(清除網絡緩存或餅乾) 
		* Record a trace export the waterfall	(記錄跟踪出口瀑布 )
		* Using large resource rows in network timeline for more detail	(使用網絡的時間線大資源的行詳細 )
		* Tricks for getting more information out of the Network panel	(技巧獲取更多信息在網絡面板 )
		* WebSocket inspection	(WebSocket的檢查 )
		* Find and filter XHRs from the Network panel	(查找並從網絡板式過濾器XHRs )
		* Get a dump of the network stack’s internal state	(找網絡協議棧的內部狀態轉儲 )
	* Settings +	(設置+) 	
		* Emulating touch events (模擬觸摸事件)
		* Emulating UA Strings Device Viewports(模擬UA字符串設備視口)
		* Emulating Geolocation	(仿真地理位置)
		* Dock-to-right for viewport debugging	(碼頭到右的視口調試 )
		* Disable JavaScript	(禁用JavaScript的 )
	* General + (常規 +	)
		* Quickly switch between tabs	(標籤之間快速切換 )
		* Get an improved dock-to-right experience (得到改進的碼頭向右體驗)
		* Use ‘Disable Cache’ for cache invalidation (使用“禁用緩存”的緩存失效)
		* Inspect Shadow DOM (檢查DOM的影子) 
		* Preview all inspectable pages (預覽全部網頁視察)
		* Get insights into which sites have appcache'd (進入該網站已經appcache'd見解)
		* Select multiple filters in the Network/Console panels (在網絡/控制台面板中選擇多個過濾器)
		* Clear the cache and perform a hard reload (清除緩存並執行硬重裝 )
		* Insights with the Chrome Task Manager (洞察與 Chrome 瀏覽器任務管理器 )
	* Additional Tools + (附加工具+	)
		* Debugging iOS apps in DevTools (偵錯中DevTools iOS應用)
		* JSRunTime: DevTools Extension For Grepping JavaScript Objects		(JSRunTime：DevTools 擴展為中用grep命令搜索JavaScript的對象)
	
    
5. ####Additional Resources (其他資源)
	5.1. **Videos 影片**  
	<https://developer.chrome.com/devtools/docs/videos>  
	5.2. **Blog Posts (部落格文章)**  
	<https://developer.chrome.com/devtools/docs/blog-posts>  
	5.3. **Mailing List (郵件列表)**  
	<https://groups.google.com/forum/?fromgroups#!forum/google-chrome-developer-tools>  
	5.4. **Contributing to DevTools (促進 DevTools)**  
	<https://developer.chrome.com/devtools/docs/contributing>  
  
##Use Tools	使用工具 

1. ####Inspecting & Tweaking (檢閱 ＆ 調整優化)  
<https://developer.chrome.com/devtools/docs/dom-and-styles>

  
2. ####Debugging JavaScript (JavaScript 的除錯)
<https://developer.chrome.com/devtools/docs/javascript-debugging>
  
3. ####Device Mode & Mobile Emulation (設備模式和模擬移動裝置)  
<https://developer.chrome.com/devtools/docs/device-mode>

4. ####Remote Debugging on Android	 (Android 上的遠端除錯)
<https://developer.chrome.com/devtools/docs/remote-debugging>
  
5. ####Saving Changes with Workspaces (在工作區保存所做的更改) 
<https://developer.chrome.com/devtools/docs/workspaces>  

##Performance & Profiling	性能與剖析 
1. ####Evaluating Network Performance (評估網絡性能)
<https://developer.chrome.com/devtools/docs/network>
     
2. ####Using the Timeline (使用時間軸)
<https://developer.chrome.com/devtools/docs/timeline>
     
3. ####Timeline Demo: Layout Thrashing	 (時間線演示：佈局抖動)  
<https://developer.chrome.com/devtools/docs/demos/too-much-layout/index>
  
4. ####Profiling JavaScript Performance (剖析 JavaScript 性能)  
<https://developer.chrome.com/multidevice/webview/workflow>
5. ####JavaScript Memory Profiling (JavaScript 的內存分析) 
<https://developer.chrome.com/devtools/docs/javascript-memory-profiling>
  
  
  
##Reference	參考資料

1. ####Console API Reference (控制台 API 參考)  
<https://developer.chrome.com/devtools/docs/console-api>

2. ####Command Line API Reference (命令列 API 參考)  
<https://developer.chrome.com/devtools/docs/commandline-api>
  
3. ####DevTools Extensions API (DevTools 擴展的 API)  
<https://developer.chrome.com/devtools/docs/integrating>
  
4. ####Keyboard Shortcuts (鍵盤快捷鍵)  
<https://developer.chrome.com/devtools/docs/shortcuts>
  
5. ####Settings (設置)  
<https://developer.chrome.com/devtools/docs/settings>
  
6. ####Remote Debugging Protocol (遠端偵錯通訊協定)  
<https://developer.chrome.com/devtools/docs/debugger-protocol>
