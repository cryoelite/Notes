- ``trait``
  Kind of like an *interface* from other langs.
  Traits allow us to define functionality that can be shared to one or more [[Data Type]]s. By [[Data Type]]s we refer to [[Struct]] and [[Enum]]s.
- Traits are defined using ``trait``, and then for a type they are implemented using ``impl`` blocks.
  Much like *interface*s, ``trait``s can just define the [[Method]] Signatures, and the actual implementation for the methods can be given by the type implementing the  Trait.
  
  Syntax:
  ``
  <Optional Access Modifier> trait <trait name> {
   fn <method 1 name>(<params>)-> <Optional Return Type>;
   //can define a default implementation
  fn <method 2 name>(<params>)-> <Optional Return Type> { } //ok
   ...
    fn <method n name>(<params>)-> <Optional Return Type>;
  }
  ``
  And then to implement them,
  
  ``
  impl <trait name> for <Struct/Enum name> {
   <Method 1 definition>{...}
    <Method 2 definition>{...}
   ...
    <Method n definition>{...}
  }
  ``
  
  For ex.:
  ```rust
  pub trait Dance{
   fn move(&Self)-> i32;
   
  }
  
  struct X{
   x:i32,
  }
  
  impl Dance for X {
   fn move(&self)-> i32 {
   2+2
   }
  }
  
  fn main() {
   let x= X{2};
   x.move(); //works
  }
  ```
  
  The [[Access Modifier]]s are applied to trait definitions too and they are *private* by default, so if a [[Crate]]/ [[Module]] has a private trait then it can't be used by an external [[Crate]] to implement the trait on its types, this doesn't mean private traits defined for public types have their implementations hidden, the implemented methods are still available to external [[Crate]]s.
- Traits have to be implemented on [[Data Type]]s by the compile-time, Rust doesn't support dynamically implementing traits on types at runtime. So [[Function]]s can't implement Traits, but [[Macro]]s can.
- All [[Method]]s in a trait definition without a default implementation need to be defined when the trait is implemented for a [[Data Type]].
- Default implementation
  A trait definition can define a default implementation for a [[Method]], and then that definition is used if the implementing type doesn't override it.
  
  For ex.:
  
  ```rust
  pub trait Dance{
   fn move(&Self)-> i32 { }
  }
  
  struct X{
   x:i32,
  }
  
  impl Dance for X {
  
  }
  //ok
  
  ```
  The empty ``impl`` block is necessary as it defines ``Dance`` is being implemented on ``X``.
  
  Trait implementations with empty bodies are called *Marker Trait*s or *Tag Trait*s.
- Orphan Rule
  This is part of a property called [[coherence]]. According to it, a ``trait`` can be implemented for a type iff either the trait or the [[Data Type]] is local to the [[Crate]] (defined in the same [[Crate]]). 
  
  For ex.:
  If crate ``A`` defines a trait ``X`` then it can define the trait ``X`` for any [[Data Type]] , local or external, such as a type ``T`` from crate ``B``. 
  Similarly, if crate ``A`` defines a type ``T`` then it can implement trait ``X`` defined in crate B for its type ``T``. 
  But if crate ``C`` defines neither type ``T`` or trait ``X`` but takes them from crate ``A`` and ``B``, i.e., both are external then this is not allowed according to this rule.
  
  This is to ensure the Parent type exists, and other people’s code can’t break our own or theirs because a trait/type is defined at a place different from the crate where it is created.
  
  * Newtype Pattern: It is possible to avoid the Orphan Rule by simply wrapping the external type ``T`` in a new type and then implementing a trait on this new type. It's called so because of a similar pattern in Haskell.
  For ex.:
  ```rust
  use std::fmt;
   
  struct Wrapper{
      v: Vec<String>
  }
   
  impl fmt::Display for Wrapper {
      fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
          write!(f, "[{}]", self.v.join(", "))
      }
  }
   
  fn main() {
      let w = Wrapper{v:vec![String::from("hello"), String::from("world")]};
      println!("w = {}", w);
  }
  
  ```
