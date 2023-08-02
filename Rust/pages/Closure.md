- Anonymous [[Function]]s
  Can be stored, passed around, capture the values of their context and even be ran in different contexts.
  For ex.:
  ```rust
  fn yo()-> i32 {
   2
  }
  
  fn main() {
  let someOption= Option::Some("ya");
  let value=someOption.unwrap_or_else(|| yo());
  }
  ```
  Here the body of the closure calls ``yo()`` and returns its return value when the [[Unwrap]] gets a ``None``.
- Syntax
  Closely resembles that of a [[Function]] but closures can omit most of the explicit [[Data Type]] definitions as they are able to infer them from the context.
  Closures are short-lived, context-dependent and narrow-use [[Function]]s
  
  ``
  let <variable name> = <optional move> |<param name>: <optional param type>, ... , <more params>| -> <optional return type> { <body that returns the value of the return type> }
  ``
  If there's a single statement/expression in the closure we can even omit the [[Scope]] Blocks.
  Then we can call a closure with ``<variable name>()``, just like [[Function]]s. 
  
  For ex.:
  ```rust
  fn  add_function  (x: u32) -> u32 { x + 1 } //A function
  
  fn main() {
  let add_closure1 = |x: u32| -> u32 { x + 1 }; //A verbose closure
  let add_closure2 = |x|             { x + 1 }; // Return type and param type is inferred
  let add_closure3 = |x|               x + 1  ; // Single expression so the block can be omitted
  //All these are valid definitions
  add_closure2(2); //required
  add_closure3(4);  //required
  }
  ```
  As we can see, unlike functions, closures can even infer the return types. However, for the parameter's [[Data Type]]s, we need to call the closures at-least once otherwise the compiler won't be able to infer the type of the params, just like [[Variable]] declarations. This is also to say, closure params aren't dynamically typed, they are statically typed and hence they must have a single concrete type, which the Rust compiler can infer if we call them once.
- Capturing [[Variable]]s
  Closures can capture variables, but unlike *C++* where the captured variables need to be explicitly defined, closures automatically capture them if their body uses them and they do so in 3 ways, just like [[Function]]s. They either do immutable [[Borrow]] or mutable [[Borrow]] or take [[Ownership]].
  
  For ex.:
  ```rust
  fn main() {
      let mut list = vec![1, 2, 3];
      let x = || println!("{}", list[0]); //borrows list immutably.
      x();
      let mut y = || list.push(4); //borrows list mutably
      y();
  }
  ```
  The [[Variable]]s are borrowed from the point of closure creation till the last time the closure is invoked (*Nox-Lexical Lifetimes* [[Lifetime]]).
  
  * We can force the closure to take [[Ownership]] of all captured variables with the ``move`` keyword making [[Copy or Move]] always move the captured values.
  
  For ex.:
  
  ```rust
  fn main() {
      let mut list = vec![1, 2, 3];
      let mut y = move || list.push(4); //moves list's ownership to the closure
      y();
  }
  ```
  
  This is sometimes required, such as in [[Thread]]s
  For ex.:
  ```rust
  use std::thread;
  
  fn main() {
      let list = vec![1, 2, 3];
      println!("Before defining closure: {:?}", list);
  
      thread::spawn(move || println!("From thread: {:?}", list))
          .join()
          .unwrap();
  }
  ```
  Here, since the closure runs in a separate thread, it is not guaranteed if the main thread or the child thread will finish first, but if main has the ownership and it dies first then the ``list`` will be deallocated rendering an immutable reference passed to the child thread as a dangling [[Reference Type]]. To avoid this, rust compiler itself ensures we ``move`` the captured variables.
  
  A closure can then ``move`` the variable to another closure or [[Function]] or etc passing the [[Ownership]] to something else. This is called moving a variable outside a closure.
- Capture [[Trait]]s and passing closures around
  All closures implement some traits depending on what they do with their captured [[Variable]]s. 
  There are 4 cases, 
  
  * A closure captures a variable but doesn't mutate it, nor moves it outside.
  * Mutates the value.
  * Moves it outside.
  * Doesn't capture any variable, so no moving outside nor mutating.
  
  Based on these cases, it automatically implements 1 or more of these traits
  
  * ``FnOnce``: Applies to all closures that can be called at-least once. All closures implement this. Ones that move captured references outside their body implement only this because they can be called only once as they move the references outside so they can’t be used again.
  
  * ``FnMut``: Applies to closures that mutate references but don’t move them outside.
  
  * ``Fn``: Applies to closures that don’t mutate references and don’t move references outside. It is also applied to closures that don’t capture anything.
  
  We can then use these traits with the [[Generic Type]]s too. The type of [[Function]]s/ [[Method]]s and Closures is ``( )`` for using with Generics and they also automatically implement these traits. This is why we can pass a function/method to params that accept a closure.
  For ex.:
  ```rust
  fn yo<T>(a: T) -> String 
  where
      T: Fn() -> i32,  //defining the type of T as function with (), then the trait with Fn and return type of the closure/function passed to T
  {
      a();
      return String::from("ya");
  }
  fn ya() -> i32 {
      return 2;
  }
  
  fn yo2<T>(a: T) -> String
  where
      T: Fn() -> i32 + FnMut() -> i32, //We use this syntax for multiple Fn traits.
  {
      a();
      return String::from("ya");
  }
  
  
  fn main() {
      let x = || 2;
      yo(x); //works
      yo(ya); //also works!
      yo2(x); //works too
  }
  
  ```
- Returning Closures
  Closures are represented by [[Trait]]s, which is why they are [[DST]]s. 
  So to return a closure we need to use [[Trait Object]]s.
  
  For ex.:
  ```rust
  fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
      Box::new(|x| x + 1)
  } //works
  ```