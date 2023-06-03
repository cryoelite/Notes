- The standard,
  ``for(init; bool; step)`` 
  ``while(<bool>) {...}``
  and
  ``do {...} while (<bool>);``
- [[break]] and [[continue]] directives are supported. Work like in other languages, they break/continue the immediately above loop.
- for..in Loop
  With [[Object]]s we can use this loop to loop over properties of the object.
  For ex.:
  ```js
  let x= {...};
  for(let key in x) {...} //use x[key] to access values or just key to access property names.
  ```
  It doesn't loop over [[Symbol]] properties.
- for..of Loop
  With [[String]]s we can use this loop to loop over the characters of the String.
  For ex.:
  ```js
  let x = "...";
  for (let char of x) {...}
  ```
-