- ``self``
  We use ``self`` in a trait to pass an instance of the type to the function, allowing us to call other [[Method]]s and fields.
  While this means we can’t access a type-specific method/field in a trait’s default method, we can access trait-specific methods because of it. 
  For ex.:
  ```rust
  pub trait X {
      fn na();
      fn ya(&self);
      fn yo(&self) -> String {
          Self::na();
          self.ya();
          return "na".to_string();
      } //works because self will always have na() and ya() as it’s defined on trait itself.
      fn ayo(&mut self);
  }
  
  struct A {
      x: i32,
  }
  
  impl X for A {
      fn na() {}
      fn ya(&self) {}
      fn ayo(&mut self) {
          self.x += 2;
      }
  }
  
  fn main() {
      let mut a = A { x: 2 };
      a.ayo();
  }
  ```
  However, non-default implemented trait methods can access instance fields and methods.
- [[Function]]/ [[Method]] Parameters/Return Types can use [[Trait]]s too
  When Parameters use traits, they require the type of object being passed as argument to implement the trait(s).
  These are simply a syntax sugar for using [[Generic Type]]s, aka ``Trait Bound`` Syntax.
  
  Syntax:
  
  ``
  fn <somename>(<param name>: <optional reference &> impl <trait name> + <any other trait> + .., <param 2>) {...}
  ``
  
  For ex.:
  ```rust
  pub trait X {
   fn notify() {}
  }
  pub fn notify(item: &impl X) { //Requires the type of object being passed to notify() to implement the trait X
      println!("Breaking news! {}", item.notify());
  }
  ```
  
  * ``Trait Bound`` Syntax: 
  The ``impl Trait`` is simply a syntax sugar for a longer syntax which is just the [[Generic Type]] syntax.
  
  For ex.:
  ```rust
  pub trait X {
   fn notify() {}
  }
  pub fn notify<T: X>(item: &T) { 
      println!("Breaking news! {}", item.notify());
  }
  ```
  There's a slight difference between the 2 syntaxes, the ``impl Trait`` syntax requires simply the type of the argument object to implement the trait, but it doesn't require the object's types to be the same.
  For ex.:
  ```rust
  pub trait X {
   fn notify() {}
  }
  pub fn notify<T: X>(item: &T, item2: &T) {  //accepts a single type T and that T must be the same for both the args/params
  }
  pub fn notify(item: &impl X, item2: &impl X) {  //accepts 2 types, they can be the same, they just need to implement the trait X
  }
  
  ```
  
  * The Return types can use the ``impl Trait`` and [[Generic Type]]s too
  However, there's a limitation with the Rust Compiler that disallows different code paths returning different types. All code paths in a [[Function]]/ [[Method]] must return the same [[Data Type]] object, and that type must implement the given trait(s). 
  For ex.:
  ```rust
  pub trait X {}
  
  struct A{ }
  
  impl X for A {}
  
  struct B{ }
  
  impl X for B {}
  
  fn yo()-> impl X {
   if (1==2) {
     return A{};
    }
  else {
    return B{};
   }
  }
  //whilst it should work, doesn't. Gives an error as both must return the same type of object.
  ```
- Conditionally implementing [[Method]]s and Traits for custom [[Data Type]]s
  Using ``Trait Bound`` Syntax, we can define methods or traits for types that have
  given traits.
  For ex.:
  ```rust
  struct X<T> {…}
  trait Y {}
  trait Z {
  fn na() {}
  }
  
  impl<T: Y> X<T> {
  	fn yo() { }
  }
  
  impl<T: Y> Z for X<T> {
  	
  }
  
  impl<T> Y for X<T> {
  
  }
  ```
  For a type ``T`` used to create the object of ``X``, the method ``yo()`` can only be called if ``T`` implements the trait ``Y`` and the methods of trait ``Z``, i.e., ``na()`` can only be called if ``T`` implements the trait ``Y``.
  
  This is how we conditionally implement [[Method]]s/Traits for a [[Data Type]]. Yes, this condition is resolved at compile-time itself as the concrete definitions are put in by the compiler.
  
  * Blanket Implementation
  We can also apply traits to [[Generic Type]]s directly. These are known as blanket implementations as they apply a trait to multiple types.
  
  For ex.:
  ```rust
  impl<T: Display> ToString for T {
      // --snip--
  }
  ```
  This is how the ``ToString`` trait is already applied in the [[Standard Library]], for any [[Data Type]] that implements the trait ``Display``, the trait ``ToString`` is automatically applied. Similarly we can apply our own traits to multiple types at once too.
- Multiple Traits for [[Generic Type]]s definition are combined with the ``+`` [[Operator]]
  
  For ex.:
  ```rust
  trait X{}
  trait Y{}
  pb fn yo<T: X+Y>() {}
  ```
