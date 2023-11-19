- Fast, multi-threaded, type-safe & memory efficient language that eliminates most bugs at compile-time. It also bridges the gap between a low-level language and a high-level language with its compiler. Uses [[rustup]] to maintain versions on platforms and [[cargo]] which is its package manager. 
  
  Its mascot is called *Ferris*, a red crusty crab and its users, *Rustaceans*.
- [[Default Linter Rule]]
- Lines need to be ended with ;
- Rust files end in the extension ``.rs``
- Install rust by installing [[rustup]], here's a quick reference [docs book](https://doc.rust-lang.org/book/ch01-01-installation.html).
- [[rustup]] also introduces [[Cargo]]: Rust's Package manager, [[rustc]]: Rust's compiler and [[rustup]] itself which manages the updating of both. These are all available to the CLI, but if not, add them to the PATH as they are executables. 
  
  The ``rls`` (Rust Language Server) and ``rust-analyzer`` (Provides implementation of the rls) are generally automatically installed with rustup, but if they are not (resulting in failed code-completion etc.) we can manually install them with 
  ``rustup component add rls`` and ``rustup component add rust-analyzer``
- A rust project needs a [[cargo.toml]] file at the bare minimum.
- Main Entrypoint to any Rust executable
  ``fn main() {…}``
- [[Variable]]
- [[Expression]]
- Statements vs Expressions
  Statements are instructions that perform some actions and don’t return a value. Expressions are the same but return a value. [[Function]]s definitions in Rust are statements. But calling a function is an 
   expression. This distinction is not as visible in other languages but in rust,
  ```rust
  let x = (let y = 6); 
  ```
  is an error as ‘let’ is a statement and hence doesn’t return anything. 
  However, a custom [[Scope]] block is an expression. Every scope block in Rust returns something.
  
  For ex.:
  ```rust
  let y = {
   let x= 1;
   x+1 //Here x+1 is an expression, as it is evaluated then the value is being returned
  }; //Note the semicolon, it is required for custom scope blocks which need to return a value
  println!(“{}”,y); //prints 2
  ```
- [[Comment]]
- [[Control Flow]]
- [[Struct]]
- [[Module System]]
- [[Testing Framework]]
- Reading environment args and environment variables
  To do so,
  ```rust
  use std::env;
  
  fn main() {
  let values: Vec<String>= env::args().collect();
  //puts env args as String objects into values.
  let val= env::var("MYENV_VAR");
  let isOk= val.is_ok(); 
  //reads the given env var and returns Result in val. .is_ok() returns true if the Result is Ok().
  
  }
  ```
  To pass env args,
  
  ``cargo run arg1 arg2``to [[Cargo]]. 
  
  Sometimes env args can fail due to Unicode issues. To solve that we can use ``.args_os()`` instead, this returns values of ``OsString`` [[Data Type]].
- [[File]]
- *Functional Programming*
  Rust borrows some concepts from FP languages. An FP language generally
  sees [[Function]]s as values.
  These concepts in Rust are similar to what FP languages have.
  * [[Closure]]
  * [[Iterator]]
- [[Concurrency or Parallelism]]
- *Object Oriented Programming*
  Rust isn't strictly OOP in the sense languages like *C++* are. Instead, it focuses on bringing all important aspects from OOP languages. By definition, OOP languages should provide *objects* that are packages of data and methods, and the methods operate on said data in their own packages. Rust does this using ``impl`` blocks and [[Struct]]/ [[Enum]]s. Encapsulation is handled through [[Access Modifier]]s.
  Inheritance isn't wholly used, but it's most important aspects, abstract classes, which is done using [[Trait]]s and their implementers, and Polymorphism where an abstract type is used and at runtime replaced with a more concrete type, which is handled through [[Trait Object]]s, are covered.
- *Raw Identifier*s
  Rust allows naming items such as [[Function]]s, [[Variable]]s etc. with the same name as keywords, but requires a special prefix to be used, ``r#``. This is applied to both the definition and the call-site.
  
  For ex.:
  ```rust
  fn r#match(){}
  
  fn yo() {}
  
  fn main() {
   r#match(); //works
   r#yo(); //also works
  }
  ```
  
  The benefit is that even if the definition isn't using ``r#``, the call-site would still work. It would call the ``r#`` version and if it doesn't exist, it'd call the normal version. Both can't be defined together in the same [[Scope]] so there's no ambiguity to which one will be called.
-