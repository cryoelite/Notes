alias:: Empty Type

- A [[Data Type]] represented with ``!`` and has no values. 
  So it is kind of like *void* in *C++*, but not exactly, as blocks that denote return type ``!`` never return, so the control flow never actually returns back to the caller.
  ``fn yo() -> ! {…}``
  Although a syntax error, it says [[Function]] ``yo`` returns never. This type of function is called a *Diverging* [[Function]] as control never returns from this function. [[Scope]] blocks always return something, or [[Unit Type]] if nothing explicitly.
  
  The never type has some uses, namely, it tells Rust that a given piece of code never returns hence the only possible return types are others. 
  
  For ex.:
  ```rust
  fn main(){
  let x= Some(2);
  /*
  let y= match x {
  	Some(_) => 2,
  	None => “ye”
  };
  */
  //is an error as match needs to return the same type for all arms, however,
  let y= match x {
  	Some(_) => 2,
  	None => panic!(…)
   };
  } // works!
  ```
  The never type can be coerced into any type.
  
  For ex.:
  ```rust
  let x: ! = panic!();
  // Can be coerced into any type.
  let y: u32 = x;
  ```
  
  Rust knows if the never type is returned then the control flow has changed and hence it doesn’t need to worry about the arm’s type.
  
  * ``continue`` also returns a never type.
-