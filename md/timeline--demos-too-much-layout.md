Performance profiling with the Timeline
==
 

This demo shows how you can use the Timeline to identify a kind of performance bottle-neck called 'forced synchronous layouts'. The demo application animates several images back and forth using `[requestAnimationFrame()](http://docs.webplatform.org/wiki/apis/timing/methods/requestAnimationFrame)`, the [recommended approach](http://updates.html5rocks.com/2012/05/requestAnimationFrame-API-now-with-sub-millisecond-precision) for performing frame-based animation. But there's a considerable amount of stuttering and jank as the animation runs We'll use the Timeline to diagnose what's going on.

For more information about using Frames mode and forced asynchronous layouts see [Locating forced synchronous layouts](/chrome-developer-tools/docs/using-timeline#locating_forced_synchronous_layouts) in [Using the Timeline](/chrome-developer-tools/docs/using-timeline).

## 
        Make a recording

        First you'll make a recording of the animation.

1.  Click **Start** to start the animation.
2.  Open the Timeline panel on this page, and go the Frames view.
3.  Click the Record button in the Timeline.
4.  After after a second or two (10-12 frames recorded) stop the recording and click **Stop** to stop the animation.

<!--     <section class="expandable">
         <button type="button" class="button-red button expand-control" id="show-hide-1">Show/Hide Demo</button>
 -->

        <iframe
  data-src="/chrome-developer-tools/docs/demos/too-much-layout/index_727b3f098a4c7c5f99809a9e55fde44a.frame"
  class="framebox inherit-locale "
  style="width: 100%; height: 600px;">

[This section requires a browser that supports JavaScript and iframes.]

</iframe>    

## Analyze the recording

Looking at the recording of the first few frames it's clear that each one is taking over 300ms to complete. If you hover your mouse over one of the frames a pop-up appears showing additional details about the frame.

<div class="screenshot"> ![](images/frame-rate.png)</div>

Locate an Animation Frame Fired record and notice the yellow warning icon next to it, indicating a forced synchronous layout. The icon is slightly dimmed indicating that one of its child records contains the offending code, rather than this record itself. Expand the Animation Frame Fired to view its children.

<div class="screenshot">![](images/recording-1.png)</div>

The child records show a long, repeating pattern of **Recalculate Style** and **Layout** records. Each layout record is a result of the style recalculation that, in turn, is a result of the `requestAnimationFrame()` handler requesting the value of `offsetTop` for each image on the page. Hover your mouse over one of the Layout records and click the link for sources.js next to the Layout Forced property.

<div class="screenshot">![](images/layout-warning-hover.png)</div>

The Sources panel opens at line 43 of the source file at the `update()` function, which is the `requestAnimationCallback()` callback handler. The handler computes the image's `left` CSS style proeprty on the the image's `offsetTop` value. This forces Chrome to perform a new layout immediately to make sure it provides the correct value.

	// Animation loop
	function update(timestamp) {
	    for(var m = 0; m < movers.length; m++) {
	        movers[m].style.left = ((Math.sin(movers[m].offsetTop + timestamp/1000)+1) * 500) + 'px';
	        }
	    raf = window.requestAnimationFrame(update);
	};

We know that forcing a page layout during every animation frame is slowing things down. Now we can try to fix the problem directly in DevTools.

## Apply fix within DevTools

Now that we have an idea what's causing the performance issues, we can modify the JavaScript file directly in the Sources panel and test our changes right away.

1.  In the Sources panel that was opened previously, replace line 43 with the following code.

            `<pre>movers[m].style.left = ((Math.sin(m + timestamp/1000)+1) * 500) + 'px';</pre>`

This version computes each image's `left` style property on its index in its holding array instead of on a layout-dependent property (`offsetWidth`).2.  Save your changes by pressing Cmd-S or Ctrl-S.

## Verify with another recording

The animation is clearly faster and smoother than before, but it's always good practice to measure the difference with another recording. It should look something like the recording below.

        <div class="screenshot">![](images/fixed.png)</div>

  </div>

  