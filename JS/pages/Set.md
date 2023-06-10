- In JS, a Set is less like an [[Array]] and more like a [[Map]] as it is still an [[Object]] and hence we need to use its methods to CRUD values in it.
- Main methods
  * [`new Set([iterable])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/Set) – creates the set, and if an `iterable` object is provided (usually an array), copies values from it into the set.
  * [`set.add(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/add) – adds a value, returns the set itself.
  * [`set.delete(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/delete) – removes the value, returns `true` if `value` existed at the moment of the call, otherwise `false`.
  * [`set.has(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/has) – returns `true` if the value exists in the set, otherwise `false`.
  * [`set.clear()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/clear) – removes everything from the set.
  * [`set.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/size) – is the elements count.
  * [`set.keys()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/keys) – returns an iterable object for values,
  * [`set.values()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/values) – same as `set.keys()`, for compatibility with `Map`,
  * [`set.entries()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/entries) – returns an iterable object for entries `[value, value]`, exists for compatibility with `Map`.
- Just like a map, a set can take value of any type.