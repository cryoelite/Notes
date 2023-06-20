- Currying is a general programming concept where we turn a [[Function]] ``f(a,b,c)`` call into ``f(a)(b)(c)``.
  For ex.:
  ```js
  function curry(func) {
  
    return function curried(...args) {
      if (args.length >= func.length) {
        return func.apply(this, args);
      } else {
        return function(...args2) {
          return curried.apply(this, args.concat(args2));
        }
      }
    };
  
  }
  
  
  function sum(a, b) {
    return a + b;
  }
  
  let curriedSum = curry(sum);
  
  console.log( curriedSum(1)(2) ); //prints 3
  ```
-