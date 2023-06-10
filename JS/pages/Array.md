- This is a special data structure in JS.
  ```js
  let x = new Array();
  let y= []; 
  ```
  are the 2 syntaxes to create an Array.
  
  It is still just an [[Object]], just quite optimized by the JS engine.
- 0-indexed.
- Access is the same with ``<array obj>[<int>]`` as in [[C++]], but it doesn't support going from the end using negative int value. The ``.at(<int>)`` method is the same as ``[ ]`` but also supports indexing from the right end.
- For size we have ``.length``.
  However, it doesn't give the actual count of the values in the array, just the max. index + 1.
  For ex.:
  ```js
  let fruits = [];
  fruits[123] = "Apple";
  
  fruits.length; // 124
  ```
  using Array as an Object we have broken ``length`` here.
  
  Similarly, we can modify the length property directly and that works as well.
- Arrays in JS are not homogenous, so they can hold values of any type and any element can be any type, even [[Object]]s
- Stack/Queue like methods
  ``.push(<val>)``: Push an element to the end of an array
  ``.pop()``: Get an element from the end of an array and remove it
  ``.shift()``: Get an element from the front and remove it
  ``.unshift(<val>)``: Push an element to the front.
  
  Yes shift and unshift have a TC of O(n).
- Unlike an [[Object]], Arrays are special as the engine performs various optimizations with this Object such as storing all elements in contiguous memory locations.
- Arrays can still be used like an [[Object]], however it is unadvised to do so as it breaks various optimizations on them.
- for..of [[Loop]] works with Array. We can use for..in too but it is not optimized for looping over arrays and also visits internal properties of the array Object. Arrays implement [[Iterable]]s so they work directly with for..of.
  
  ``.forEach(<func>(item,index,array)`` 
  For each element of the array, call the [[Function]] and pass it the given 3 items.
  For ex.:
  ```js
  let x =[1,2];
  x.forEach(alert); //passes each element to alert.
  ```
  This is also to say, Functions in JS can be passed around like so and JS automatically passes them the arguments.
- Multi-dimensional arrays work in JS like normal.
- ``.toString()`` returns a string with comma-separated list of elements.
  For ex.:
  ```js
  alert([1,2] + 1); // "1,21"
  ```
- Delete an element:
  We can use ``delete arr[2];`` and it deletes the element but this is an [[Object]] way of deletion and is hence unrecommended, also it doesn't modify the length of the array and if we access the element we get [[undefined]].
  
  Alternatively, we can use ``.splice(<start>, <deleteCount>);``
  Removes from given index to given number of elements and shifts the rest of the array. It also accepts negative index.
  For ex.:
  ```js
  let arr = ["1","2","3"];
  arr.splice(1,1);
  console.log(arr); //prints 1,3
  ```
- We can also use ``.splice()`` to insert elements at a given index.
  For ex.:
  ```js
  let arr = ["1","2","3"];
  arr.splice(1,0, "a", 3);
  console.log(arr); //prints 1,2,3,a,3
  ```
- ``.slice(<optional start>, <optional end>)`` returns an array from an array from start (inclusive) to end (exclusive).
- Concatenation
  ``.concat(<arg1>,<arg2>...)`` adds given element to the array. If the arg is an array, or more specifically is an [[Object]] with a [[Symbol]] property Symbol.isConcatSpreadable with value ``true`` then all the elements/values from K:V pairs added to the array except some special ones like ``length``.
  For ex.:
  ```js
  let x =[1,2];
  x.concat(2,4); //ok, makes x 1,2,2,4
  x.concat([2,3]); //1,2,2,4,2,3
  
  let y= {
   0: "yo",
   1: "noo",
   [Symbol.isConcatSpreadable]: true,
   length: 2,
  };
  
  x.concat(y); //ok, 1,2,2,4,2,3,"yo", "noo"
  ```
- Searching an element:
  The usual ``.indexOf()/.lastIndexOf()/.includes()/.find()/.findIndex()/.findLastIndex()/.filter()`` etc.
- ``.map(<func>(item,index,array))`` Returns an array after applying the given function to the item and getting its return value.
  
  Also passes ``this`` to the func.
- ``.reverse()`` to modify the array and reverse its elements.
- ``.join(<optional str>)`` Joins an array and returns a [[String]] with the optional string used to specify where each element was joined.
- ``.sort(<optional func>(a,b))`` modify an array by sorting all elements. Optional func is custom comparator.
- ``.reduce(<func>(accumulatedValue, item, index, array), <optional initial value>)`` to reduce an array.
  Similarly we have ``.reduceRight(...)`` which goes from right end to the left end.
  Also passes ``this`` to the func.
- [[typeOf]] doesn't work with arrays, it returns ``Object``. 
  We use ``Array.isArray(<obj>)`` instead.
- ``Array.from(<obj>, <optional mapping function>(item), <this arg>)`` Takes an object and returns an Array Object from it. it takes the value of every key in the Object except a few special properties such as ``length``, also making it an [[Iterable]] in the process. The custom mapping function takes each value and can process it and return a new value.
- Since an Array is built on an [[Object]], a variable comparing one Array to another variable compares the reference and not the individual elements.