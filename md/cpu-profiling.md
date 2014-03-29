Profiling JavaScript Performance
==
**

*   [Intro to JavaScript Profiling](#cpu-profiles)
      <li>[Using the Flame Chart](#flame-chart)
  **

## Introduction to JavaScript Profiling

Using Google Chrome, open the [V8 Benchmark Suite](http://v8.googlecode.com/svn/data/benchmarks/v7/run.html) page. Click the **Start
  profiling** button or press <span class="kbd">Cmd</span> + <span class="kbd">E</span> start recording a JavaScript CPU profile. Now refresh the V8 Benchmark Suite page.

When the page has completed reloading, a score for the benchmark tests
  is shown. Return to the DevTools and stop the recording by clicking the Stop button or by pressing <span class="kbd">Cmd</span> + <span class="kbd">E</span> again.

  <div class="screenshot">
    ![](cpu-profiling-files/heavy-bottom-up.png)
  </div>

This **Bottom Up** view lists functions by impact on performance.
    It also enables you to examine the calling paths to those functions.

Now select the **Top Down** view by clicking the Bottom Up
  / Top Down selection button. Then click the small arrow to the left of
  **(program)** in the **Function** column. The
  **Top Down** view shows an overall picture of the calling
  structure, starting at the top of the call stack.

**Note:** You can click the **Percentage** button to view absolute times.

Select one of the functions in the **Function** column,
  then click the **Focus selected function** button (the Eye
  icon on the right).

  <div class="screenshot">
    ![](cpu-profiling-files/focus-selected-function.png)
  </div>

This filters the profile to show only the selected function and its callers. Click the **Reload** button at the bottom-right of the window to restore the profile to its original state.

Select one of the functions in the **Function** column,
  then click the **Exclude selected function** button (the X
  icon). Depending on the function you selected, you should see something
  like this:

  <div class="screenshot">
    ![](cpu-profiling-files/tree-top-down.png)
  </div>

The **Exclude selected function** button removes the selected function from the profile and charges its callers with the excluded function's total time. Click the **Reload** button to restore the profile to its original state.

You can record multiple profiles. Click the **Start profiling** button, reload the V8 Benchmark page, then click the **Stop profiling** button.

  <div class="screenshot">
    ![](cpu-profiling-files/sidebar-profile-history.png)
  </div>

The sidebar on the left lists your recorded profiles, the tree view on the right shows the information gathered for the selected profile.

## Using the Flame Chart

The Flame Chart view provides a visual representation of JavaScript processing over time, similar to those found in the Timeline and Network panels. Using the **Flame Chart** feature on the Details view after performing a _JavaScript &amp; CPU profile_, you are able to view the profile data a few different ways.

### Visualizing execution paths

By analyzing and understanding function call progression visually you can gain a better understanding of the execution paths within your app.

  <div class="screenshot">
    ![](cpu-profiling-files/flamechart00.png)
  </div>

### Identifying outliers with color coding

When zoomed out you can identify repetitive patterns that could be optimized, or more importantly, you're able to spot outliers or unexpected executions much easier.

  <div class="screenshot">
    ![](cpu-profiling-files/flamechart-outliers.png)
  </div>

### Visualizing JavaScript profiler data against a time scale (like Timeline)

Other JavaScript profiling reports data that is aggregated _over time_, whereas the Flame Chart reports data _by time_. This means when you see events happen, you are able to drill into the time scale of them and really get some perspective on execution of JavaScript. For example, when you see big streaks of yellow in the Timeline, this is the perfect way to see the issue.

  <div class="screenshot">
    ![](cpu-profiling-files/flamechart02.png)
  </div>

You may even want to do a Timeline recording simultaneously while you do a recording with the JavaScript Profiler. Here’s a snippet you can use to do this:

  <pre class="prettyprint">
    `(function() {
      console.timeline();
      console.profile();
      setTimeout(function() {
          console.timelineEnd();
          console.profileEnd();
      }, 3000);
    })();`</pre>

**Note**: The horizontal axis is time and vertical axis is the call stack. Expensive functions are wide. Call stacks are represented on the Y axis, so a tall flame is not necessarily significant. Pay close attention to wide bars, no matter their position in the call stack.

### How to use the Flame Chart:

1.  Open the DevTools and go to the Profiles panel.
2.  Choose **Record JavaScript CPU profile** and click **Start**.
3.  When you are done collecting data, click **Stop**.

In the profile view, select the Flame Chart visualization from the select menu at the bottom of the DevTools.

  <div class="screenshot">
    ![](cpu-profiling-files/flamechart03.jpg)
  </div>

**Note**: For increased accuracy of profiling times enable **High resolution CPU profiling** in the DevTools flame-chart under Profiling. When enabled, you can zoom into the graph to view it by a tenth of a millisecond even.

Across the top of the panel is an overview that shows the entire recording. You can zoom in on a specific region of the overview by selecting it with your mouse as shown below, and you can also pan left and right by clicking on the white area and dragging your mouse. The Details view timescale shrinks accordingly.

  <div class="screenshot">
    ![](cpu-profiling-files/flamechart04.png)
  </div>

In the Details view, a **call stack** is represented as a stack of function "blocks". A block that sits atop another was called by the lower function block. Hovering over a given block displays its function name and timing data:

  <div class="screenshot">
    ![](cpu-profiling-files/flamechart05.jpg)
  </div>

*   **Name** — The name of the function.
*   **Self time** — How long it took to complete the current invocation of the function, including only the statements in the function itself, not including any functions that it called.
*   **Total time** — The time it took to complete the current invocation of this function and any functions that it called.
*   **Aggregated self time** — Aggregate time for all invocations of the function across the recording, not including functions called by this function.
*   **Aggregated total time** — Aggregate total time for all invocations of the function, including functions called by this function.

The colors in the Flame Chart are reused for same functions. This helps you see a common pattern and then spot outliers better. There is no correlation to colors used in the Timeline for things.

  <div class="screenshot">
    ![](cpu-profiling-files/flamechart06.png)
  </div>

Clicking a function block opens its containing JavaScript file in the Sources panel, at the line where the function is defined.