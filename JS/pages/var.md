- An old way of declaring variables, it is unrecommended to be used in modern JS.
- Variables declared with var are only either [[Function]] [[Scope]]d or Global Scoped
  For ex.:
  ```js
  {
   var yo= 2;
  }
  console.log(yo); //works, prints 2
  ```
  Here yo became Global Scoped and that allows it to be visible in any other scope.
  var can be redeclared. The redeclaration is simply ignored and if there's initialization with redeclaration, that is assigned to the variable.
  For ex.:
  ```js
  var x=2;
  var x=3;
  console.log(x); //prints 3
  ```
- Variables declared with var are ``hoisted``, aka ``raised`` to the top of the function/global [[Scope]].
  However, their assignments are not, they occur only where they are made.
  That is,
  ```js
  function sayHi() {
  console.log(phrase); // is not an error, prints undefined
  phrase = "Hello"; // is not an error
  
    if (false) {
      var phrase= "yo";
    }
  
    console.log(phrase);
  }
  sayHi();
  ```
  Works. ``phrase`` is undefined, but declared in the first line inside the function. The declaration from below ``hoists``, pierces through the code block and gets declared at function start. This is how var declares variables.
-