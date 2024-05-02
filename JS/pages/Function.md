filters:: {"generator function" true}

- To create one, 
  ``function <name>(<param 1>, <param 2>...) {...}``
  If we have a function ``xyz`` we can call it with ``xyz();``
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
  
  This is also to say, in JS functions can receive any number of args despite their parameters, if there are more args than parameters then extra args are ignored, if there are less args than parameters then the unassigned parameters are [[undefined]]. 
  
  And this is also to say, functions in JS are uniquely identified by just their names. Which is to say there is no function overloading in JS, as there can be only 1 function with a given name, if there's more, one of them has the other [[Shadowed]]
  For ex.:
  ```js
  function yo(a) {
    console.log("1");
  }
  
  function yo() {
    console.log("2");
  }
  
  let x=  {
      yo(){
          console.log("1");
      },
      yo(){
          console.log("2");
      }
  };
  
  class X{
      yo(){
          console.log("1");
      }
      yo(){
          console.log("2");
      }
  }
  yo(); //prints 2
  x.yo(); //also prints 2
  
  let obj= new X();
  obj.yo(); //also prints 2
  ```
  The same is true in [[Class]]es and [[Object]]s.
-
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
- Rest arguments
  Using the ``...`` spread [[Operator]] on a parameter we can convert incoming args to elements in an [[Array]] .
  For ex.:
  ```js
  function yo(a,...b) {
   //use b as an array
  }
  yo(1,2,3,3,4); //ok, so b[0] will be 2, b[1] will be 3 and so on.
  ```
  
  The rest parameter must be at the end of parameter definition in a function.
- Array-like special Object ``arguments`` 
  Every function (except arrow function) has this [[Object]] which has **all** the args of a function.
  For ex.:
  ```js
  function yo (a) {
   console.log(arguments[0]); 
   console.log(arguments[1]); 
   console.log(arguments[2]); 
  }
  yo(1,2); //prints 1 2 undefined
  ```
  Unlike the rest parameter, this Object captures all the args. It is [[Iterable]] and uses array-like syntax for accessing values but it is not an [[Array]] and hence doesn't support the array methods.
  The arrow function doesn't have this Object and if we use it inside it, it access the ``arguments`` of the parent function.
- The ``...`` spread [[Operator]] can also be used to expand an [[Iterable]] into individual args.
  For ex.:
  ```js
  let x = [1,2,5,1,2];
  console.log(Math.max(...x)); // unpacks x and passes all elements of x to .max(...)
  ```
  
  Similarly, it can also be used to copy an Object.
  ```js
  let x = { a: 2, b:"yo" };
  let y= { ...x }; //Spread the list of properties and copy them into this empty object with their values.
  x["a"]; //prints 2;
  x==y; //false
  ```
-
-
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
  ``alert(...)`` gets the function body converted to [[String]] and prints that. 
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
  Returns ``this`` [[Object]] instead of [[undefined]] implicitly.
- Ctor functions should be PascalCase by convention. 
  new keyword here creates a new empty Object and passes it to Yo(), which then uses ``this`` to modify it. Then the value of ``this`` is returned implicitly.
  
  Arrow Functions can't be used with Ctor Fns.
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
- Recursion works just like any other language. Max safe recursion depth is 10000, and maybe more depending on the engine and if it supports ``Tail-Call Optimization``.
  
  In JS, each function call is associated with an [[Execution Context]] which is stored in an [[Execution Context Stack]] and this is the stack that tracks the program's flow.
- JS supports Nested Functions.
- ``IIFE``: Immediately Invoked Function Expressions
  That is, this syntax allows us to declare a function expression and immediately execute it. 
  For ex.:
  ```js
  (function() {
   ...
  }
  )();
  
  // Ways to create IIFE
  
  (function() {
    alert("Parentheses around the function");
  })();
  
  (function() {
    alert("Parentheses around the whole thing");
  }());
  
  !function() {
    alert("Bitwise NOT operator starts the expression");
  }();
  
  +function() {
    alert("Unary plus starts the expression");
  }();
  ```
  We can only do it with function expressions.
- ``NFE``: Named Function Expressions
  Function expressions can use an ``internal name``. This allows them to reference theirselves.
  For ex.:
  ```js
  let x = function yo() {
   yo();
  }; //ok
  
  let y = function()  {
   y(); //works as well, however
  };
  
  let z = y;
  y = null;
  
  //now z() will yield an error since it calls y() which is null.
  ```
- Functions are still [[Object]]s in JS, there is no Function type. That means they have properties too.
  Like the ``name`` property, in JS, all functions have this property set automatically, and through a feature called ``contextual name``, in the [[Compilation]] the property is set to the correct name through context of the code.
  For ex.:
  ```js
  function yo() {}
  
  console.log(yo.name); //prints yo
  
  let x = function() {};
  
  console.log(x.name); //prints x
  
  let y= {
   yo() {}
   hey: function(){}
  };
  
  console.log(y.yo.name); //prints yo
  console.log(y.hey.name); //prints hey
  ```
  For ``NFEs``, name is set to the ``internal name`` and not the variable name.
