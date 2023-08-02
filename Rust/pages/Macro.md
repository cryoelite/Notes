- Macros are special symbols that expand to more text in the source code.
  Just like *C++* has *#define* which can define a symbol which is expanded before compilation phase and modifies the code in the translation unit, Rust has macros which are symbols that get replaced with other text in the equivalent of translation unit in rust (not sure what it is [[TODO]]).
  
  This means, we can use macros to define [[Function]]s and other code in a single place, and using the same symbol in different projects can allow us to skip writing a lot of boilerplate. 
  The only cost associated with Macros is the less simpler syntax and linter assistance making them harder to maintain and debug. As they use a slightly different syntax than normal Rust.
  
  * Unlike [[Function]]s, Macros must be defined/imported into a [[Scope]] before being used.
  
  * Rust has 2 main type, but 4 different sub-types of Macros
  Declarative macros: Implemented with [[macro_rules!]]
  Procedural macros: They include [[Custom Derive Macro]]s, [[Macro Attribute]]s and [[Function Macro]]s.
- Procedural Macros
  These macros must be defined in their own special [[Crate]]s and then added as a dependency to the crate that needs to use them.
  
  For ex.:
  ```rust
  use proc_macro;
  
  #[some_attribute]
  pub fn some_name(input: TokenStream) -> TokenStream {
  }
  ```
  ``TokenStream`` is a [[Data Type]] that is defined by Rust itself and holds the source code in text.
  
  * [[Procedural Macro Crate]]s are different from normal crates.
  * [[Custom Derive Macro]]s are macros like ``#[something_derive]`` applied only on either [[Struct]]s or [[Enum]]s. They need to have ``derive`` at the end of their name due to how [[Procedural Macro Crate]]s are defined, this is a known technical limitation.
  
  These macros are defined with the ``#[proc_macro_derive]`` [[Macro Attribute]]. 
  
  For ex.:
  ```rust
  use proc_macro::TokenStream;
  use quote::quote;
  use syn;
  
  #[proc_macro_derive(HelloMacro)]
  pub fn hello_macro_derive(input: TokenStream) -> TokenStream {
      // Construct a representation of Rust code as a syntax tree
      // that we can manipulate
      let ast = syn::parse(input).unwrap();
  
      // Build the trait implementation
      impl_hello_macro(&ast)
  }
  ```
  * [[Macro Attribute]]s are like Custom Derive Macros but they don't have the limitation of ``derive`` name at their end and can be applied to other items such as [[Function]]s too. They take the whole item, such as a function, modify it, and return the newly modified function back. 
  For ex.:
  ```rust
  #[route(GET, "/")]
  fn index() { }
  ```
  These macros are defined with ``#[proc_macro_attribute]``
  For ex.:
  ```rust
  #[proc_macro_attribute]
  pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {}
  ```
  
  * [[Function Macro]]s define macros that look like functions and work kind of like [[macro_rules!]]. They can take any number of arguments and accept ``TokenStream`` as parameter so they take the code and modify it and return it, just like [[macro_rules!]] but without any pattern matching involved.
  The syntax to call them is ``<some proc macro name>!(<some code>)`` which is slightly different from [[macro_rules!]].
  
  These macros are defined with the ``#[proc_macro]`` syntax
  
  For ex.:
  ```rust
  pub fn sql(input: TokenStream) -> TokenStream { }
  ```
-
- Print and debug
  ``print!()`` is a [[macro_rules!]] macro that prints text to stdout.
  ``println!()`` is its newline ending variant. 
  For ex.:
  ```rust
  prinln!("{} yoo {}",2,4); 
  ```
  Here the given string is printed and all ``{ }`` in it indicate scalar/printable [[Data Type]]s which are provided as variable number of args to the macro.
  
  * Output Format:
  We can specify output formats inside the ``{ }``.
  
  * Debug Printing:
  If we use ``{:?}``  then it prints the given object in debug mode, but requires the [[Data Type]] to implement the ``Debug`` [[Trait]], this is a derivable trait.
  
  For ex.:
  ```rust
  #[derive(Debug)]
  struct X {}
  
  fn main() {
  let x = X{};
  println!("{:?}", X);
  //and it will debug print it. 
  
  println!("{:#?}", x); //debug prints the instance.
  ```
  
  Alternatively, we can instead use ``dbg!(<value>)``. This macro returns the [[Ownership]] of expressions so having it there or not there is the same thing to Rust.
  For ex.:
  ```rust
  let a=dbg!(2*4); //a will be 8 and it will print 2 * 4 = 8
  ```
  ``dbg!`` prints to stderr.
  
  * ``eprintln!(...)``: Just like dbg!, prints stuff to stderr.
- Common Macros
  * ``println!(<>)``: Prints stuff