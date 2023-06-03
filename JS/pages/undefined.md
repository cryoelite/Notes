- Just like [[null]], ``undefined`` has no [[Object]] wrapper and has only 1 value to its type, ``undefined``.
- Represents an un-assigned value. We can assign manually or it can be a result of not initializing a variable either because of a non-assignment or because of an error that itself returns undefined.
  For ex.:
  ```js
  let x= undefined; //Not recommended but works
  let y; //value is 'undefined' until assigned
  ```