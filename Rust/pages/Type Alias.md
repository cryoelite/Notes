- Just like *typedef* in *C++*, we can have aliases for [[Data Type]]s in Rust.
  The syntax is much like [[Associated Type]]s.
  
  For ex.:
  ```rust
  fn main() {
   type yo= i32;
   let x: i32= 2;
   let y: yo = 4;
   println!("{}",x+y ); 
  } //works
  ```