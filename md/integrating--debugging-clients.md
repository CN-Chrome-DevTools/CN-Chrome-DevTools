Sample Debugging Protocol Clients
==

There are a number third-party clients for the Chrome debugging protocol. This section presents a sample.

#### Contents

<nav class="inline-toc">

1.  [Brackets](#brackets)
2.  [Light Table](#light_table)
3.  [NodeJS](#nodejs)

        1.  [chrome-remote-interface](#chrome-remote-interface)
    2.  [crconsole](#crconsole)
4.  [Sublime Text](#sublime_text)
5.  [Telemetry](#telemetry)
6.  [Vim](#vim)
7.  [WebDriver](#webdriver)
8.  [WebStorm](#webstorm)</nav>

## Brackets

Brackets is a web-based IDE that uses the Chrome debugging protocol to enable 
debugging and live HTML/CSS development.

![](debugging-clients-files/brackets.png) 

More information:

*   [Official site](http://brackets.io/).
*   This [blog post from Mark  DuBois](http://www.markdubois.info/weblog/2013/03/adobe-brackets-revisited/)  gives an overview of working in Brackets.
*   Download from [download.brackets.io](http://download.brackets.io/).
*   Source code available on [GitHub](https://github.com/adobe/brackets).

## Light Table

Light Table is a new IDE that takes a novel approach to arranging the 
developer's workspace. Light Table is currently in alpha. It's not open source, 
but the alpha version is available for free at this time.

![](debugging-clients-files/lighttable.png) 

More information:

*   Read the [blog post](http://www.chris-granger.com/2013/04/28/light-table-040/)  describing new features in 0.4.0, including DevTools integration.
*   Download from the [official site.](http://www.lighttable.com/)

## NodeJS

A number of modules have been developed to make use of the Chrome debugger from 
Node scripts. 

### chrome-remote-interface

The `chrome-remote-interface` module wraps the debugger protocol with a Node-style 
JavaScript API.

![](debugging-clients-files/chrome-remote.png) 

More information:

*   Install using `npm`:

        npm install -g chrome-remote-interface
    `</pre>
*   Source code available on        [GitHub.](https://github.com/cyrus-and/chrome-remote-interface)

    ### crconsole

    The `crconsole` module provides a command-line interface to the Chrome console. It 
    uses the `chrome-remote-interface` module to communicate with the Chrome debugger 
    protocol.

    More information:

*   Install using `npm`:
    <pre class="prettyprint notranslate" translate="no">`npm install -g crconsole

*   Source code available on [GitHub](https://github.com/sidorares/crconsole).

## Sublime Text

The Sublime Web Inspector project adds Chrome debugger integration to the 
popular Sublime Text editor. You can install it from the Sublime Text package 
manager.

![](debugging-clients-files/sublime.png) 

More information:

*   See the [official page](http://sokolovstas.github.io/SublimeWebInspector/) for  an overview and installation instructions.
*   Source code available on  [GitHub](https://github.com/sokolovstas/SublimeWebInspector).

## Telemetry

Telemetry is a performance testing framework used by the Chromium project to 
test multiple versions of the Chrome browser. It uses the debugging protocol to 
remotely control instances of Chrome.

More information:

*   [Introduction to Telemetry on  Chromium.org.](http://www.chromium.org/developers/telemetry)

## Vim

Chrome.vim is an experimental plugin for the Vim editor that provides some basic 
Chrome operations as Vim commands.

More information:

*   [https://github.com/mklabs/vimfiles/tree/master/custom-bundle/vim-chrome](https://github.com/mklabs/vimfiles/tree/master/custom-bundle/vim-chrome)

## WebDriver

The Selenium browser automation tools use WebDriver API to abstract interactions 
with different browsers. The WebDriver implementation for Chrome uses the Chrome 
debugging protocol.

More information:

*   [Selenium WebDriver project.](http://docs.seleniumhq.org/projects/webdriver/)

If you know of more, please let us know using the Feedback tool at the top right 
of this page!

## WebStorm

WebStorm is a commercial IDE that supports debugging and live-editing in Chrome. 
WebStorm uses a [Chrome extension 
](http://www.jetbrains.com/webstorm/webhelp/using-jetbrains-chrome-extension.html)to 
integrate with the Chrome debugger.

![](debugging-clients-files/webstorm.png) 

More information:

*   Download from [JetBrains](http://www.jetbrains.com/webstorm/).
*   [Screencast describing the latest debugging  features.](http://tv.jetbrains.net/videocontent/improved-javascript-debugger-in-webstorm-7)