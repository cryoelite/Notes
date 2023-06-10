- A weaker variant of the [[Map]].
- Methods:
  ``new WeakMap()``
  * [`weakMap.set(key, value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/set)
  * [`weakMap.get(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/get)
  * [`weakMap.delete(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/delete)
  * [`weakMap.has(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/has)
- Only accepts [[Object]]s as keys and doesn't prevent [[Garbage Collection]].
  That is,
  ```js
  let x = new Map();
  let y = {};
  x.set(y,2);
  y=null;
  
  //y still exists in Map x
  ```
  Now even though we can never explicitly get y again, it's still a reachable reference and is hence not Garbage Collected. This is also because we can iterate over Map keys and then get it again.
  A weakMap prevents that as it is not [[Iterable]] and once an Object is removed, it is unreachable and is hence removed from the WeakMap as well.
  That is,
  ```js
  let x = new WeakMap();
  let y = {};
  x.set(y,2);
  y=null;
  
  //y doesn't exist anymore in WeakMap x
  
  ```
  The GC can occur at any time, it is upto the JS engine and is not automatically triggered when an Object loses reference.
-