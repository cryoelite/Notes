- ``mod``
  A module allows us to group and organize code within a [[Crate]] for easy reusability and readability. It also puts [[Access Modifier]]s on its items, which is private by default. Module definitions need to be at the top of an ``.rs`` file.
  
  Syntax:
  ``
  <access modifier> mod <name> {
   //stuff
  } 
  
  //or
  <access modifier> mod <name>;
  ``
  
  For ex.:
  In main.rs,
  ```rust
  mod x { //module names must be in snake-case
   pub fn yo() {}
  }
  
  fn main() {
   crate::x::yo(); //to access it
  }
  ```
- Modules can be defined right away, which is done by having a ``{ }`` block after them, if we instead have a ``;`` semicolon after the definition, then the module is looked for in the places defined in [[Module System]]. Either way, this *imports* a module into our [[Crate]] and all the module's contents with the appropriate [[Access Modifier]] can be accessed by the crate.
- Modules can nest other modules as well.
- Path to an item in a module
  The path to an item in a starts from the [[Crate]] Root to the module in it, then its submodules and so on.
  
  For ex.:
  For this module definition in main.rs
  ```rust
  mod x {
   mod y { 
   fn yo(){}
   }
   pub mod z { 
   pub fn yo(){}
   }
  }
  ```
  Rust would need to get
  ```rust
  crate::x::z::yo();
  ```
  
  ``crate`` is the [[Crate]] Root, so here it is accessing itself. 
  
  * Absolute vs. Relative Path
  Whilst the absolute path starts from the [[Crate]] Root, a relative path is used when the item is within this crate.
  For ex.:
  In ``main.rs``
  ```rust
  mod front_of_house {
      pub mod hosting {
          pub fn add_to_waitlist() {}
      }
  }
  
  pub fn eat_at_restaurant() {
      // Absolute path
      crate::front_of_house::hosting::add_to_waitlist();
  
      // Relative path
      front_of_house::hosting::add_to_waitlist();
  }
  ```
  The relative path starts from the current crate.
  
  * [[super]]
  
  
  
  * [[use]]
  This keyword basically opens a namespace.
- Modules defined in separate files
  As defined in [[Module System]], a module can either have its definition at the time of declaration
  ``mod <name> {...}`` or it can have a semicolon ``mod <name>;``, in which case the module's definition is looked for in some specific places. 
  This allows us to break our project down into multiple modules and hence multiple files.
  
  For ex.:
  In ``main.rs`` or ``lib.rs``,
  ```rust
  mod modA;
  ```
  
  Create ``src/modA.rs`` or ``src/modA/mod.rs`` with content
  ```rust
  mod modA {
  	pub modB{
  		pub fnA(){}	
   }
  }
  
  pub mod modC;
  ```
  
  Then create ``src/modA/modC.rs`` or ``src/modA/modC/mod.rs`` 
  ```rust
  pub mod modC {
   pub fnA() {}
  }
  ```
  
  While using ``mod.rs`` seems more intuitive/organized, the same filenames make it confusing for working in IDEs but to the compiler it makes no difference and we can even mix both folder structures for a project. However each module must only be declared using any one method not both.
-