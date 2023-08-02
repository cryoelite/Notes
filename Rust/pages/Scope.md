- A scope is the range within a program for which an item is valid. There's a single global scope and many local scopes in any program. A new custom scope can be created with {…}.
  title:: Scope
  
  For ex.:
  ```rust
  fn xyz() {
  	{
  		let y:i32=2;
  	}
  	println!(y); //will throw error as y is not in the same scope, it is also already dropped.
  }
  ```
- Return
  A scope block always returns a value but that value is discarded if there is no receiver or there's no semicolon at the scope's end.
  For ex.:
  ```rust
  let x = {
  2
  }; //works
  ```
  Scope blocks can return a custom value if there's an expression as the last instruction in them otherwise they return the [[Unit Type]] as default.
- Variable Scope
  
  For ex.:
  ```rust
  {
  	let s = “aa”; //immutable string
  }
  ```
  In this example, ``s`` isn’t valid before being declared and is only valid till the end of the scope, after which it goes out of scope and is hence disposed. 
  
  Variables also go out of scope after their last usage in the code. But this also means, that if a variable is used at the very end of the program then it will remain in scope until then. This ability of a compiler to tell if a variable is no longer is being used before the end of the scope block is called *Non-Lexical Lifetimes (NLL)*.
  
  * Just like RAII (Resource Acquisition Is Initialization) works in C++, the values on heap in rust are deallocated when the scope finishes. 
  For ex.:
  ```rust
  {
  let s= String::from(“yo”)  //mutable string
  }
  ```
  The ``s`` loses its value after the scope is finished. But this [[String]] is on the heap, so it actually has to deallocate the memory allocated for the strings. For this, rust requires the type to implement *drop* function. This function is called at the end of the scope automatically and must clear the memory.
  
  * [[Variable]]s are deallocated in the reverse order of their creation.