- [[Generic Type]]s can use traits, and traits can use generics too.
  For ex.:
  ```rust
  pub trait X<T> {
      fn yo(x: T);
      fn no(&self, x: T);
      fn ok() -> Option<T>;
      fn lol(&self) -> Option<T>;
  }
  
  struct A {}
  
  impl<T> X<T> for A {
      fn yo(x: T) {}
      fn no(&self, x: T) {}
      fn ok() -> Option<T> {
          None
      }
      fn lol(&self) -> Option<T> {
          None
      }
  }
  
  struct B {}
  
  impl X<i32> for B {
      fn yo(x: i32) {}
      fn no(&self, x: i32) {
          println!("yo no");
      }
      fn ok() -> Option<i32> {
          println!("ok");
          None
      }
      fn lol(&self) -> Option<i32> {
          None
      }
  }
  
  fn main() {
      let a = A {};
      a.no(2);
      A::yo(4);
      A::ok() as Option<i32>;
      a.lol() as Option<i32>;
  
      let b = B {};
      b.no(2);
      //b.no("ab"); //error
      //B::ok() as Option<f64>; //error
  } // works
  
  ```
  Just like [[Method]] ``impl`` blocks we can conditionally define traits for [[Data Type]]s too. 
  However, generics or not, a trait can only be implemented once for any [[Data Type]]. So if we conditionally define a trait for a type, then it's methods are only available for that type.
  
  * [[Associated Type]]s provide an alternative to using traits with generics. The benefit of using associated types is that the caller code has no need of knowing the type or defining it, it is all handled by the type implementing the trait itself.
- Common Traits in the [[Standard Library]] 
  * ``Copy``
  The ``Copy`` trait can only be implemented for a [[Data Type]] if it doesn't implement the ``Drop`` Trait.
  
  * ``Sized``
  This trait is automatically applied to all types whose size is known at compile time. We can apply it to [[Generic Type]]s and custom [[Data Type]]s too .
   
  For ex.:
  ```rust
  fn yo<T> (a: T) {}
  //is actually
  //fn yo <T:Sized> (a:T) {…}
  
  //Hence
  //fn yo<T: ?Sized> (a: &T) {…} //allows DSTs to be passed to yo as well
  ```
  The ``?<trait>`` syntax says ``T`` may or may not implement the ``Sized`` trait. This syntax is only applicable for ``Sized`` trait. We can pass [[Dynamically Sized Type]]s using this trait.
