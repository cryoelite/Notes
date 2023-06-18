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
- In general, for..of can be used with any [[Object]] that implements the [[Symbol]] Symbol.iterator. This property should hold a function that returns an Object which has a ``next()`` [[Function]] in it. This ``next()`` function then must return an Object with 2 keys,
  {
   done: boolean,
   value: any,
  };
  
  For ex.:
  ```js
  let x= { 
   a: 0,
   b: 5,
   [Symbol.iterator]: function() {
    return {
     a: this.a,
     b: this.b,
     next() {
       if (a<=b) 
          return {done: false, value: this.a++};
       else
           return {done:true, value: this.a};
      }
    };
   }
  };
  
  for (let i of x) {...} //gets 0,1,2,3,4,5
  
  ```
  This [[Object]] is called an [[Iterable]].
  
  for..of calls the function associated with Symbol.Iterator once and parses the Object returned from it.
  Then it call's the next() method of that Object and keeps calling it until it returns an Object with done: true.
  
  This is also to say that we can simply use any Object's iterator explicitly as well.
  For ex.:
  ```js
  let x = "abc";
  let it= x[Symbol.iterator];
  let val = it[next]();
  while(!val[done]) {...};
  ```
- We can also use [[Generator Function]]s with ``Symbol.Iterator``
  For ex.:
  ```js
  let x = {
   start: 0,
   end: 5,
   *[Symbol.Iterator]() {
     for(let value= start; start<= end; ++start)
     yield value;
    }
  }; //works 
  ```
-