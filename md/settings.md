Settings
==

 **

*   [General](#general)

        *   [Appearance](#appearance)
    *   [Elements](#elements)
    *   [Rendering](#rendering)
    *   [Sources](#sources)
    *   [Profiler](#profiler)
    *   [Console](#console)
    *   [Extensions](#extensions)
*   [Workspace](#workspace)

      <!--*   [Port Forwarding](#port-forwarding)
 -->*   [Experiments](#experiments)
  **

    **To modify the Settings in DevTools:**

*   Click on the Settings cog ![](settings-files/settings-icon.png) and open up the General Settings pane to make changes. Alternatively, you can use the shortcut <span class="kbd">?</span> to open to Settings pane too.

  <div class="screenshot">
    ![](settings-files/general-settings.png)
  </div>

## General

#### Disable cache

Prevents the caching of resources **ONLY** for pages which have DevTools open. This will not disable caching if the DevTools are closed.

#### Disable JavaScript

Immediately suspends all JavaScript from running on the tab running the instance of DevTools which has this checked.

**Note:** Both the Disable cache and Disable JavaScript settings will **ONLY** apply when DevTools is open. When it is open, for the page which has the DevTools open.

#### Enable Ctrl + 1-9 shortcuts to switch panels

You are able to use <span class="kbd">Ctrl</span> + <span class="kbd">1-9</span> shortcuts to jump to specific tabs in Chrome when multiple tabs are open, with tabs 1-8 and 9 being the last tab if more than 9 are open. This setting will enable DevTools to function in the same way, so you are able to switch between panels quickly.

**Note:** Enabling this may cause conflicts with shortcut keys that are used by other applications.

### Appearance

#### Split panels vertically when docked to right

Enabling this will change the layout of the panels so the main section is stacked on top of the sidebar sections. You may find this useful when there is not enough horizontal space when they are side-by-side which is the case for smaller screens.

  <div class="screenshot">
    ![](settings-files/dock-to-right.png)
  </div>

### Elements

#### Color format

*   As authored - **(how the code was written in the stylesheet)**
*   HEX: `#DAC0DE`
*   RGB: `rgb(128, 255, 255)`
*   HSL: `hsl(300, 80%, 90%)`

  <div class="screenshot">
    ![](settings-files/color-format-settings.png)
  </div>

The **color format** setting lets you control how the color codes are displayed in the **Styles Sidebar** of the Elements panel. In addition to the Settings option for controlling the color format, you can click on the gear icon at the top of the Styles Sidebar to change the format of color codes.

  <div class="screenshot">
    ![](settings-files/color-picker-format.png)
  </div>

Selecting **As authored** will use the color format for properties as defined in the stylesheet.

#### Show user agent styles

You may find it useful to show **user agent styles** in the Styles Sidebar of the Elements panel.

  <div class="screenshot">
    ![](settings-files/show-user-agent-styles.png)
  </div>

The **user agent** refers to the browser. Each browser implements a default stylesheet consisting of basic style rules which are applied to DOM Elements in a page. If you've ever had a difficult time removing white-space between two elements, for example, it may have been because of the user agent stylesheet adding a default margin or padding to that particular type of element.

#### Word wrap

As with any text editor you can choose to wrap long lines of code in the Elements panel.

#### Show Shadow DOM

With [Shadow DOM](http://www.html5rocks.com/en/tutorials/webcomponents/shadowdom/), elements can get a new kind of node associated with them. This new kind of node is called a **shadow root**. An element that has a shadow root associated with it is called a shadow host. The child nodes of a shadow host isn't rendered; the content of the shadow root is rendered instead.

  <div class="screenshot">
    ![](settings-files/show-shadow-dom.png)
  </div>

*   [Shadow DOM 201](http://www.html5rocks.com/en/tutorials/webcomponents/shadowdom-201/)
*   [Shadow DOM 301](http://www.html5rocks.com/en/tutorials/webcomponents/shadowdom-301/)
*   [Shadow DOM Polyfill](http://www.polymer-project.org/platform/shadow-dom.html)
*   [Visualizing Shadow DOM Concepts](http://updates.html5rocks.com/2013/03/Visualizing-Shadow-DOM-Concepts)
*   [Define presentational boxes using shadow DOM](http://blogs.adobe.com/webplatform/2013/04/03/defining-presentational-boxes-with-shadow-dom/)

#### Show rulers

This will display a ruler overlay along the top, left and bottom sides of the viewport.

  <div class="screenshot">
    ![](settings-files/show-rulers.png)
  </div>

### Sources

#### Search in content scripts

[Content scripts](http://developer.chrome.com/extensions/content_scripts.html) are JavaScript files contained within Chrome extensions that run in the context of web pages but from a protected scope that is completely separate from ordinary page JavaScript. As such, content scripts and page scripts cannot interact with each other in an ordinary way.

  <div class="screenshot">
    ![](settings-files/search-content-scripts.png)
  </div>

When looking at the **Content Scripts** tab in the Sources panel you will see both the scripts added by extensions (_or by [User Scripts](http://www.chromium.org/developers/design-documents/user-scripts) which are compiled into extensions in Chrome_), and also content scripts that are built-in parts of the browser, specifically the API that extensions can use.

**Note:** When developing Chrome apps or extensions enable this setting so you can search within these native API scripts, otherwise it is not useful having it enabled.

#### Enable JS source maps

If you have code which is concatenated and minified it is difficult to tell what file a piece of code may be in when you need to debug it. Enabling this setting is useful for [debugging JavaScript](https://developers.google.com/chrome-developer-tools/docs/javascript-debugging#source-maps) and [working with Source maps](http://www.youtube.com/watch?v=HijZNR6kc9A#t=1m32s) in general.

  <div class="screenshot">
    ![](settings-files/js-source-maps.png)
  </div>

#### Enable CSS source maps

Enables Source Maps for CSS files generated using a preprocessor (for example, Sass).

For details see [Working with CSS Preprocessors](/chrome-developer-tools/docs/css-preprocessors).

#### Auto-reload generated CSS

Only used if **Enable CSS source maps** is turned on. Determines whether generated CSS files should be reloaded when the source file is saved.

#### Detect Indentation

Specifies how code is indented when editing in DevTools:

*   2 spaces
*   4 spaces
*   8 spaces
*   Tab character

#### Show whitespace characters

This will show dots for spaces and tab characters in the Sources panel.

### Profiler

#### High resolution CPU profiling

Enables you to zoom into the graph to view it by a tenth of a millisecond on Flame charts.

### Console

#### Log XMLHttpRequests

Display XHR request objects in the console which expand to show details of the request.

#### Preserve log upon navigation

When navigating through multiple pages of a site you may choose to not clear the console logs with each page load so you can view the history of output across pages.

**Note:** Both of these settings can be changed by right clicking on the console.

  <div class="screenshot">
    ![](settings-files/console-right-click.png)
  </div>

### Extensions

  **Open links in:** `a panel chosen automatically`

## Workspace

[Workspaces](http://www.youtube.com/watch?v=bqfoYaKCYUI#t=3m39s) allow you to select **custom directories** in your file system which are always available for you to edit within the **Sources panel**. This can be one specific project directory or a directory that contains multiple different projects within it.

To use this feature it open the **Workspaces tab** via the Settings pane. Here you will see an **Add Folder** link allowing you to add local directories for editing (eg: project root directories).

  <div class="screenshot">
    ![](settings-files/workspace.png)
  </div>

Once you've added a folder directory, you will be able to view, edit and save changes to files within it anytime you are working in the Sources panel. All changes to files will persist down to local file contained in directory.

In addition to adding a file system for your Workspace, you are also able to add file **Mappings** individual file urls to the path of the file on your local machine.