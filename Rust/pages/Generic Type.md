- Just like other langs, rust supports Generics. 
  They are simply abstract [[Data Type]]s that act as stand-ins for more concrete types defined elsewhere. The benefit is that we get a type definition that allows multiple [[Data Type]]s in the same place. 
  Rust's powerful compiler requires us to only use features allowed by the type definition, through [[Trait]]s. 
  
  For ex.: 
  ```rust
  fn largest<T>(list: &[T]) -> &T {
      let mut largest = &list[0];
  
      for item in list {
          if item > largest {
              largest = item;
          }
      }
  
      largest
  }
  
  fn main() {
       let number_list = vec![34, 50, 25, 100, 65];
  
      let result = largest(&number_list); //Rust can infer the correct type for T automatically
  }
  ```
  Whilst a correct example of a Generic Type, fails to compile. This is because it is not guaranteed that every concrete type that will replace T will implement the ``>`` [[Operator]] (greater than).
  Rust allows us to restrict the usage of items like ``largest`` [[Function]] here to only types that will implement this behavior, this means the call-site will fail to call the function, to do so we use [[Trait]]s, so here we make the function like so
  
  ```rust
  fn largest<T: PartialOrd>(list: &[T]) -> &T {
      let mut largest = &list[0];
  
      for item in list {
          if item > largest {
              largest = item;
          }
      }
  
      largest
  }
  ```
  
  And this says that concrete types for T must implement the ``std::cmp::PartialOrd`` [[Trait]].
- Generics can be used anywhere we have any definition, be it [[Function]]s, [[Struct]]s, [[Enum]]s, [[Method]]s, [[Trait]]s etc.
- General Generic Syntax
  For [[Function]]s / [[Method]]s 
  ``
  fn <fn name><<lifetime><generic name 1>: Trait 1 + Trait 2 +,...,+ Trait n = <Optional Type>, <lifetime><generic name 2>: Trait 1 + Trait 2 +,...,+ Trait n = <Optional Type>,....,<lifetime><generic name k>: Trait 1 + Trait 2 +,...,+ Trait n = <Optional Type>>(<params>) -> <Return Type, default is Unit Type> { 
  ...
  }
  ``
  And the other definitions are kind of the same. 
  Here [[Lifetime]]s and [[Trait]]s are optional but restrict usage of the function with only types that satisfy the requirements.
  Multiple ``trait``s are defined with the ``+`` [[Operator]]. 
  
  * There's another syntax specific to [[Function]]s/ [[Method]]s to make this definition soup more readable. It uses the ``where`` keyword and is hence called the *Where Clause*.
  
  Syntax
  ``
  fn <fn name><<generic name 1>, <generic name 2>...<<generic name n>>(<params>) -> <Return Type> where <generic name 1> : <traits>, <generic name 2>: <traits>, <generic name n>:<traits> { }
  ``
  
  ```rust
  //fn yo<T: X, R: X+Y>(…) -> String {…}
  //can be written as
  
  fn yo<T,R>(…)-> String 
  where T: X,
  	R: X+Y 
  { } 
  ```
- [[Struct]]s and [[Enum]]s 
  
  For ex.:
  ```rust
  struct X<T,K> {
   x: T,
   y: K,
   z: T
  }
  
  //And similarly for enums
  enum A<T> {
   Hello(T, i32),
   No
  }
  
  //and to use
  fn main() {
   let x= X{2,4.0,5}; //ok, infers the types as i32 and f64
   let a= A::Hello(2,4); //ok
  }
  ```
