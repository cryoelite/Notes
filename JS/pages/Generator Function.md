- A special type of [[Function]].
- Like Dart or python, JS also supports generator functions which can return values multiple times creating the concept of ``streams``.
  It works like so
  ```js
  function* yo(){
   yield 1;
   yield 2;
   return 3;
  }
  let genny = yo();
  console.log(genny.next()); //prints {value: 1, done: false}
  console.log(genny.next()); //prints {value: 2, done: false}
  console.log(genny.next()); //prints {value: 3, done: true}
  
  let x = {
   *yo(){ //generator methods
     ...
    }
   aye: function*() {} 
  };
  ```
  These functions are defined with ``function*`` keyword.  Then we can ``yield`` values in the function. Yielding returns a value or nothing, in which case [[undefined]] is returned, yielding doesn't finish the function, it will send the control back but the position in the function will be remembered so when it is called again it will continue from the yield to the next yield or return. ``return`` however returns the control back and function won't resume again.
  
  A generator function returns an [[Object]] called a ``Generator Object`` which has a method ``.next(<optional arg>)``. When we call next it runs until either it finds a yield , or the function finishes, or the return is found. In all cases ``.next()`` returns an Object with 2 properties, value and done, the done is set to true if the function is finished, that is return or the function's over. value has the returned value, or undefined. 
  Calling ``next`` on a finished Generator Object gives value as undefined and done as true.
  Calling the function again can create another generator Object so we can reuse it.
  For ex.:
  ```js
  function* yo() {
   return 2; 
  }
  
  let x = yo();
  console.log(x.next()); //prints {value: 2, done: true}
  console.log(x.next()); //prints {value: undefined, done: true}
  
  x = yo();
  console.log(x.next()); //prints {value: 2, done: true}
  ```
  
  
  
   for..of [[Loop]]
  Generator Object's return an [[Iterable]] Object, so they can be iterated over. We can use for..of to do so 
  ```js
  function* yo() {
   yield 1;
   return 2;
  }
  (for let value of yo){....} //value gets just the value of next()
  
  //similarly
  let x = [...yo]; //as its an iterator, it can use spread Operator
  console.log(x); //prints 1 (not 2)
  
  ```
  
  However, iterators don't get the value returned using ``return``, so they may not completely finish the generator function. This is why it is recommended to only ``yield`` values inside a generator function and let it implicitly return.
-
- Generator Composition
  Using ``yield*`` we can delegate yield to another Generator, this is called Generator Composition.
  For ex.:
  ```js
  function* gen1(){
   yield 2;
   yield 3;
  }
  function* gen2() {
   yield* gen1();
   yield 4;
  }
  let x= gen2();
  for(let value of x){
   console.log(value); //prints 2 3 4
  }
  ```
  The flow is exactly like expected, until the delegated generator yields all its values, it keeps yielding.
- ``.next(<arg>)`` can pass value to ``yield`` [[Operator]
  For ex.:
  ```js
  function* gen(){
  let x = yield 1;
  console.log(x);
  }
  
  let x = gen();
  console.log(x.next().value); //prints 1 
  x.next(5); //prints 5
  console.log(x.next().value); //prints undefined
  ```
  The first call to ``.next()`` runs the generator till the yield's result. Then it is suspended where yield would return some value or [[undefined]], then the 2nd call to ``.next()`` passes an arg which becomes the ``yield``'s return value and runs the generator until end and last ``.next()`` has no value so it returns undefined.
  
  Similarly, we can also send an [[Error]] into a generator. To do so we use ``<generator obj>.throw(<err Obj>)``
  For ex.:
  ```js
  function* gen() {
  try {
    let x = yield 1;
    console.log(x);
  } catch (err) {
    console.log(err.message);
  }
  }
  - let x = gen();
  console.log(x.next()); //prints 1
  x.throw(new Error("nah")); //prints nah
  ```
  It continues the generator after passing an Error to the yield, if the error is unhandled it leaks back into the calling code.
- ``<generator obj>.return(<arg>)`` Returns an Object like ``.next()`` returns with the given value and finishes the generator so done is set to true.
-
-
-