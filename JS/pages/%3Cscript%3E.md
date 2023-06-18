- This [[HTML]] tag allows importing JS code into the HTML document, which is how the page can then execute the code.
- ``Async`` script
  If a script tag uses this attribute then it immediately executes the script even if the [[HTML]] page isn't loaded yet. However the script must be independent of the rest of the scripts and the page.
  This only works for external scripts, meaning scripts that are imported from an src attribute
  or if the script is a [[Module]]. In the latter case, if the script imports other modules then it waits until they are loaded so if they are not async then it is basically [[Defer]]red.
  For ex.:
  ```html
  <script async type="module">
    import {counter} from './analytics.js';
  
    counter.count();
  </script>
  ```
  This inline script is ran as soon as ``./analytics.js`` is ready.\
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