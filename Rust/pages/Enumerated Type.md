alias:: Enum

- ``enum``
  Short for enumerations, as they allow us to enumerate the possible variants of a defined [[Data Type]]. 
  
  Syntax:
  ``
  enum <name>{ 
   <type names>
  }
  ``
  For ex.:
  ```rust
  enum Size {
   SMALL,
   LARGE
  }
  fn main() {
   let carSize: Size= Size::LARGE;
  }
  ```
  Variants of an ``enum`` are namespaced under its identifier.
- Variant of an ``enum`` can optionally also take a [[Function]] with some parameters.
  This makes the function create an instance of an ``enum`` with the given variant and values provided to it.
  
  For ex.:
  ```rust
  enum X {
      Y(i32, String, i32),
      Z,
  }
  
  fn main() {
      let x = X::Y(2, String::from("yo"), 5);
      match x {
          X::Y(value1, string, value2) => { 
              println!("{}", value1); //to access
          }
          _ => {
              println!("Invalid variant");
          }
      } 
  }
  ```
  We use [[Pattern Matching]] to access an ``enum``s value, because it is not guaranteed what variant of an enum is stored in a variable.
  
  * This function can then also have named fields as parameters.
  For ex.:
  ```rust
  enum X {
      Y(i32, String, i32),
      Z(x: i32),
  }
  
  fn main() {
      let x = X::Z(x: 2); //to instantiate
      match x {
          X::Y(value1, string, value2) => {
              println!("{}", value1);
          }
          X::Z {x} => { //to access
              println!("{}", x);
          }
      } 
  }
  ```
  Just usual [[Pattern Matching]].
- [[Method]]
  Enums can have methods too. 
  Methods of an ``enum`` apply to all its variants if they use ``self``.
  
  For ex.:
  ```rust
  enum X{
   Y
  }
  
  impl X {
   fn yo() {}
   fn yas(&self) {}
  }
  
  fn main() {
   let z= X::Y;
   X::yo(); //for non-instance methods
   z.yas(); //for instance methods
  }
  ```
- [[Option Type]]
- [[Result Type]]
- Size of an Enum
  The size of an enum is the same as the biggest variant of it, so if an enum had a variant ``X`` and then a variant ``Y(i32)`` then the size of the enum would be the same as an ``i32``. 
  The reason is pretty simple, an enum's value can only be any 1 of its variants, so the largest variant's size is the size of the enum itself.