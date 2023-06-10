- An [[Object]] like data structure.
  Yes, it is still just an Object but the main specialization are the methods in it that allow keys of any type, even Object and [[NaN]] .
  To achieve this, Map uses [SameValueZero](https://tc39.github.io/ecma262/#sec-samevaluezero) alg.
- Methods:
  * [`new Map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/Map) – creates the map.
  * [`map.set(key, value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/set) – stores the value by the key.
  * [`map.get(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/get) – returns the value by the key, `undefined` if `key` doesn’t exist in map.
  * [`map.has(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/has) – returns `true` if the `key` exists, `false` otherwise.
  * [`map.delete(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/delete) – removes the element (the key/value pair) by the key.
  * [`map.clear()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/clear) – removes everything from the map.
  * [`map.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/size) – returns the current element count.
  * [`map.keys()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/keys) – returns an iterable for keys,
  * [`map.values()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/values) – returns an iterable for values,
  * [`map.entries()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/entries) – returns an iterable for entries `[key, value]`, it’s used by default in `for..of`.
  * ``.forEach(value,key,map)``
- Map can be created like so as well
  ```js
  let map = new Map([
    ['1',  'str1'],
    [1,    'num1'],
    [true, 'bool1']
  ]);
  ```
- [[Object]] has a method ``Object.entries(<obj>)`` that returns an array of K:V pairs. This can be used to directly create a Map from an Object.
- Similarly, [[Object]] also has an ``Object.fromEntries(<array of K:V pairs>)`` that returns an Object from the given K:V pairs. Using a ``<map>.entries()``, we can directly convert a Map to an Object.
-