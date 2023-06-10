- An [[Operator]] . Returns the type of a value/variable in string.
- For ex.:
  ```js
  typeof 2 //"Integer"
  - typeof null //"object"
  - typeof Symbol("id") //"symbol"
  - typeof alert //"Function"
  ```
  The typeof null is not "Object", this is a known error with typeof.
  [[Function]]s are not a type in JS, they belong to [[object]] but typeof returns "Function" for the sake of convenience.
- There's also the function variant of ``typeof(...)``, it's the same but uses a [[Function]] instead.