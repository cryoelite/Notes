- Fixed-size, multi-type and immutable.
  Syntax ``<let/const> <mutability> <varname>: (<type1>, <type2>...,<type n>) = (<value of type 1>, <value of type 2>..., <value of type n>)``
  For ex.:
  ```rust
  let tup: (u8, bool, f32)= (1, true, 2.0);  //to declare
  let x =tup.0; //to access
  ```
  A single variable can bind to a tuple with multiple values/types is because a tuple is seen as a single compound element, so the variable binds to that.
- Defining the types is optional as they can be automatically inferred too.
- We access a tuple's values by its indices and using the ``.`` [[Operator]] on its variable, this is called tuple indexing. Indices start at 0.
  Alternative, we can also use [[Pattern Matching]] to get a bind multiple variables to an entire tuple.
  For ex.:
  ```rust
      let tup = (500, 6.4, 1);
  
      let (x, y, z) = tup;
      let k= tup.0; //access by index
  ```
  This is called Tuple [[Destructuring]] because we destructure a single variable into multiple variables.
-
-