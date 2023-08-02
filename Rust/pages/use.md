- ``use``
  This  keyword in Rust is much like the *using* keyword in C++. It is prepended to a [[Crate]]/ [[Module]]'s item name and then allows subsequent items in the same [[Scope]] to use the item as if they were in the same [[Scope]].
  Syntax: 
  ``
  use <some-path>;
  ``
  
  For ex.:
  ```rust
  mod x {
      pub mod y {
          pub mod z {
              pub fn yo() {}
          }
      }
  }
  
  use crate::x::y::z;
  
  fn main() {
      z::yo();
  
      use crate::x::y::z::yo;
  
      yo(); //also works
  }
  ```
  As we can see, the last item in the path after ``use`` isn't opened, the keyword simply opens the namespace till that item and plops the item's name in the current namespace.
- It's idiomatic in Rust to use ``use`` to only open up till the [[Module]]s and not the specific items, as it allows identifying if the item is in the current module or is present in some external module for maintainers. 
  But items in the [[Standard Library]] library, it is idiomatic to specify the full path to an item.
- ``use`` does not import anything. It simply opens a namespace. 
  The actual import is only made when a [[Module]] is defined at the top of a file.
  
  * When we define external packages to the [[cargo.toml]], and compile with [[Cargo]], the external crates are already imported in the [[Prelude]]. So we can directly use ``use`` without defining the [[Module]]s.
  For ex.:
  For a cargo.toml
  ```toml
  ...
  [dependencies]
  rand = "0.9.0"
  ```
  
  We can use a ``main.rs`` like so
  ```rust
  use std::io;
  use rand::Rng; //works
  
  fn main() {
      println!("Guess the number!");
  
      let secret_number = rand::thread_rng().gen_range(1..=100);
  }
  ```
- ``as``
  We can give an alias to the item brought to the current namespace by this keyword.
  
  For ex.:
  ```rust
  use x::y::z as K;
  
  //and use K wherever z would have been used.
  ```
- Re-exporting
  The ``pub`` [[Access Modifier]] can also be applied to ``use``.
  
  By default, when ``use`` is used in a [[Scope]] it works as if it only opens the namespace to an item as *private*. But by using ``pub`` to ``use`` we make the item *public*.
  This effectively allows external [[Crate]]s/ [[Module]]s to also have a direct path to an item as if the item was in this scope. 
  For ex.:
  ```rust 
  mod x {
    pub mod y {}
  }
  
  use crate::x::y; 
  //makes y available here, it is like we defined y here 
  // mod y { }
  
  mod n {
   pub mod k {
  
    }
  }
  
  pub use crate::n::k;
  // however this is like we defined pub k here and not just k
  // pub mod k {}
  ```
  
  A better example:
  ```rust
  pub mod modA {
      pub fn fnA() {}
  }
  pub mod modB {
      pub use crate::modA;
   
      pub fn fnB() {
          modA::fnA();
      }
  }
   
  mod modC {
      use crate::modB;
      pub fn fnC() {
          modB::modA::fnA(); //works, as if modA is defined inside modB. It won't work if we hadn't used pub in pub use crate::modA;
      }
  }
  
  ```
- Nested Paths
  Using ‘use’ for each item in an item can be tedious, so we can use a single ‘use’ to open multiple items. 
  For ex.:
  ```rust
  use crate::x::{y, z, m::n}; //opens y, z and n from m
  
  //We can also nest the base path if we wish to open it too,
  //use crate::x::{self, m::n}; opens crate::x; and crate::m::n;
  
  ```
- Glob [[Operator]]
  Used to open all items in an item.
  ```rust
  use crate::x::*;
  
  //opens all the public items in x in scope. Generally used in testing as in production projects this pollutes the scope.
  
  ```