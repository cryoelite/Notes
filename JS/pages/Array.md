- This is a special data structure in JS.
  ```js
  let x = new Array();
  let y= []; 
  ```
  are the 2 syntaxes to create an Array.
  
  It is still just an [[Object]].
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
- for..of [[Loop]] works with Array. We can use for..in too but it is not optimized for looping over arrays and also visits internal properties of the array Object.
- Multi-dimensional arrays work in JS like normal.
- ``.toString()`` returns a string with comma-separated list of elements.
  For ex.:
  ```js
  alert([1,2] + 1); // "1,21"
  ```
-