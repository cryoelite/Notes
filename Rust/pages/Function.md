- Just like other languages. 
  However, functions by theirselves are not a data type and the compiler might inline them in some cases.
  
  Unlike *C++*, Functions can be defined anywhere, but they must be accessible by the caller’s scope.
  Basic syntax:
  ``fn <function name>(<params>)-> <return type> { 
  …  
  return <value>;
  }``
  For ex.:
  ```rust
  fn another_function(x: i32, unit_label: char) {
   println!("The value of x is: {x} and char is {unit_label}");
  }
  ```
  * Functions must define the types of their params in rust.
- Return
  Function [[Scope]] blocks follow the same return rules as any scope, also meaning that they implicitly return the [[Unit Type]] if no other type is defined . But they can also use the ``return`` keyword, which only works with functions. A return keyword immediately returns a result from a function.
  For ex.:
  ```rust
  fn yo()->i32 {
   return 2; //ok
  }
  
  fn ayo()->i32 {
   2 //also ok but '2;' won't be as this is a statement.
  }
  ```
- The ``fn`` [[Data Type]] and the ``Fn`` [[Trait]]
  Functions in Rust aren't a data type, but there's a type associated with them, the ``fn`` type. This is called a *Function Pointer* and is a [[Pointer]] type that can point to function definitions on the stack.
  This allows us to pass functions around as arguments.
  
  Syntax:
  ``
  fn(<param type 1>, <param type n>) -> <return> type 
  ``
  And that's how we define these types.
  
  For ex.:
  ```rust
  fn yo(x: i32) -> f64 {
      2.0
  }
  
  fn na(x: fn(i32) -> f64) {
      x(2);
  }
  
  fn main() {
      let a: fn(i32) -> f64 = yo;
      na(a); //works
  }
  ```
  
  * ``Fn`` [[Trait]]: Part of the 3 traits, ``Fn``, ``FnMut`` and ``FnOnce``. Function pointers implement all 3 of these traits that [[Closure]]s also use. 
  Meaning, we can use [[Generic Type]]s that constrain to [[Closure]]s with these traits, and also pass them Function Pointers. 
  
  For ex.:
  ```rust
      enum Status {
          Value(u32),
          Stop,
      }
  
  
  fn main() {
      let list_of_statuses: Vec<Status> = (0u32..20).map(Status::Value).collect(); //works
  }
  ```
  Here, we can create a [[Vector]] of [[Enum]]s using a [[Range]] and passing a function to an argument that accepts [[Closure]]s. 
  
  * [[Type Alias]]: We can use them with Function Pointers like so
  ```rust
  fn yo(x:i32, y:i32) {} 
  fn main() {
  type Binop = fn(i32, i32) -> ();
   let x: Binop = yo;
   x(2,4);//ok
  }
  ```
-