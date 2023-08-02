- ``Pin``
  This is a special [[Data Type]] defined in ``std::pin`` [[Module]]. This type wraps [[Pointer]] types and either allows or prevents the values pointed by the pointer to be moved.
  
  All [[Pointer]] [[Data Type]]s implicitly implement ``Unpin`` [[Trait]], this means that if the Pin wraps the pointer, it will allow the value to be moved.
  
  But if the [[Pointer]] explicitly implements the ``!Unpin`` [[Trait]] (defined in ``std::marker`` as ``PhantomPinned``), then Pin prohibits the value from being moved.
  
  For ex.:
  ```rust
  use std::pin::Pin;
  use std::marker::PhantomPinned;
  
  #[derive(Debug)]
  struct Test {
      a: String,
      b: *const String,
      _marker: PhantomPinned,
  }
  
  
  impl Test {
      fn new(txt: &str) -> Self {
          Test {
              a: String::from(txt),
              b: std::ptr::null(),
              _marker: PhantomPinned, // This makes our type `!Unpin`
          }
      }
  
      fn init(self: Pin<&mut Self>) {
          let self_ptr: *const String = &self.a;
          let this = unsafe { self.get_unchecked_mut() };
          this.b = self_ptr;
      }
  
      fn a(self: Pin<&Self>) -> &str {
          &self.get_ref().a
      }
  
      fn b(self: Pin<&Self>) -> &String {
          assert!(!self.b.is_null(), "Test::b called without Test::init being called first");
          unsafe { &*(self.b) }
      }
  }
  
  //And now if we try to move
  
  pub fn main() {
      // test1 is safe to move before we initialize it
      let mut test1 = Test::new("test1");
      // Notice how we shadow `test1` to prevent it from being accessed again
      let mut test1 = unsafe { Pin::new_unchecked(&mut test1) };
      Test::init(test1.as_mut());
  
      let mut test2 = Test::new("test2");
      let mut test2 = unsafe { Pin::new_unchecked(&mut test2) };
      Test::init(test2.as_mut());
  
      println!("a: {}, b: {}", Test::a(test1.as_ref()), Test::b(test1.as_ref()));
      //std::mem::swap(test1.get_mut(), test2.get_mut()); //error
      println!("a: {}, b: {}", Test::a(test2.as_ref()), Test::b(test2.as_ref()));
  }
  ```
  Here, we have Pinned to the stack, meaning we create a Pin of a value on the stack. If we Pin a value on the heap like ``Pin<Box<T>>`` then it'd be heap pinning. Stack pinning is [[unsafe]].
  read more at [Docs](https://rust-lang.github.io/async-book/04_pinning/01_chapter.html).
- Primitives can be moved in and out of Pin, this is because they have no problem being moved around, so ``Pin<&mut u8>`` behaves the same as ``&mut u8``. Pin types can be defined simply with ``Pin<&T>``.
- Heap Pinning
  Meaning values on the heap won't be moved if they are pinned.
  
  For ex.:
  ```rust
  use std::pin::Pin;
  
  struct MyStruct {
      value: u32,
      _pin: PhantomPinned, //necessary, this tells the compiler this Struct implements !Unpin
  }
  
  fn main() {
      let mut my_struct: Pin<Box<MyStruct>> = Box::pin(MyStruct {
          value: 10,
          _pin: PhantomPinned,
      });
    //Box::pin returns a pinned type
  
      println!("{}", my_struct.value);
      
      //and to modify the value behind a Pinned Type
       unsafe {
          let mut_ref: Pin<&mut MyStruct> = Pin::as_mut(&mut my_struct);
          let mut_pinned: &mut MyStruct = Pin::get_unchecked_mut(mut_ref);
          mut_pinned.value = 32;
      }
      println!("{}", my_struct.value);
  }
  ```
- ``pin_utils``
  And other [[Crate]]s are external crates that simplify Pinning and provide [[Macro]]s that do the pinning and other things theirselves so we need to do is use the Pinned types.
  For ex.:
  ```rust
  use pin_utils::pin_mut;
  
  fn main() {
      let x= "yo";
      pin_mut!(x);
      let y= x;	 
   	//Here y’s type will be Pin<&mut &str>. It moves x into y and y is now wrapped in Pin’s safety. 
  }
  
  ```
-