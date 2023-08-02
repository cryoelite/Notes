- Operator Overloading
  Rust doesn’t allow operator overloading like *C++*. 
  However we can define operator behavior using [[Trait]]s, these traits are defined in ``std::ops`` [[Module]] with the name of the operation they do.
  
  For ex.:
  ```rust
  use std::ops::Add;
  
  struct X {
      Y: i32,
  }
  impl Add for X {
      type Output = i32;
      fn add(self, other: X) -> Self::Output {
          self.Y + other.Y
      }
  }
  fn main() {
      let a = X { Y: 2 };
      let b = X { Y: 3 };
      let c = a + b; //works
  }
  ```
- Explicit Type Annotation: 
  To explicitly give [[Data Type]] for any type annotation, such as in [[Generic Type]]s, we can use ``::<T>``. The ``::<>`` is called the [*Turbofish Operator*](https://turbo.fish/).
  
  For ex.:
  ```rust
  fn yo<T>(x:T){}
  
  fn main() {
  let x= yo::<i32>(2); //works
  }
  ```