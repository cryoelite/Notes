- The standard, 
  ``if(<bool>) {...} else if (<bool>) {...} else {...}``
  The {...} is optional if a single statement is used like in [[C++]].
- [[Conditional]] Operator is also supported like Ternary Operator in C++.
- Switch Statement is also supported.
  They use strict equality.
  
  For ex.:
  ```js
  let a="1";
  let b=0;
  
  switch (+a) {
  case "1": ... 
               break;
  case b+1: ... //runs this as +a converts a to number 1 and it is strictly equal to 0+1
                break;
  default: 
              break;
  }
  ```
  Just like [[C++]], the cases allow fall-through, so if any case gets matched and it doesn't have a [[break]] then the next case is executed without its condition being checked.
-