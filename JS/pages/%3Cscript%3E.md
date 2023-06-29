- This [[HTML]] tag allows importing JS code into the HTML document, which is how the page can then execute the code.
- ``defer`` script
  [[Defer]]rs the script loading.
  The script is loaded in the background and the rest of the [[HTML]] document doesn't wait for it. Then when the DOM is built and ready, just before the ``DOMContentLoaded`` [[Browser Event]], the deferred scripts are executed. The event waits for these scripts to finish loading and executing and then finally triggers.
  
  That is, the order of loading and execution is
  [[HTML]] Document is loaded
  DOM is loaded
  Deferred Scripts are loaded parallely to each other, but executed in their relative order on the Document.
  ``DOMContentLoaded`` event is triggered
  
  For ex.:
  ```html
  <script defer src="https://javascript.info/article/script-async-defer/long.js"></script>
  ```
  This attribute is only valid on external scripts.
- ``Async`` script
  If a script tag uses this attribute then its execution is independent from the rest of the [[HTML]] page , it can even be loaded and executed before the page is fully loaded. 
  
  The script must be independent of the rest of the scripts and the page.
  
  This only works for external scripts, meaning scripts that are imported from an src attribute
  or if the script is a [[Module]]. In the latter case, if the script imports other modules then it waits until they are loaded so if they are not async then it is basically [[Defer]]red.
  For ex.:
  ```html
  <script async type="module">
    import {counter} from './analytics.js';
  
    counter.count();
  </script>
  ```
  This inline script is ran as soon as ``./analytics.js`` is ready.
  
  So async is like the defer attribute but the ``DOMContentLoaded`` event is independent to it as well. 
  
  Dynamically added scripts (scripts that are added to the [[DOM]] from the JS) behave like async and defer by default.
-
- ``nomodule``
  Older [[Browser]]s that  don't understand ``type= module`` for [[<script>]] ignore the script, but they accept this attribute and so they execute the script. Modern browsers ignore this script.
  For ex.:
  ```html
  <script type="module">
  //ignored by old browsers
  </script>
  <script nomodule>
  //ignored by new browsers
  </script>
  
  ```
- ``crossorigin``
  This attribute is necessary for cross-origin access/[[CORS]], that is when the external script being loaded is on another domain.