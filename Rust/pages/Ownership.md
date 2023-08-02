- Rust is a systems programming language so it allows memory access like *C++*. All programs have to manage their memory usage, i.e. (de)allocating memory. Other languages either run a *GC* for the same  or leave it up-to the programmer (*C*). But in rust, a third method, [[Ownership]] is used. Using this method memory (de)allocation rules are checked right at compile time and the program doesn’t even compile if they aren’t followed. So rust has no *GC*, just a very guaranteeing memory model.
- Stack vs Heap
  *Stack* is a fixed size contiguous (adjacent, touching borders) segment of memory. This is a LIFO approach as data must be pushed and popped. 
  In a *heap*, the size isn’t fixed so when we want to store some data, the chunk is allocated and the address returned through a pointer. 
  Pushing to the *stack* is faster than to the *heap*, because the allocator has to allocate the space and do a few other things. 
  Same for reading. When we read from the *stack*, the entire *stack* is moved to the processor and then elements can be accessed very fastly. 
  
  However, since a *heap* is not a contiguous segment of memory, it can’t be passed all at once to the *L caches*. So the read is slower. 
  
  *Stack* and *heap* aren’t treated much differently in other languages but in Rust they are, and ownership rules help in understanding how Rust operates with them.
- Ownership Rules
  * Each value in Rust has an *owner*.
  * There can only be one owner at a time.
  * When the owner goes out of scope, the value will be dropped.
- The [[Copy or Move]] defines how ownership affects [[Variable]]s.
- Returning Ownership
  [[Function]]s can return ownership too. 
  For ex.:
  ```rust
  fn main() {
  let x = String::from("yo");
  let y = give_and_get_ownership(x); //y gets the ownership from the function
  
  }
  
  fn give_and_get_ownership(x: String)-> String {
   x //x has the ownership here 
  }
  
  ```
-
-