- ``union``
  The same from the *C* or *C++* world. 
  That is, they are like [[Struct]]s but they can only have 1 field active at any time.
  
  For ex.:
  ```rust
  #[repr(C)]
  union MyUnion {
      f1: u32,
      f2: f32,
  }
  
  fn main() {
  let u = MyUnion { f1: 1 };
  let f = unsafe { u.f1 };
  }
  ```
  It is [[unsafe]] to access a union as Rust can't ensure which field is active at compile-time and accessing an inactive field is an error.
  
  ``#[repr(C)]`` [[Macro]] says ``Do what this language does`` with data layout and everything about the structure. By using ``C`` as a value to it, we ensure the data layout used in ``C`` [[ABI]] is used here. Data layout refers to how data is structured in the memory.