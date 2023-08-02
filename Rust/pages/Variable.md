- Variables in rust are immutable by default, meaning their value can’t be updated after initializing them once.
  For ex.:
  ```rust
  let x: i32;
  x=6; //works
  x=7 //error.
  ```
  Declaration syntax is simply ``<let/const> <optional mut> <varname>: <var type> = <optional value>``.
- NNBD
  Rust is non-nullable by default, infact even the concept of ``null`` doesn’t exist in Rust. Every variable must either have a value or a type defined at-least and every variable must be assigned a value before being used. All of this is checked during compile time itself, and after compilation a variable must have a type. 
  For ex.:
  ```rust
  let y; // error as y doesn’t have a type
  let z: i32; //ok
  let x;
  x=2; //ok and x’s type is inferred as i32.
  let a:i32;
  let b= a; //error as a doesn’t have a value.
  let m=2; //ok and m’s type is inferred as i32.
  ```
- Mutable Variables
  ``let mut <varname>:<var type>;``
  
  Whilst mutable variables allow reassignment of value, they forbade the reassignment of types so the variable can only get a value of the same type.
- Constant
  ``const <varname>:<type>= <value>;``
  
  Unlike variables created with ``let``, ``const`` vars are always immutable. Another feature of const is that it allows constant expressions like C++. So 
  ```rust
  const ABC: u32 = 2*2*3+3; //works
  ```
- Shadowing
  Allowed, even in the same [[Scope]].
  We can create a new variable with the same name as a previous one. The new variable ‘overshadows’ the older one and will be accessed when called later in the scope instead of the variable being shadowed. The overshadowing variable won’t be accessed if not in present or parent [[Scope]]. This allows us to use some cool things like
  ```rust
  let spaces = "   ";
  let spaces = spaces.len();
  ```
  The first spaces is of type [[String]] while the latter is a [[Number]] [[Data Type]] .
- Data types
  Rust is a statically typed language, meaning the type of values must be resolved at compile time. Explicitly specifying a type isn’t required as the compiler can infer the type of a variable automatically. It is a bit more powerful than other languages as type can be inferred even after the declaration of variable. However, types must be specified when multiple types are possible from an expression. 
  For ex.
  ```rust
  #![allow(unused)]
  fn main() {
  let guess: u32 = "42".parse().expect("Not a number!"); //works
  }
  ```
  If we remove the type then it throws an [[Error]]. This is to say, the parse() [[Function]] picks up the 
   type given to the variable. The compiler passes the type given to the receiver to the function for its [[Generic Type]]s, so parse knows it has to work for [[Number]] [[Data Type]] ``u32``.
- *Static Variable*s
  Rust allows global variables, i.e., variables in the global [[Scope]]. They have be to declared with the ``static`` keyword. However, using them isn't a best practice as if we use them with [[Thread]]s, multiple threads accessing the same Global variables can cause *Data Race*.
  Static variables can only have static [[Lifetime]]s and hence can only have values given at compile-time.
  
  
  Syntax:
  ``
  static <optional mut> <varname>:<vartype>= <value>;
  ``
  For ex.
  ```rust
  static HELLO_WORLD: &str = "Hello, world!";
  static mut YO: u32=0; 
  fn main() {
      println!("name is: {}", HELLO_WORLD);
      unsafe {
      YO= 2;
       println!("yo: {}", YO);
    }
  }
  ```
  It is in [[Default Linter Rule]]s to define static variables in *SCREAMING_SNAKE_CASE*.
  Accessing/Mutating static variables is [[unsafe]] as it could lead to *Data Race*.
  
  * ``static`` variables are different from ``const`` variables as they have a fixed address throughout the runtime, however const variables depend on their [[Scope]] and can have any address as they are initialized/destroyed each time the scope starts/ends.
-