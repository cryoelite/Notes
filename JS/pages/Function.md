- To create one, 
  ``function <name>(<param 1>, <param 2>...) {...}``
  If we have a function ``xyz`` we can call it with xyz();
- Just like other languages, [[ECMAScript]] also has [[Scope]]s, so variables declared inside Functions are only visible inside the function's scope. They can use/modify variables from outer scope unless they are [[Shadowed]].
- Parameters don't need to have any type and can also be omitted at call-site.
  For ex.:
  ```js
  function xyz(x,y= yo(), z="na", p) {
  ...
  
  }
  //can be called with
  xyz();
  xyz("yo");
  xyz("yooo","yo","a","b","c"); 
  xyz("x", undefined);
  //etc.
  ```
  Here y and z are ``default`` parameters as when they are provided no value or undefined (strict equality) then the given default value is evaluated and assigned.
- We can check if a parameter is provided a value with the [[Nullish-Coalescing Operator]] or the || or explicit check for undefined.
  ```js
  function xyz(x) {
  if(x===undefined) x=2;
  }
  
  ```
- [[Return]] keyword returns a value, just like other languages. However since there the types are dynamic, there is no change in the function syntax.
  For ex.:
  ```js
  function x(){
    return 2;
  }
  ```
  
  If a function doesn't return anything, it returns undefined.
  The expression to be evaluated at return or the value to be returned must start on the same line as the return as [[ASI]] assumes semicolon after an empty return.
-
- Function Expression
  In [[ECMAScript]] a function is just a value to be evaluated, this is why it can also be assigned to variables.
  For ex.:
  ```js
  let x= function() {
    ...
  }; //Notice the semicolon
  //called with 
  x();
  ```
  Here ``function(){...}`` is a Function Expression. Just like a normal function, if it returns a value, that will be returned from ``x()`` otherwise undefined will be returned.
  
  The semicolon is because the Function Expression is still a value being assigned, and just like other values being assigned, it indicates the end of value.
- Function Declarations can be called before they are declared in their own scope, Function Expression's can't.
  For ex.:
  ```js
  function xyz(){
    pp();
    function pp() {
      console.log("yoo");
    }
  }
  
  xyz();
  ```
  works and prints "yoo". But if ``pp`` was assigned to a variable, it would have needed to be called after declaration of the variable. 
  This is because [[JavaScript]] Engine creates the function declarations before executing the code in an [[Initialization Stage]].
- Functions in [[JavaScript]] allow nesting. However they are only visible in their respective [[Scope]]s and the ones below in strict mode.
- If we call a function without evaluating it, it returns the function body.
  For ex.:
  ```js 
  function sayHi() {
    alert( "Hello" );
  }
  
  alert( sayHi ); //prints function sayHi() {
                          // alert( "Hello" );
                          // }
  ```
  ``alert(...)`` gets the function body converted to [[string]] and prints that. 
  This also allows us to assign the function to other variables.
- Callback Function
  A function passed as a parameter to another function.
  For ex.:
  ```js 
  function x(p) {p();} //Here p is a callback function
  
  ```
- Arrow Function
  These functions are an even simpler way to create Function Expressions.
  ```js
  let x= () => {
    return 2;
  };
  let y = () => 2;
  
  console.log(x());
  console.log(y());
  ```
- Constructor Function
  This function can create [[Object]]s at runtime.
  We use [[new]] [[Operator]] and [[this]] to do so.
  For ex.:
  ```js
  function Yo(name) {
   this.name = name
   this.age = 2;
   this.hi = function() {...};
  }
  
  let x= new Yo("Haa");
   x.hi(); //works
  ```
  Ctor functions should be PascalCase by convention. 
  new keyword here creates a new empty Object and passes it to Yo(), which then uses ``this`` to modify it. Then the value of ``this`` is returned implicitly.
- The main purpose of Ctor Fns is reusability.
  However, there's a one-off version as well.
  For ex.:
  ```js
  let x = new Function() {
    this.name = "yo";
  };
  ```
  Here [[new]] does the same as earlier, however, this function is immediately called and evaluated for the variable. The result is that x contains an Object returned from [[this]].  And this Function can't be called again. 
  The purpose of this Ctor Fn is simply to offer encapsulation.
- Return from Ctor Fn
  If a Ctor fn returns an Object, that is returned instead of this.
  For any other value, the value is ignored and this is returned.
  For ex.:
  ```js
  function BigUser() {
    this.name = "John";
    return { name: "Godzilla" };  // <-- returns this object
  }
  alert( new BigUser().name );  // Godzilla, got that object
  ```
-