- Derivable Traits
  By using ``#[derive]`` [[Macro Attribute]] we can make our [[Data Type]]s automatically implement a given trait. This only works for a few basic traits like ``PartialEq``. 
  For ex.:
  ```rust
  #[derive(PartialEq, Debug)]
  struct X{}
  ```
  
  These are some more derivable traits
  * Comparison traits: [`Eq`](https://doc.rust-lang.org/std/cmp/trait.Eq.html), [`PartialEq`](https://doc.rust-lang.org/std/cmp/trait.PartialEq.html), [`Ord`](https://doc.rust-lang.org/std/cmp/trait.Ord.html), [`PartialOrd`](https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html).
  * [`Clone`](https://doc.rust-lang.org/std/clone/trait.Clone.html), to create `T` from `&T` via a copy.
  * [`Copy`](https://doc.rust-lang.org/core/marker/trait.Copy.html), to give a type 'copy semantics' instead of 'move semantics'.
  * [`Hash`](https://doc.rust-lang.org/std/hash/trait.Hash.html), to compute a hash from `&T`.
  * [`Default`](https://doc.rust-lang.org/std/default/trait.Default.html), to create an empty instance of a data type.
  * [`Debug`](https://doc.rust-lang.org/std/fmt/trait.Debug.html), to format a value using the `{:?}` formatter.
- ``Deref`` and ``DerefMut``
  Traits defined in ``std::ops``.
  
  * ``Deref``: Any [[Data Type]] that implements this trait can use the dereference [[Operator]] ``*``. 
  [[Pointer]]s and non-primitive [[Reference Type]]s implement it in [[Standard Library]] .
  
  The main method of this trait is the ``deref()`` [[Method]] which is called when value of the implementing type is getting dereferenced.
  ``Deref`` and its ``deref()`` return immutable [[Reference Type]]s.
  
  * Since [[Pointer]]s implement the ``Deref`` [[Trait]], they can be dereferenced.
  For ex.:
  ```rust
  fn main {
   let x = Box::new(2);
   println!("{}", *x);
  }
  ```
  That is also to say, a pointer has a [[Reference Type]] to the data (as the data is stored in a separate memory address from the pointer itself). Since Reference values can't be used without being dereferenced we need to use the ``*`` [[Operator]].
  
  We can implement this Trait for our types too,
  For ex.:
  ```rust
  use std::ops::Deref;
  
  struct MyBox<T>(T);
  
  impl<T> MyBox<T> {
      fn new(x: T) -> MyBox<T> {
          MyBox(x)
      }
  }
  
  impl<T> Deref for MyBox<T> {
      type Target = T;
  
      fn deref(&self) -> &Self::Target { //notice dereference returns the &T
          &self.0
      }
  }
  
  fn main() {
   let x = MyBox::new(2);
   *x; //returns 2
   //is the same as
   *(x.deref()); //also returns 2
  }
  ```
  using an [[Associated Type]] ``target``.
  The reason ``deref()`` [[Method]] returns a [[Reference Type]] is simply because if it returned the value directly, it would invoke [[Copy or Move]] and transfer the [[Ownership]] out of ``MyBox``. 
  The main goal dereferencing is to reach a primitive type, as primitive types have the concrete implementation for how actually dereferencing works without transferring [[Ownership]] or [[Copy or Move]]ing. 
  So if our type ``T`` dereferences into a value of type ``K`` (i.e., ``T``'s ``Deref`` trait's ``deref`` method returned ``&K``) which dereferences into a primitive type such as ``i32`` then the dereference chain to go from ``&T`` to ``i32`` would be
  ``*<T value>``->``*<K value>``->``*<i32 value>`` where the dereference of ``*<i32 value>`` is defined internally.
  
  The feature by which Rust automatically goes from ``*<T value>`` to ``<i32 value>`` is called [[Deref Coercion]].```
  
  * ``DerefMut``:  This trait and its ``deref()`` method return mutable references.
  Rust's [[Deref Coercion]] uses the following chain
  
  * From ``&T`` to ``&U`` if ``T: Deref<Target=U>``, i.e., ``T`` implements the ``Deref`` trait. and returns a value of type ``&U`` from its ``deref()`` [[Method]].
   
  * From ``&mut T`` to ``&mut U`` if ``T: DerefMut<Target=U>``
  * From ``&mut T`` to ``&U`` if ``T: Deref<Target=U>``
- ``Drop`` Trait
  This trait needs implementation for the ``drop`` [[Method]] which takes a mutable [[Borrow]] of ``self`` and then runs some cleanup/deallocation code. The ``drop`` method is called automatically by the Rust compiler when the instance of the type goes out of [[Scope]]. ``Drop`` trait is included in the [[Prelude]]. Basically, the ``drop()`` method is much like a *Destructor* from other languages.
  
  For ex.:
  ```rust
  struct MyT{ yo:i32 }
  impl Drop for MyT {
  	fn drop(&mut self) {
  	//do something with self.yo. 
   }
  }
  ```
  
  * Smart-[[Pointer]]s implement this trait.
  
  * We can't explicitly call the ``drop()`` [[Method]] as it leads to the *Double Free* error. So, Rust provides a way to let the compiler know as well as call it early. It is the ``std::mem::drop(<value of type T that implements Drop>)`` [[Function]] in the [[Prelude]].
  For ex.:
  ```rust
  fn main() {
   let x = Box::new(2);
   drop(x); //works
  }
  ```
- ``Send`` and ``Sync`` Traits
  Defined in ``std::marker``.
  
  The send trait indicates the [[Ownership]] of the [[Data Type]] that implements it can be transferred between threads. Almost all types in Rust implement it, but not types such as [[Rc<T>]]. 
  
  Similarly, the sync trait indicates the type implementing it is safe to be accessed from multiple threads. 
  
  * Both of these are known as marker traits as they are in the marker Module and they have no [[Method]]s to implement either, they just mark a type and are used to indicate rust's concurrency rules are followed. 
  Still, we can have custom implementation for both but that is part of [[unsafe]] Rust. 
  * All primitive types implement both of these. 
  * If a type is composed only of Send Trait implementing types then that type implicitly implements the send trait as well. Same with Sync trait as well.
  * These traits are [[unsafe]].
- [[unsafe]] Traits
  Traits which have some [[unsafe]] [[Method]]s. 
  We declare these traits with ``unsafe`` and same for their ``impl`` blocks .
  
  For ex.:
  ```rust
  unsafe trait Foo {
      // methods go here
  }
  
  unsafe impl Foo for i32 {
      // method implementations go here
  }
  
  fn main() {}
  ```
- [[Supertrait]]
- [[Future]]'s ``Future`` Trait
  The future trait’s working helps us understand [[Async and Await]] Rust better.
  
  It basically looks like this, 
  ```rust
  trait SimpleFuture {
      type Output;
      fn poll(&mut self, wake: fn()) -> Poll<Self::Output>;
  }
   
  enum Poll<T> {
      Ready(T),
      Pending,
  }
  ```
  For ex. 
  ```rust
  pub struct SocketRead<'a> {
      socket: &'a Socket,
  }
   
  impl SimpleFuture for SocketRead<'_> {
      type Output = Vec<u8>;
   
      fn poll(&mut self, wake: fn()) -> Poll<Self::Output> {
          if self.socket.has_data_to_read() {
              // The socket has data -- read it into a buffer and return it.
              Poll::Ready(self.socket.read_buf())
          } else {
              // The socket does not yet have data.
              //
              // Arrange for `wake` to be called once data is available.
              // When data becomes available, `wake` will be called, and the
              // user of this `Future` will know to call `poll` again and
              // receive data.
              self.socket.set_readable_callback(wake);
              Poll::Pending
          }
      }
  }
  ```
  Basically, ``Future`` returns a ``Poll`` [[Enum]] from the ``poll`` [[Function]]. The [[Executor]] calls the ``poll`` function and the enum indicates if the future is complete or pending. 
  The poll function gets a [[Closure]] ``wake`` which tells the executor when it can wake and call the ``poll`` function again to forward the future towards completion. Using this functionality ``Future`` trait allows multiple Futures to be run in the same [[Thread]] as when one is pending other is run and so on for all of them.
  
  The actual future trait looks like so
  ```rust
  trait Future {
      type Output;
      fn poll(
          // Note the change from `&mut self` to `Pin<&mut Self>`:
          self: Pin<&mut Self>,
          // and the change from `wake: fn()` to `cx: &mut Context<'_>`:
          cx: &mut Context<'_>,
      ) -> Poll<Self::Output>;
  }
  ```
  They used [[Pinned Trait]] and the ``context`` [[Closure]] here. The ``context`` closure allows identifying which [[Function]] called the wake, otherwise it’s god’s plan as it is just a [[Pointer]] .
  
  A ``Future`` is stored inside a ``Waker`` [[Data Type]], this type provides the ``wake`` function and the future is also stored in this type. The executor receives this type and calls it a ``Task``. 
  
  The ``wake`` function is essentially called through integration with an *IO-aware* system blocking primitive, such as *epoll* on Linux, *kqueue* on FreeBSD and MacOS, *IOCP* on Windows, and *ports* on Fuchsia (all of which are exposed through the cross-platform Rust [[Crate]] ``mio``). These primitives all allow a thread to block on multiple asynchronous IO events, returning once one of the events completes.
- ``Stream`` 
  Just like [[Iterator]]s, but uses [[Async and Await]] and [[Future]]s. 
  For ex.:
  ```rust
  async fn send_recv() {
      const BUFFER_SIZE: usize = 10;
      let (mut tx, mut rx) = mpsc::channel::<i32>(BUFFER_SIZE);
   
      tx.send(1).await.unwrap();
      tx.send(2).await.unwrap();
      drop(tx);
   
      // `StreamExt::next` is similar to `Iterator::next`, but returns a
      // type that implements `Future<Output = Option<T>>`.
      assert_eq!(Some(1), rx.next().await);
      assert_eq!(Some(2), rx.next().await);
      assert_eq!(None, rx.next().await);
  }
  ```
  or 
  when used in a [[Loop]],
  ```rust
  async fn sum_with_next(mut stream: Pin<&mut dyn Stream<Item = i32>>) -> i32 {
      use futures::stream::StreamExt; // for `next`
      let mut sum = 0;
      while let Some(item) = stream.next().await {
          sum += item;
      }
      sum
  }
  //for loop doesn’t work with stream. 
  ```
-