alias:: JavaScript

- Programs in [[JavaScript]] are called "scripts", they can be written directly in webpages and almost all modern web browsers can execute it. All that is required to run [[JavaScript]] code is a JavaScript engine, which
  is V8 in Chrome/Opera/Edge and Spidermonkey in Firefox.
- In browsers, the
  “script” is parsed then compiled into machine code and executed, and it is
  heavily optimized.
-
- It is a pretty “safe”
  language, as it doesn’t have low-level access. Still, the capability of JS
  itself varies depending on the environment executing it, for browsers JS can
  manipulate the webpage, interact with web servers, get set cookies, remember
  “user data” etc. On servers (like in Node.js), it can do other things like File
  I/O etc.
-
- That said, JS on
  browser has many limitations imposed to enforce security, such as not being
  able to see contents of another tab in the browser, no access to OS, strict
  browser managed access to peripherals, not being able to connect to other
  domains unless explicitly allowed by both domains, etc. These are not present
  in JS outside the scripts in webpages.
-
- <!--[if !supportLists]-->2.   <!--[endif]-->Useful Links
- <!--[if !supportLists]-->1.   <!--[endif]-->MDN for anything JS, it is not the whole ECMA spec sheet but it is the
  closest to it with practical and concise examples.
- <!--[if !supportLists]-->2.   <!--[endif]-->[https://caniuse.com/](https://caniuse.com/) To check what feature is supported in what browser.
- <!--[if !supportLists]-->3.   <!--[endif]-->[https://kangax.github.io/compat-table](https://kangax.github.io/compat-table) Similar
-
- <!--[if !supportLists]-->3.   <!--[endif]-->Execution
- For browsers, any .html
  file that uses a <script> tag with inline js or external js file as
  source can execute a js file.
- For server-side, or
  locally, we can use node <filename.js> to execute it using Node.js.