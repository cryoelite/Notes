- A reference in Rust, is much like a Reference in *C++*, it doesn't have take [[Ownership]], nor does it [[Copy or Move]], it simply stores a pointer to the actual value and passes it around. This is called [[Borrow]]ing. 
  
  For ex.:
  ```rust
  fn main() {
      let s1 = String::from("hello");
      let p= &s1; //p's type is inferred as &String
      sec(&s1);
       println!(&s1); //ok
  }
   
  fn sec(s: &String) {
      println!("{}",s);
  }
  ```
  It looks like so
  ![Three tables: the table for s contains only a pointer to the table for s1. The table for s1 contains the stack data for s1 and points to the string data on the heap.](../assets/image_1689029258981_0.png){:width 500 :height 500}
  
  Here, we do [[Borrow]]. `&` is the reference [[Operator]], it copies a reference to a value. On the contrary, we have the ``*`` [[Operator]] which is the *dereference* Operator and it reads the value at the given reference.
  
  So with `&s1` we are borrowing the value behind ``s1``, which is to say the [[Function]] `sec` or `p` are not going to own the value ``hello``, simply have access to it. [[Borrow]] is a synonym to ‘referencing the value’ from other languages. 
  Borrowed values cannot be modified, so ``sec`` can’t modify ``s`` as it is a reference.
  
  Since the [[Ownership]] of `s1` isn't given to `p` or `s`, the value inside `s1` will have a [[Lifetime]] of `s1`. Which is to say, the value inside isn't dropped when the `sec` [[Function]] is over and `s` goes out of [[Scope]].
- The [[Data Type]] of references is not the same as the type of the values they refer, it is a Reference Data Type which is either ``&<T>`` or ``&mut <T>``.
- Just like [[Variable]]s, references can be *immutable* or *mutable*
  An immutable reference is retrieved with the ``&`` [[Operator]] and the [[Data Type]] of the Reference type is ``&<T>``.
  
  A mutable reference is retrieved with ``& mut`` and the data type is ``& mut<T>``.
  
  Immutable references don't allow any modification on the value, only reads. A mutable reference does allow modification but it is very strict about that.
  For ex.:
  ```rust
  fn main() {
      let mut s = String::from("hello");
      sec(&mut s);
  }
   
  fn sec(s: &mut String) {
      s.push_str("yo");
      println!("{}",s);
  } //ok
  ```
  Mutable references require ``mut`` keyword on 3 things, 
  1. The [[Variable]] which will pass the mutable reference, this need to be mutable to allow mutable references being taken of it. An immutable variable can't give out a mutable reference.
  2. The reference itself must be retrieved with the ``& mut`` keyword.
  3. The [[Data Type]] of the mutable reference will be ``& mut T``. 
  
  * If a value has a mutable reference to it, then it can only have the mutable reference and no other reference to it can be active at that time.
  For ex.:
  ```rust
  let mut s= String::From("yo");
  let p = &mut s;
  let x= &mut s; //ok, there are 2 mutable refs but NLL ensures p is already dropped
  ```
  Works, however
  ```rust
  let mut s= String::From("yo");
  let p = &mut s;
  let x= &mut s; 
  
  println!(“{} {}”, p,x); //error
  ```
  is an [[Error]] as both mutable references are alive at the same time.
  
  These restrictions ensures that an (im)mutable reference is always reading value at the correct address.
  * There can't be a mutable reference when an immutable reference exists for a value.
  
  * There can be any number of immutable references, and all of them are valid even all at once.
- Dangling pointers are avoided at compile-time itself in Rust through the use of [[Lifetime]]s.
  For ex.:
  ```rust
  fn main(){
    let p = sec();
  }
  
  fn sec() -> &String{
      let mut s = String::from("hello");
      
      &s 
  }
  ```
  is an [[Error]] as even though the reference type is returned, the value itself can been already dropped. Rust knows this with the use of Lifetimes.
- [[Slice]]
- Do recall from *C++* that the size of a reference/pointer is fixed and it may be larger than a primitive type like [[Number]] int32 so don’t use refs for primitives. They do make things easier though.
  
  As can be seen here
  ![image.png](../assets/image_1689031775031_0.png)
- All References have an associated [[Lifetime]] with them.
- Referencing and Dereferencing
  
  A [[Data Type]] ``&T`` simply stores value that is a memory address, so a ``&T`` value will never be equal/comparable to a value of type ``T``. 
  
  ``T`` -> ``&T`` : We do this with ``<value>``-> ``&<value>``. The ``&`` [[Operator]] returns the memory address and a type ``&T`` can store the address.
  
  ``&T`` -> ``T`` : We do this with ``<value>`` -> ``*<value>`` where the ``*`` [[Operator]] is called the dereference operator. It accesses the value at the address pointed to by a ``&T`` value and returns a value of type ``T``.  
  
  * The memory address has a single type, i32 for 32-bit systems and i64 for 64-bit systems. But by having a different name for ``&T``, Rust ensures no type-checking is required when converting references/derefences.
  
  * Dereference of non-primitive [[Data Type]]s requires implementation of the ``Deref`` [[Trait]].
-
-