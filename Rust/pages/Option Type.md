- This is a predefined ``enum`` in Rust's [[Prelude]]. As we know, rust doesn't have the concept of *null*, the Option Enum is the closes to null we get in Rust as it simply has a variant used to represent the absence of a value.
- It is defined like so
  ```rust
  enum Option<T> {
    None,
    Some(T),
  }
  ```
  where ``<T>`` is a [[Generic Type]] definition.
  
  
  For ex.:
  ```rust
  fn main() {
   let x = Option::Some("yo"); //T is &str
   let y = None; //Rust allows shorthanding Some or None for the Option's variants.
   let z= match x {
    Option::Some(value) => 2,
    None => 5
   }
  }
  ```
  
  The benefit is, we don't need to worry if a value is *null*, because we have to use [[Pattern Matching]] to get value out of an enum and hence it is guaranteed we will only operate on Option's values if they are ``Some``.
- [[Unwrap]]
  This [[Method]] is implemented for ``Option``.