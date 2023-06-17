alias:: Exception

- The same as in other languages like [[C++]].
- ``try {...} catch (err) {...} finally {...}`` to try and catch errors.
  However, in JS, syntax errors are a bit more nuanced. For ex.:
  ```js
  try{
   a=2;
  }
  catch(err) {...}  //catches this error
  ```
  Works, but other syntactical errors fail the compilation itself. Generally, only the errors at runtime are meant to be handled by try catch.
  
  * It is synchronous, so
  ```js
  try {
  setTimeout(function(){
   ...
  },100);
  }
  catch(err) {} //won't catch errors from inside setTimeout
  ```
  If the error doesn't occur whilst a try block is being executed then it is not caught in JS. So for places like these, try catch must be in the same execution context.
  * ``catch(...)`` can omit ``(...)``.
  * ``finally`` is **always** ran. 
  For ex.:
  ```js
  try{
   return;
  }
  catch {...}
  finally{ console.log("yo")}; //will print yo
  ```
  * ``try {...} finally {...}`` is allowed in JS too.
-
- The error [[Object]] has 3 properties, ``name``, ``message`` and ``stack``.
  It is thrown using the ``throw`` keyword. JS also provides built-in Error Objects.
  For ex.:
  ```js
  let x= {
   name: "ayo",
  };
  try{
   throw x; 
   //or throw new Error("ayo"), or throw new SyntaxError("ayo") etc.
  } catch (err) {
   console.log(err.name); //prints ayo
  }
  ```
  For built-in error Objects, the name is the Object's Ctor name, and message is the provided string.
  
  We can check if a given error is a certain type with 
  ``if (err instanceOf ReferenceError){...}`` and the like.
  
  * Rethrowing is allowed in JS.
  * ``throw`` throws any given data type, be it [[Object]], [[Number]] etc.
- Global Error Handling
  In browsers, the [[window]] Object provides a global error event handler ``.onerror``and in Node, ``process.on("<exception type>")`` provides global error event handler per error type.
  For ex.:
  ```js
  window.onerror= function(message, url, line, col, err) {...}
  ```
  
  External error-handling utilities make use of these handlers and set them to custom values. So when we insert a JS [[<script>]] URL from one of these services, they assign a custom value to these methods and then provide the details by sending errors to their [[API]]s.
  For ex.: [muscula](https://www.muscula.com/)  and [errorception](https://errorception.com/)
- ``unhandledrejection`` [[Browser]] [[Event]]
  Can be caught with
  ```js
  window.addEventListener('unhandledrejection', function(event) {
    // the event object has two special properties:
    alert(event.promise); // [object Promise] - the promise that generated the error
    alert(event.reason); // Error: Whoops! - the unhandled error object
  });
  
  ```