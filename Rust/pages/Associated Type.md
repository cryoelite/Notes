- Just like [[Generic Type]]s on [[Trait]]s, we can define types in trait definitions which the implementors would then define and hence we get something similar to a generic type but without using generics and a more concise trait definition.
  
  Syntax:
  ``
  trait <trait name> {
   type <some type name 1>;
   type <some type name 2>; 
    //and so on
    //then use them as types in any following methods, as they are part of Self, we need to use Self::<some type name> 
  }
  
  impl <trait name> for <type name> {
     type <some type name 1> = <some type>;
     type <some type name 2> = <some type>;
    //and define the methods with either <some type> or Self::<some type name> 
  }
  ``
  [[Method]] definitions can use ``Self::<type name>`` in trait definition.
  
  For ex.:
  ```rust
  trait X {
      type A;
      type B;
      fn yo(a: Self::A, b: Self::B);
      fn no(&self, a: Self::A, b: Self::B);
  }
  
  struct M {}
  
  impl X for M {
      type A = i32;
      type B = f64;
      fn yo(a: Self::A, b: f64) {}
      fn no(&self, a: Self::A, b: Self::B) {}
  }
  
  fn main() {
      let m = M {};
      M::yo(2, 4.0);
      m.no(5, 6.0);
  } 
  ```