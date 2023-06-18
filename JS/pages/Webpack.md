- [Webpack](https://webpack.js.org/)
- Bundles assets and content for better serving website to client browsers.
- They do something like so
  
  * Take a “main” module, the one intended to be put in `<script type="module">` in HTML.
  * Analyze its dependencies: imports and then imports of imports etc.
  * Build a single file with all modules (or multiple files, that’s tunable), replacing native `import` calls with bundler functions, so that it works. “Special” module types like HTML/CSS modules are also supported.
  * In the process, other transformations and optimizations may be applied like
  Unreachable code removed.
  Unused exports removed (“tree-shaking”).
  Development-specific statements like `console` and `debugger` removed.
  Modern, bleeding-edge JavaScript syntax may be transformed to older one with similar functionality using [Babel](https://babeljs.io/).
  The resulting file is minified (spaces removed, variables replaced with shorter names, etc).
  
  So we get something like this
  ```html
  <!-- Assuming we got bundle.js from a tool like Webpack -->
  <script src="bundle.js"></script>
  ```
  
  The generated bundle.js has all the [[Module]]s and [[Optimization]]s and can be directly used without the module type in the [[<script>]].
-