- [[Module]]s theirselves, and items within them are *private* by default, which in Rust means, that parent modules can't access these modules or their items. We use the ``pub`` keyword to turn them to *public* and hence allow external modules to access them.
  This isn't just true for modules, but all items, [[Struct]]s/ [[Function]]s etc. either outside a [[Module]] or inside, in a [[Crate]]. Items in the open in a crate, are available directly from the crate's namespace.
  
  For ex.:
  ```rust
  mod x{
   pub mod y {
    pub fn yo(){}
   }
  }
  
  fn main() {
   crate::x::y::yo(); //works
  }
  ```
  * The only exception to the access modifiers are the top-level modules in a [[Crate]] Root, they can be accessed by the rest of the [[Crate]] Root's contents even if they are not public and the top-level modules are submodule to the [[Crate]] Root ``crate`` Module/the ``<crate-name>`` Module.
   
  * Even if a module is made *public*, its items must be made explicitly *public* to be accessible.
  
  * Parent modules can't access child modules and/or their contents if they are not public, but the child modules can access the sibling modules and items despite the access modifiers. Similarly, the items inside the modules can access each other if they are inside the same module without getting affected by the [[Access Modifier]]s.
  For ex.:
  ```rust
  mod x {
      pub mod y {
          pub fn ayo() {
              super::yo();
          }
      }
      fn yo() {}
  
      mod z {
          pub fn a() {}
      }
  
      mod k {
          pub fn b() {
              crate::x::z::a();
          }
      }
  
      pub mod m {
          pub fn c() {
              crate::x::k::b();
          }
      }
  }
  
  fn main() {
      crate::x::y::ayo();
      crate::x::m::c();
    	//works
    	//but crate::x::z::a(); won't work 
  } 
  
  ```
- All the items just need a ``pub`` before their definition to be made public.
  
  * [[Struct]] fields and [[Method]]s are private unless explicitly made public with pub. 
  For ex.:
  ```rust
  struct X {
   pub Y:i32,
  }
  ```
  
  * For [[Enum]]s , we just need to make the enum definition ``pub <enum-name>`` to make all its fields and [[Method]]s public.
-