- An Iterable is an [[Object]] that has the [[Symbol]] method `[Symbol.Iterator]` or `[Symbol.asyncIterator]` in it with a value that is a function which returns an Object which has a method ``.next()`` which returns an Object with a value and a done property.
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
- [[Async]] Iteration over [[Iterable]]
  We use ``for await (let value of iterable)`` to [[Loop]] over async iterators.
  An ``async`` iterator is an Object with a [[Symbol]] property ``Symbol.asyncIterator`` that works much like ``Symbol.Iterator`` but instead uses an async ``.next()``.
  For ex.:
  ```js
  let x = {
  start = 1,
  end = 5,
  [Symbol.asyncIterator](){
  return {
   start: this.start,
   end: this.end,
   async next() {
      if(this.start <= this.end) return {done:false, value: this.start++}
      else return {done: true}
    }
   }
  }
  }
  - (async ()=> {
  for await (let value of x){
  console.log(value); //prints 1,2,3,4,5
   }
  })();
  ```
  ``Symbol.asyncIterator`` must have a ``next()`` that returns Promises, so either explicitly or implicitly using async keyword. Recall that we can't use ``await`` outside top-level of [[Module]]s or async functions or Promises, this is why we need to do this trick here to use ``for..await of``.
- [[Async]] [[Generator Function]] [[Iterable]]
  We simply make an async Iterable and use yield instead of return in its next. Then we can use an Async Iterable.
  For ex.:
  ```js
  let range = {
    from: 1,
    to: 5,
  
    async* [Symbol.asyncIterator]() {
      for(let value = this.from; value <= this.to; value++) {
        yield value;
      }
    }
  };
  
  (async () => {
  
    for await (let value of range) {
      console.log(value); // 1, then 2, then 3, then 4, then 5
    }
  
  })();
  ```
  Normal async generator functions can simply use ``async function* <name>(<params>){...}`` and for their values we need to use ``await <generator obj>.next(<optional arg>)`` to get the value.