- Similarly, there's a ``length`` property, it returns the no. of parameters except the rest parameter a function accepts.
- We can use Functions as [[Object]]s by setting custom properties as well. 
  For ex.:
  ```js
  function yo() {
   if(yo.count=== undefined) yo.count=0;
   
  yo.count++;
  }
  yo();
  yo();
  yo.count; //returns 2
  ```
  This is just like using closures, except the function property is visible on the same [[Scope]] as the function's declaration/expression as well as inside it meaning we don't need to rely on closures to remember and set variables (properties) on the [[Environment Object]]. Still, it takes space in the function Object and unlike the [[Environment Object]], a function's lifetime ends at the end of the scope, so for [[Global]] Functions, this means their properties are not [[Garbage Collection]]ed until the end of the program or explicit deletion.
- We can also create Functions from [[String]]s, this is done using [[new]] keyword.
  The format is
  ``let x = new Function (<optional param 1 in string>, <optional param 2 in string>, ...<optional param n in string>, <string function body>);``
  
  For ex.:
  ```js
  let x = new Function('a', 'b', 'return a+b'); //works. 
  ```
  These functions are evaluated and dynamically created at runtime. So they can be passed around different programs.
  There are many differences to how this function works, like the [[Lexical Environment]] and hence  [[Environment Object]] only refer to the [[Global]] [[Scope]]. This is because there's no guarantee the variables and other [[Object]]s it refers to will not have their names minified by the [[Minifier]], and since this function is evaluated at runtime, it can't be optimized to see the new names.
- Caching
  It's a simple concept where we simply store the result of a function if it is called often with the same values,
  For ex.:
  ```js
  function slow(x) {
  console.log(`Called with ${x}`);
  return x;
  }
  - function cachingDecorator(func) {
  let cache = new Map();
  - return function(x) {
    if (cache.has(x)) {    
      return cache.get(x); 
    }
  - let result = func(x);
  - cache.set(x, result);  
    return result;
  };
  }
  slow = cachingDecorator(slow);
  console.log( slow(1) ); // slow(1) is cached and the result returned
  console.log( "Again: " + slow(1) ); // slow(1) result returned from cache
  console.log( slow(2) ); // slow(2) is cached and the result returned
  console.log( "Again: " + slow(2) ); // slow(2) result returned from cache
  ```
  For the call of ``func(x)``, the rest of the function is a ``wrapper`` and it stores its value in it.
  However, such wrapper functions don't work with [[Object]]s because the method might access ``this`` which when called from a function's context might not be able to access
  That is,
  ```js
  let x = {
   a:2,
   b() {
  return this.a; 
  }
  };
  - function wrapper(fn) {
   let cache = new Map();
  return  function(x) {
    if (cache.has(x)) return cache.get(x);
    let result = fn(x);
    cache.set(x, result);
     return result;
  }
  }
  - x.b= wrapper(b);
  x.b(2); //error as the this in b() was using the [[Environment Object]]'s a, but when called through 
            // the wrapper's returned function the ``[[Environment]]`` was set to the function(x) which has 
           // its ``[[Environment]]`` set to wrapper(fn)
  ```
- We can use ``<function>.call(<context obj>, <optional arg1>, <optional arg2>,...)`` to explicitly set ``this`` to some other Object. This is a method present in all Function's and it does nothing special, simply set's the ``this`` to the given Object.
  So, we can change our code to
  ```js
  ...
  let result = fn.call(this,x);
  ...
  ```
  and now the wrapper works even for methods. Here we are simply ``lifting`` the context, so that the context of ``fn`` is the same as it's parent function ``function(x)`` which has ``this`` set to the Object.
  We can add variable args to our wrapper function with rest parameters and for key, hashing all args together and storing them as a single key.
- Similar to ``<function>.call(<context>,<...args>)`` we have ``<function>.apply(<context>,<iterable>)``. They work the same, however ``.apply(...)`` accepts an [[Iterable]] and not individual args, and is a bit faster as it is optimized better by the engine.
- Using ``.call(...)`` and ``.apply(...)`` we get a concept known as ``Method borrowing``. It is simply that we change context of an object's method to another, essentially ``borrowing`` a method from one object and using it in another.
  For ex.:
  ```js
  let x ={
   a:2,
   yo() {
    return this.a;
   }
   };
   let y = {
   a:4,
   yo() {
    return this.a;
   }
   };
  x.yo.call(y); //returns 4
  ```
- Function binding
  As we've seen the ``this`` [[Object]] for a function keeps changing with the change in [[Environment Object]]. ``<function>.bind(<object>, <optional arg value 1>, <... 2>, ...)`` returns a [[Function]] with a ``this`` that is bound to an Object and never changes.
  For ex.:
  ```js
  function yo(fn) {
    console.log(fn());
  }
  let x= {
    a: 2,
    ho() {
      if(this === undefined || this.a === undefined)
       return 5;
      else
        return this.a;
    }
  }
  yo(x.ho); //prints 5
  x.ho = x.ho.bind(x);
  yo(x.ho); //prints 2
  
  //Similarly, we can even bind arguments
  
  function mul(x,y) {
   console.log(x*y);
  }
  let double = mul.bind(null, 2); //Set context to null and first arg to 2
  double(3); //prints 2*3 = 6
  double(4); //prints 2*4 = 8  
  ```
  
  This is called a ``partial function application`` as it turns a function's arguments partial allowing us to skip them.
- Arrow Functions have no ``this`` and no ``arguments`` variable either. So they can be used in wrapper functions, etc. without imposing ``this`` to their bodies. 
  They can't be used with ``.bind(...)``, ``.apply(...)`` or ``.call(...)``, called with ``new``.
-