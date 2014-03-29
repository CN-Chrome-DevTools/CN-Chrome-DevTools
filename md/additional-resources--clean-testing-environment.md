Creating A Clean Testing Environment
==

A clean testing environment allows you to to easily determine the point of 
failure for your site or web application, rather than wondering if it was 
something in the background that caused your slow-down or failure.

Apps, extensions, background processes and unnecessary tabs are only some of the 
things that can impact your profiling figures and should be avoided where 
possible. This is of particular relevance when you wish to accurately profile 
using the [Timeline](/chrome-developer-tools/docs/timeline), [Profiles](/chrome-developer-tools/docs/cpu-profiling) or `about:tracing`.

**To set up a clean testing environment:**

<ul>
<li>
Ensure Chrome is being run with a clean profile:

*   Option 1: Use Incognito mode. Open the **Chrome menu** ![Chrome menu](/chrome-developer-tools/images/chrome-menu.png) and select **New Incognito Window**, or launch       Chrome with the `--incognito` switch.

    ![](clean-testing-environment/incognito.png)

*   Option 2: Use a fresh Chrome profile. Click the user icon in the top-right corner	of your Chrome window and select **New User**.

    ![](clean-testing-environment/screenprofile.jpg)

    &nbsp;

    To guarantee a clean profile that doesn't inherit       any extensions or settings, launch Chrome using the `--user-data-dir`       switch with the path `'/dev/null'`.  e.g:

        *   OSX:    `open -a "Google Chrome" --args --user-data-dir=/dev/null`
    *   Linux:  `google-chrome --user-data-dir=/dev/null`
    *   Windows: `"C:\Users\username\AppData\Local\Google\Chrome\Application\chrome.exe"          --user-data-dir=/dev/null`

This will ensure you get a completely empty profile instead of loading the user's profile as read-only.

*   Close other tabs and processes/applications you may have running in the background. If these apps need to run in the background, use your system monitor to ensure apps are not stealing too much CPU or RAM. ![](clean-testing-environment/activity.png)
*   Disable any Dynamic CPU overclocking you may have enabled on your system.
*   Make sure you are up to date with the latest builds of Chrome. You can use [http://download-chromium.appspot.com](http://download-chromium.appspot.com) to grab edge builds.
*   If you are running into a bug and generally use Chrome Canary as your  development browser, it is worth testing in Chrome Stable to ensure the issue  is unrelated to new features or bugs.