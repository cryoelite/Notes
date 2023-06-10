alias:: JavaScript

- Programs in [[JavaScript]] are called "[[<script>]]s", they can be written directly in webpages and almost all modern web browsers can execute them. All that is required to run [[JavaScript]] code is a JavaScript Engine, which is [V8](https://v8.dev/) in Chrome/Opera/Edge and Spidermonkey in Firefox.
  
  In browsers, the “script” is parsed then compiled into machine code and executed. It is
  heavily optimized.
  
  It is a pretty “safe” language, as it doesn’t have low-level access. Still, its capabilities vary depending on the environment executing it, for browsers JS can manipulate the webpage, interact with web servers, get set cookies, remember “user data” etc. On servers (like in Node.js), it can do other things like File I/O etc.
  That said, JS on the browser has many limitations imposed to enforce security, such as not being
  able to see contents of another tab in the browser, no access to OS, strict
  browser managed access to peripherals, not being able to connect to other
  domains unless explicitly allowed by both domains, etc. These are not present
  in JS outside the scripts in webpages.
  
  Note: Generally both Web browsers and Node.JS use V8, node simply provides a large runtime library that allows the V8 to do a lot of stuff on the server side.
- Execution
  For browsers, any .html file that uses a <script> tag with inline js or external js file as
  source can execute a js file.
  For server-side, or locally, we can use node <filename.js> to execute it using Node.js.
  
  Browsers have " [[Developer tools]] " which present various developer friendly tools to inspect a page's script and behaviors.
- [[Compilation]]
- Semicolons are optional but a good practice, otherwise line break is considered the end of a statement (called implicit semicolon and the feature called [[automatic semicolon insertion]]).
  But a single line can have multiple statements with a semicolon.
  
  [[ASI]] is a bit more powerful and can understand if sometimes line breaks shouldn't be interpreted as semicolon/statement end
  For ex.:
  ```js
  alert(3 +
  1
  + 2);
  ```
  So it is recommended use semicolons everywhere except with expressions like these.
- Comments
  Same as everywhere else,
  ``//`` for single line
  ``/* */`` for multi
-
- use strict;
  Older JS standards (before ES5 in 2009) use now what's called the [[Old Mode]], but after it a non-breaking change was introduce called ``strict mode`` that complies with any new changes in the ECMA standard. This string at the top of a script or function enable strict mode. When applied globally, it is applied to the whole script, and for function it only enables it for the function. There's no way to disable it if enabled for a script.
  
  
  For ex.:
  ```js
  use strict;
  
  ```
  Recommended to enable it always.
  For console, when we need to use it we can just use 
  ```js
  use strict; (Shift + Enter)
  //...Rest of code
  
  //or if that doesn't work,
  'use strict'; (Shift + Enter)
  
  //or if that doesn't work either, this ugly hack works
  (function() {
    'use strict';
  
    // ...your code here...
  })()
  ```
- Variable
  Use ``let`` to declare a mutable variable
  ```js
  let x;
  //  or
  let y=2; //with assignment
  //or
  let x1="ay", yo=2; //multi declaration in single line
  //or
  let x2=2,
       y1= "yo";
  
  ```
  ``let`` prohibits redeclaration.
  
  Or [[var]] for the same, however [[var]] is an old way and declares variables quite differently. 
  
  Variables can be named however as long as they aren't [[Reserved Words]], can use letters(unicode)/digits/'$'/'_' and mustn't begin with digits.
  
  [[Old Mode]] allows variable declaration without a let. This is why in console we can declare variables without using let.
  
  * Const
     Non-mutable variables can be declared with const.
    For ex.:
  ```js
  const X=2; //Immutable variable x
  const Y; //can be assigned later, but only once
  
  ```
  It is recommended to use all capitals for constants that are known prior to runtime, and normal camelCase for other variables. 
  
  const on [[Object]] denies reassignment but the Object itself can mutate however, as const is applied to the variable (which stores the address of the Object) and the Object itself is free to mutate.
  For ex.:
  ```js
  const x= {
    name:2,
  };
  x["a"]=2; //works
  ```
- Data Types
  Dynamically typed, meaning variables do have types but they can change them at runtime.
  For ex.:
  
  ```js
  let x="Yo"; //string
  x=2;  //works, type is now int
  
  ```
  There are 7 [[Primitives]] and then there is [[Object]]
  
  * [[Number]]
  * [[String]]
  * [[Boolean]]
  * [[null]]
  * [[BigInt]]
  * [[undefined]]
  * [[Symbol]]
  
  * [[Object]]
- Other complex types also exist in JS:
  Collection of values: [[Array]]
  Object like Dictionary: [[Map]]
  Collection of unique values: [[Set]]
  [[Date]]
  [[JSON]]
  
  
  Weaker variants:
  [[WeakMap]]
  [[WeakSet]]
-
- [[typeOf]]
- Interaction
  
  ``Alert``: Sends a message to the browser window and waits for the user to press "OK". Doesn't return anything.
  For ex.:
  ```js
  alert("yae");
  ```
  ``prompt``: Sends a message and presents an input field, along with an "OK" and "Cancel" button.
  If user presses Ok then returns a string with the inputted value (empty string if nothing entered). If uses presses cancel or Esc then null is returned.
  Syntax:
  ``result = prompt(title, [default]);``
  For ex.:
  ```js
  let x= prompt("yo ?"); //Then we cancel
  console.log(typeof x); //Prints "null"
  ```
  Also accepts an optional parameter, a default value to return if cancelled.
  ```js
  let x=prompt("Yo ?", 2); //cancel  
  console.log(x); //prints 2
  ```
  
  ``confirm``: Sends a message and waits for "Ok" or "Cancel", returns true on the former and false for the latter.
- [[Conversion]]
- [[Operator]]
- [[Comparison]]
- [[Conditional]]
- [[Loop]]
- [[Function]]
- [[Comments]]
- [[Console]]
- [[Testing]]
- [[Transpiler]]
- [[Polyfill]]
- [[Garbage Collection]]
- [[Destructuring]]
- [[Global]] [[Object]]
- [[Scheduling]]
-