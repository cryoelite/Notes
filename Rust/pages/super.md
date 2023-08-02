- This keyword can be only at the starting of a path, when used it goes one up in the hierarchy of modules where the current [[Module]] / [[Crate]] is the current position. 
  For ex.:
  ```rust
  mod x {
    pub mod y {
        pub mod z {
            pub mod k {
                pub fn aye() {
                    super::super::super::yo();
                }
            }
        }
    }
  - pub fn yo() {}
  }
  - fn main() {
    crate::x::y::z::k::aye(); //works
  }
  ```
- Similarly, by using ``use super::*`` with [[use]], we essentially open an entire parent namespace into the current namespace.
-