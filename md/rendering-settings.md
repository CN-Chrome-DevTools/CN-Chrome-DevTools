Rendering Settings
==
  

To access the Rendering Settings click on the drawer icon. You can press <span class="kbd">Esc</span> as a shortcut to open the drawer.

  <div class="screenshot">
    ![](rendering-settings-files/rendering-settings.png)
  </div>

#### Show paint rectangles

    When this is enabled you be able to see the regions where Chrome paints. This can help diagnose and ultimately [avoid unnecessary paints](http://www.html5rocks.com/en/tutorials/speed/unnecessary-paints/) on a page. You can also use this to [study painting behaviors](http://www.paulirish.com/2011/viewing-chromes-paint-cycle/) just by hovering over links, popups, or some content which dynamically updates.

  <div class="screenshot">
    ![](rendering-settings-files/show-paint-rects.png)
  </div>

Furthermore, showing paint rectangles shows repainted areas for each frame making it easy to visualize what slows pages down. Ideally you want to keep the number of areas being repainted as low as possible.

#### Show composited layer borders

This highlights where layers are on-screen. Use this to [accelerate render times](http://www.html5rocks.com/en/tutorials/speed/layers/) in Chrome. It can help to reduce the time it takes to render elements which may be animating or have CSS transforms/transitions applied to them that change the shape or positioning of the element.

**Note:** The blue grid represents tiles, which you can think of as sub-units of a layer that Chrome uses to upload parts of a large layer at a time to the GPU. They aren't really too important in most cases.

  <div class="screenshot">
    ![](rendering-settings-files/composited-layer-borders.png)
  </div>

Enabling [Show paint rectangles and Show layer borders](http://www.youtube.com/watch?v=x6qe_kVaBpg&t=23m42s) together can also be useful for finding any unnecessary paints which occur and cause a relayout on the Timeline panel.

**Note:** Show paint rectangles and composited layer borders settings will only apply when DevTools is open.

#### Show FPS meter

Overlays an [FPS meter](http://www.youtube.com/watch?v=x6qe_kVaBpg&t=23m17s) at the top right of the browser window which updates in real-time.

  <div class="screenshot">
    ![](rendering-settings-files/fps-meter.png)
    ![](rendering-settings-files/fps-memory-meter.png)
  </div>

When running Chrome on Android you may also **show a GPU memory meter** as part of the overlay. Enabling the FPS meter is specially helpful when developing pages that have animations.

#### Enable continuous page repainting

This is a tool that helps you [identify elements](http://www.html5rocks.com/en/mobile/profiling/) on the page which are costly. The page will [continuously repaint](http://www.youtube.com/watch?v=x6qe_kVaBpg&t=22m51s), showing a counter of [how much painting work is happening](http://updates.html5rocks.com/2013/02/Profiling-Long-Paint-Times-with-DevTools-Continuous-Painting-Mode). Then, you can hide elements and mutate styles, watching the counter, in order to figure out what is slow.

  <div class="screenshot">
    ![](rendering-settings-files/continuous-page-repainting.png)
  </div>

#### Show potential scroll bottlenecks

Shows area of the page that slow down scrolling. Touch and mousewheel event listeners can delay scrolling. Some areas need to repaint their content when scrolled. [Additional details on causes in this ticket.](https://code.google.com/p/chromium/issues/detail?id=253552#c13)