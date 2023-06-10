- This [[Object]] contains the Global variables/functions and other environment-specific values in it. It is not the same as the [[Environment Object]] or the [[Lexical Environment]].
  This object is accessible and even mutable by code, 
  In browsers, ``window`` is the name of this Object, ``global`` in node.js and most modern browsers and other environments also now support ``globalThis`` as a more standard name that doesn't need to depend on the environment.
- Variables declared with [[var]] and function declarations are directly accessible through the Global Object as they exist as a property in it.
  That is,
  ```js
  var x=2;
  console.log(global.x); //prints 2
  ```
  However, it is unrecommended to rely on this behavior and if we wish to truly have variables persist globally, we should instead set the property directly on the Global Object to set Global Variables.
- Essentially, JS built-in [[Object]]s we use are all Global Object's properties and JS simply allows syntax sugar to access them.
  For ex.:
  ```js
  alert(2);
  //is the same as
  window.alert();
  //Similarly,
  let x = new Array();
  //is the same as
  let x = new window.Array();
  ```
  
  This is why we can also test for a feature using the Global Object,
  such as
  ```js
  if (!window.Promise) {
    window.Promise = ... // custom implementation of the modern language feature
  }
  ```
-
-