- [[Method]]s
  
  Using Generics in Methods and their implementation blocks allow us to define certain behavior only for certain concrete types. 
  
  For ex.:
  ```rust
  struct X<T> {
   x: T
  }
  
  impl<T> X<T> {
   fn yo(&self, value: T) {}
  }
  ```
  Here we say ``impl`` will accept a generic and the same generic will be used for an instance of the ``X``. ``impl`` can only take a generic if it is applied to the [[Struct]]/ [[Enum]] as well. And the type after ``impl`` can take a generic only if ``impl`` takes a generic.
  
  So, 
  ```rust
  //impl X<T> {...} is invalid as T is unknown
  //impl<T> X{...} is invalid as X needs a type
  impl X<i32>{ 
   fn naa(&self) { }
  } //is valid because we simply define a concrete type of X
  ```
  
  When we define a [[Method]] for ``X<T>`` then it is on every instance of every type of ``X``. But when we define it for ``X<i32>``, then the [[Method]] is only available on every instance of ``X`` of type ``i32``.
  That is, for the above 2 examples combined, 
  
  ```rust
  struct X<T> {
      x: T,
  }
  impl<T> X<T> {
      fn yo(&self, value: T) {}
  }
  //impl X<T> {...} is invalid as T is unknown
  //impl<T> X{...} is invalid as X needs a type
  impl X<i32> {
      fn naa(&self) {}
  } //is valid because we simply define a concrete type of X
  
  fn main() {
      let x = X { x: 4 };
      let y = X { x: 2.0 };
  
      x.yo(2); //ok
  
      //x.yo(5.0); //error because T for X in x is i32, but here we are passing f64
      //y.yo(3); //Similarly, this is also an error
  
      y.yo(4.0); //ok
  
      x.naa(); //ok
  
      //but
      //y.naa(); //error as naa doesn't exist on the instance of X that isn't of type i32
  }
  
  ```
  
  
  * Methods theirselves can then have their own generics too, which are defined just like [[Function]]s.
- Runtime Cost of Generics: 0
  This is due to a process called *Monomorphization*. 
  During compilation, Rust compiler creates the explicit definitions for all [[Data Type]]s of a generic that are used in the code. This makes it as if we had declared each [[Function]] for a type separately. And the generic is replaced with concrete types at compile time itself. The resultant code is called [[Static Dispatch]], as the compiler knows what to call at compile-time itself.
  
  For ex.:
  
  ```rust
  //This definition
  struct X<T> {
   x:T
  }
  
  fn main() {
   let x= X{2};
   let y= X{4.0};
  }
  
  //After compilation would become
  /* 
  struct X_i32 {
   x: i32
  }
  
  struct X_f64{
   x: f64
  }
  
  fn main() {
   let x = X_i32{2};
   let y = X_f64{4.0};
  }
  */
  ```
- An example with Generic + [[Lifetime]] + [[Trait]]s + ``Trait Bound`` Syntax for a [[Function]] with [[Reference Type]]s 
  ```rust
  use std::fmt::Display;
  
  fn longest_with_an_announcement<'a, T>(
      x: &'a str,
      y: &'a str,
      ann: T,
  ) -> &'a str
  where
      T: Display,
  {
      println!("Announcement! {}", ann);
      if x.len() > y.len() {
          x
      } else {
          y
      }
  }
  ```
- There's a difference between a generic name and generic type.
  For ex.:
  ```rust
  use std::fmt::Display;
  trait X<T>{
      fn whoa(&self, x:T);
  }
  
  struct A{}
  
  impl<i32:Display> X<i32> for A {
      fn whoa(&self, x: i32) {
          println!("tf {}", x);
      }
  }
  
  fn main() {
      let a= A{};
      a.whoa(2); //as expected
      a.whoa("hm"); //is it weird ?
  } //works!
  ```
  Here we may think ``i32`` is the [[Number]] ``i32`` but in-fact it is just a name for the generic type. The definition for conditionally implementing ``X<i32>`` for [[Trait]] ``A`` here is ``impl X<i32> for A{...}``.
- Default Type Parameters
  We can define default [[Data Type]] for generics if they aren’t given with the type annotations and aren't inferable automatically. They are only applicable for generics on [[Struct]]s, [[Enum]]s or [[Trait]]s. 
  
  For ex.:
  ```rust
  struct X<T = i32> {}
  
  struct Y<T> {}
  
  impl X { //takes T as i32 automatically.
      fn yo(&self) {}
  }
  
  /* impl Y {
      fn yo(&self) {}
  }
  // error as T needs to be defined for Y
  */ 
  ```
-