- ``extern``
  Rust also supports *FFI* (Foreign Function Interface) where Rust code can call a function written in another language, and other languages can call Rust code as well. We declare these external functions inside Rust within extern blocks and then call the functions using [[unsafe]] blocks. They are marked unsafe because other languages don’t employ Rust’s Safety system and hence they are always unsafe in Rust’s eyes.
  We do so with the ``extern "<ABI name>" { <function declarations> }``.
  
  For ex.:
  ```rust
  extern "C" {
      fn abs(input: i32) -> i32;
  }
  
  fn main() {
      unsafe {
          println!("Absolute value of -3 according to C: {}", abs(-3));
      }
  }
  ``` 
  Here ``extern "C"`` defines the external [[ABI]] (Application Binary Interface) rust uses to locate the provided [[Function]]s in. ``abs()`` is part of *C*'s Standard Library so it will find it directly. 
  
  Similarly, rust can allow other languages to pick up its [[Function]]s. We do so using almost the same syntax.
  
  For ex.:
  ```rust
  #[no_mangle]
  pub extern "C" fn call_from_c() {
      println!("Just called a Rust function from C!");
  }
  ```
  ``#[no_mangle]`` is a [[Macro]] which disables name mangling for the given [[Function]]/ [[Method]], ignoring the optimization to reduce the binary size by a bit but allowing the name to remain the same and hence be picked up by external linkers easily. This isn't [[unsafe]]. 
  Compiling this generates a ``.pdb`` file in the ``target/`` directory which can be linked with a *C* program and hence be picked up there.