- The language understood by browsers natively.
- Most HTML documents can be created dynamically.
  For ex.:
  ```js
  function loadScript(src) {
    // creates a <script> tag and append it to the page
    // this causes the script with given src to start loading and run when complete
    let script = document.createElement('script');
    script.src = src;
    document.head.append(script);
  }
  ```
  This [[Function]] appends a [[<script>]] tag to the top of an ``HTML Document`` and assigns its src attribute the value given. This way we can dynamically add JS scripts to our Document.