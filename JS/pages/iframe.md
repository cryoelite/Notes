- This [[HTML Element]] is a special element as it allows embedding other [[HTML]] Documents in a Document.
  
  For ex.:
  ```html
  <iframe src="https://example.com" id="iframe"></iframe>
  
  <script>
    iframe.onload = function() { //has the load event
      let iframeWindow = iframe.contentWindow; // to get the iframe's window Object
      try {
        let doc = iframe.contentDocument; //shorthand for .contentWindow.document, to get the iframe's html Document, this may give error if Same Origin is not followed
      } catch(e) {
        alert(e); // Security Error (another origin)
      }
  
      
      try {
        let href = iframe.contentWindow.location.href; // can be set not read if not Same Origin
      } catch(e) {
        alert(e); // Security Error
      }
  
      iframe.contentWindow.location = '/'; //works
  
      iframe.onload = null; // clear the handler, not to run it after the location change
    };
  </script>
  ```
  There's the ``load`` [[Browser Event]] just like popup window does and just like the popup window or normal [[window]] we can directly set the handler on ``iframe.contentWindow.load`` too but this is inaccessible if the iframe is not of same origin. The [[Same Origin]] policy is also followed by iframes.
  
  * iframe Documents are not loaded right away, the initial document is different from the final one, however the final one is there when the ``load`` event is triggered.
  
  * Navigation
  The iframe opener [[window]] can access its iframes directly with ``<window Object>.iframes`` which is a named and ordered collection. 
  The iframe can access its parent window with ``<iframe's window Object>.parent`` or ``<iframe's window Object>.top`` to get the topmost parent window directly. 
  
  * The ``sandbox`` attribute
  The attribute, if provided without any values, applies the strictest restrictions on the iframe such as not being able to have executable [[<script>]]s, forms etc.
  We can provide a space separated list of allowed properties as given on [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) such as ``sandbox="allow-scripts allow-forms"`` and so on.
-