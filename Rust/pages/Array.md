- Fixed-size, single-type & immutable
  Syntax is ``<let/const> <mutability>: [<type>; <size n>]= [<elem1>, ..., <elem n>]``
  For ex.:
  ```rust
  let arr: [i32;3]= [1,2,3];
  let x= arr[0]; //to access
  ```
  The type and size can be omitted, and it is inferred.
- Just like C++, arrays occupy space on the Stack.
- Arrays can also be initialized with default values
  For ex.:
  ```rust
  let arr = [3;5];  // Has 5 values, all are 3 and of type i32 as automatically inferred.
  ```
- Accessing a value out of bounds leads to a [[Panic]] at runtime/compile-time if the compiler has the index available at compile-time.
- [[Destructuring]] using [[Pattern Matching]] also works for arrays
  For ex.:
  ```rust
  let arr= [1,2,3];
  let [x,y,z] = arr;
  ```
-