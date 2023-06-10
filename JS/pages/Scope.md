- A code block, i.e. anything between ``{...}`` where the ``{...}`` is used to hold multiple statements and lines, has its own scope and can access variables of the parent scope. A scope is a boundary within which identifiers are valid, the lifetime of any variable/function/etc. is limited to the scope/boundary they are in (and ones below them).
  
  For ex.:
  ```js
  {
   let x =2;
  }
  //x is not accessible anymore.
  
  {
   let x =2;
   {
    console.log(x); //works because x is in a parent scope.
   }
  }
  ```
- However, identifiers in scopes are passed around, this is because of [[Lexical Environment]].
-