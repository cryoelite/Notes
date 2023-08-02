alias:: Associated Function

- Method
  Methods are [[Function]]s defined for the [[Struct]]s/ [[Enumerated Type]]s/ [[Trait Object]]s. They are defined inside a special block with a keyword ``impl``, this is for organization, and makes it easier to find out where a custom [[Data Type]] has been defined implementations/behaviors for it, kind of like C++ where we have separate header and source files and unlike C++ there's no need for definition in the [[Data Type]] itself. Since all methods are associated with a [[Data Type]], they are also called *Associated Function*s.
  
  Syntax: 
  ``
  impl <T> {
   fn someMethod(<self/&self>,<some Params>) -> <some return type> {}
  }
  ``
  
  
  For ex.:
  ```rust
  struct Rect{
  	length: i32,
  	breadth: i32
  }
  
  impl Rect {
  	fn area(&self) -> i32 {
  	self.length * self.breadth
   } 
  }
  fn main() {
   let rect= Rect(2,4);
   let area=rect.area(); //works
  }
  ```
  
  A single custom [[Data Type]], can have multiple ``impl`` blocks for it.
- ``self``
  Every method of a [[Struct]]/ [[Enumerated Type]]/ [[Trait Object]] can define ``self`` as the first parameter in order to access the instance values, it can choose to not have the parameter if it doesn’t need access to field instances. ``self`` is of type ``Self`` and copies the instance field values into itself, it also takes the [[Ownership]] so it is rarely used.
  ``&self`` is the immutable [[Reference Type]] variant and ``&mut self`` is the mutable reference variant. Rust allows us to use the shorthand ``self``/``&self``/``&mut self`` without specifying the ``Self`` type.
  
  For ex.:
  ```rust
  struct ABC {}
  
  impl ABC {
  	fn yo() {}
      fn ctor() -> Self {
      ABC{}
    }
  }
  
  fn main(){
   ABC::someFunc();  //to call a self-less method
  }
  ```
  ``Self`` is a type that can be assigned to the impl's type, so it can be used for *Constructor*s/*Factory* Methods.
- Methods in rust can have the same name as *fields* of the custom [[Data Type]]. 
  Rust know what we are calling by assessing if we specify the () or not.
  
  For ex.:
  ```rust
  struct X {
  length: i32,
  }
  
  impl X {
   fn length(&self)->i32 { 
    2
  }
  }
  
  fn main() {
   let x= X{length: 5};
   let y= x.length(); //gets 2
   let z = x.length; //gets 5
  }
  ```
- Automatic *Referencing* and *Dereferencing*
  For ex.:
  ```rust
  Struct X {
  length: i32,
  }
  
  impl X {
   fn yo(&self) {
   //yee
   }
  }
  fn main() {
   let x = X{ length: 5};
   x.yo(); 
   //is the same as
   (&x).yo();
  }
  ```
  In Rust, ``x`` is an *object pointer*, and we know in langs like C++, we access methods inside them with the ``->`` [[Operator]] so like ``(*x).yo()`` or ``x->yo()``,  but rust doesn’t have the ``->`` [[Operator]], and nor does it need to be explicit specified if we want to use the immutable [[Reference Type]] (with ``&(x).yo()``), mutable ref (with ``(&mut x).yo()``), or [[Copy or Move]] of the instance (with ``(x).yo()``) as it automatically determines the right type from the parameter of the methods, like ``yo`` above takes an immutable reference, so rust knows ``x.yo()`` is equivalent to ``(&x).yo()``.
- Calling other methods
  
  For ex.:
  ```rust
  struct A {
      x: i32,
  }
  
  impl A {
      fn ya() {}
  
      fn na(&self) {}
  
      fn callSelf(&self) {
          Self::ya();
          self.na();
      }
  
      fn callNoSelf() {
          A::ya();
          //can't call na as there's no known instance here
      }
  }
  
  fn main() {
      let a = A { x: 2 };
      a.callSelf();
      A::callNoSelf();
  }
  ```
-