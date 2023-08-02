- Defined in the ``std::ops`` [[Module]] of [[Standard Library]] .
  A range is a [[Struct]] defined like so
  
  ```rust
  pub struct Range<Idx> {
      pub start: Idx,
      pub end: Idx,
  }
  
  pub struct RangeInclusive<Idx> {
      pub start: Idx,
      pub end: Idx,
  }
  ```
  
  We can create a ``Range`` in Rust with a special syntax
  ``
  <start value, inclusive> .. <end value, exclusive>
  ``
  or create a ``RangeInclusive`` with 
  ``
  <start value, inclusive> .. =<end value, inclusive>
  ``
  
  These syntaxes don't mean anything by theirselves, but other constructs in Rust understand they mean to represent a [[Collection]] of values like an array and they must iterate from start to end.
  
  For ex.:
  ```rust
  use std::ops::Range;
  
  fn main() {
      for i in 1..=5 {
          //loops from 1 to 5 (inclusive)
      }
  
      let x: Range<i32> = 0..5;
      let y: Vec<i32> = (0..=5).map(|x| x).collect();
  } //works
  ```
-