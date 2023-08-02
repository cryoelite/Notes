- Many of Rust's compile-time checks can be bypassed by unsafe rust. This allows more control over the behavior of the code however the checks aren't even performed and it's the duty of the programmers to ensure the code does not cause issues at runtime.
  
  Rust is inherently conservative with its rules at compile-time. That is, it would rather fail a correct program than allow a potentially error-causing code to run. By using unsafe rust, we disable these checks at compile time and taking the various guarantees in our hands.
  
  To use unsafe rust, we have a [[Scope]] block that starts with the ``unsafe`` keyword. 
  For ex.:
  ```rust
  unsafe fn foo() {}
  fn main() {
      unsafe {
          foo();
      }
  }
  
  ```
  
  The unsafe blocks don't completely disable Rust's compile-time rules, they simply allow the unsafe superpowers to execute which would be disallowed in normal blocks.
- unsafe superpowers
  5 actions can be done in unsafe rust (inside the unsafe block), they are
  * Dereferencing a Raw [[Pointer]] 
  * Calling an unsafe [[Function]]/ [[Method]]: 
  We declare [[Function]]s/ [[Method]]s as ``unsafe``. Then the entire function body is unsafe. We can only call these functions from unsafe blocks.
  
  For ex.:
  ```rust
  unsafe fn foo() {}
  fn main() {
      unsafe {
          foo();
      }
  }
  
  ```
  It is advisable to not mark functions as ``unsafe``, rather use unsafe blocks inside them. This way we create a safe abstraction over an unsafe code. 
  
  This is required with [[extern]] methods.
  * Access/Modify mutable static [[Variable]]s:
  * Implement an unsafe [[Trait]]
  * Access fields of [[union]]s
-