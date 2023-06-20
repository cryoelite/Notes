- This special [[Function]] takes a [[String]], parses it as [[ECMAScript]] and executes the code in the string as JS for the current [[Scope]]/[[Lexical Environment]]. It can also return value, which is value of the parsed code.
  For ex.:
  ```js
  let x =1;
  let y= eval("x+1");
  console.log(y); //prints 2
  
  eval("console.log(`${x} says hello`)"); //prints 1 says hello
  eval("let z= 1; console.log(z+x);");//prints 2
  ```
  To have a multi-line eval we can use ``;``.
  
  In [[Old Mode]] it can create, read and update the variables for the Scope. However in ``strict mode`` it can only read and update variables, it can create and use variables inside but they are not created for the surrounding [[Lexical Environment]]. This is because in ``strict mode`` it has its own [[Lexical Environment]], that is, it has its own Scope so its like a block.
- Unrecommended to use it, it even harms [[Minifier]] as if it references variables they can't be minified. 
  Still if it has to be used, when variables don't need to be accessed, better to use ``window.eval(...)`` in [[Browser]]s. And if variables have to be used, use ``new Function(...)`` [[new]] [[Function]]  instead as it also works similarly.