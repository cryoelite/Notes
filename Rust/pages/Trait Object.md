- Trait Objects are much like [[Generic Type]]s where we define a [[Trait]] and instead of providing a concrete type at compile-time we provide it at runtime. So rust knows at compile-time only 2 things, that it should wait for the types till runtime and at compile time just ensure that any provided types do implement the given trait. 
  However, as we know, Rust requires a fixed size at compile time for every definition, this is why we have to make use of [[Pointer]]s. And the pointers theirselves need to know the size of the type, they can use ``dyn`` keyword which allows [[Dynamically Sized Type]]s. 
  Finally we get a [[Data Type]] definition that looks like ``Box<dyn someTrait>``.
  
  For ex.:
  ```rust
  fn main() {
   /*
  let bag = Bag {
          stuff: vec![Box::new(1i32), Box::new(2u32)], 
      }; // //would be an error, because Box<T> for Bag resolves to i32, but we provide a u32 for the next element. 
  */
      let bigger_bag = BigBag {
          stuff: vec![Box::new(1i32), Box::new(2u32)],
      }; //works
  }
   
  struct Bag<T: Tiffin> {
      stuff: Vec<Box<T>>,
  }
   
  struct BigBag {
      stuff: Vec<Box<dyn Tiffin>>,
  }
   
  trait Tiffin {}
   
  impl Tiffin for i32 {}
  impl Tiffin for u32 {}
  
  ```
- With Trait Objects, Rust must use [[Dynamic Dispatch]].
  This is in contrast to [[Static Dispatch]], where the compiler knows at compile-time all the methods and addresses to call. Here, it doesn't and instead internal pointers are used to denote where the actual method is at runtime. This prevents some optimizations, and also incurs a small lookup cost at runtime. 
  This is why Trait Objects must be used sparingly.