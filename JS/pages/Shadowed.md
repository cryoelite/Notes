alias:: Variable Shadowing

- If an inner [[Scope]] declares a variable with the same name as the outer scope, then the inner scope variable will be accessed in this scope and scopes below it.
  For ex.:
  ```js
  let x=2;
  function xyz(){
      let x =3; 
     console.log(x); //prints 3
  }
  ```
-