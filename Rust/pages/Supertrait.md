- [[Trait]]s can use other traits too.
  A [[Trait]] that gets implemented by another [[Trait]] is called a supertrait. And if we wish to implement this child trait, we are required to implement the supertrait itself as well. The benefit is that since this is a requirement, the child trait can use [[Method]]s of the supertrait and it is guaranteed they will be defined by the implementor [[Data Type]].
  
  Syntax:
  ``
  trait <some name> {
   //some methods
  }
  
  trait <some other name>: <some name> {
   //some methods
  }
  
  //then implement both for a data type
  ``
  
  For ex.:
  ```rust
  trait X {
      fn yo();
      fn no(&self);
  }
  
  trait Y: X {
      fn hello() {
          Self::yo();
          //no(); error as no() is defined for the instance and we have no access to instance here
      }
  
      fn ho(&self) {
          Self::yo();
          self.no();
      }
  }
  
  struct A {}
  
  impl X for A {
      //required
      fn yo() {}
      fn no(&self) {}
  }
  
  impl Y for A {}
  
  fn main() {
      let a = A {};
      A::hello(); //ok
      a.ho(); //also ok
  }
  
  ```
- If the child [[Trait]] implements [[Method]]s with the same name as the supertrait, they aren't defining implementation for the supertrait's method, rather they are shadowing them. 
  Then the child trait must use [[Fully Qualified Syntax For Disambiguation]] if its methods call this method.
  
  For ex.:
  ```rust
  trait X {
      fn yo();
  }
  
  trait Y: X {
      fn ho() {
          //Self::yo(); //error
          <Self as X>::yo(); //ok
      }
      fn yo() {} //same name method
  }
  
  struct A {}
  
  impl X for A {
      fn yo() {}
  }
  
  impl Y for A {}
  
  fn main() {
      let a = A {};
      A::ho();
      //A::yo(); //error
      <A as Y>::yo(); //ok
  }
  
  ```