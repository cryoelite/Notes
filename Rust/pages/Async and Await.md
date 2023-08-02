- Async/Await model in other languages is a high level method of working with concurrency. 
  Unlike OS [[Thread]]s,  this model allows for things like Coroutines, Event driven programming (Callbacks) and the like. 
  In rust, Async/Await model is natively supported and has its own features. But it lacks a runtime, which is left upto community [[Crate]]s, meaning a model to actually handle async/await paradigm in rust at runtime has to be added manually.
  
  Rust uses [[Future]]s, 
  async in Rust has 0 cost, meaning we have no dynamic dispatch or heap allocations for async [[Function]]s,
  and Futures are inert, meaning they are lazy and only execute when polled and don't use up additional resources until needed, and drop everything if they are dropped without going to completion.
  
  * Async vs [[Thread]]s: Threads incur a memory and CPU overhead when they have to be spawned, and they consume the same even at runtime and even they are idle. 
  Async overcomes this by having a few threads and managed by a state machine, this means the binary size is greater as this added construct handles tasks at runtime but the overhead is smaller and that allows for greater number of concurrent tasks. 
  Hence, Async paradigm is better than thread paradigm but also more complex, introduces more sources for bugs and requires a slightly different programming model than synchronous code which goes well even with threads.
  
  * Async/Await and ``Future`` [[Trait]] are provided by Rust and its [[Standard Library]]. However, many utility [[Data Type]]s, [[Macro]]s and [[Function]]s to work with these are provided by the ``futures`` [[Crate]]. Then the execution of async code, IO and task spawning is provided by the async runtime which is provided by any external crate, such as ``Tokio``.
- Async [[Function]]: ``async`` on functions turn them into state machines, and they implement the ``Future`` [[Trait]] automatically. [[Future]] are like normal functions but they don't block the caller [[Thread]] like normal functions when they are called, they yield the control back and are continued by the async runtime when the value from the future is needed.
  
  For ex.:
  First we add ``Future`` trait as a dependency in [[cargo.toml]] ,
  ```toml
  [dependencies]
  futures = "0.3"
  ```
  Then we can use ``block_on``, 
  ```rust
  // `block_on` blocks the current thread until the provided future has run to
  // completion. Other executors provide more complex behavior, like scheduling
  // multiple futures onto the same thread.
  use futures::executor::block_on;
  
  async fn hello_world() {
      println!("hello, world!");  
  }
  
  fn main() {
      let future = hello_world(); // Immediately returns a future but doesn't execute the function.
      block_on(future); // `future` is run and "hello, world!" is printed
  }
  ```
  ``block_on`` is an [[Executor]], meaning it executes the given future [[Data Type]].
  
  * ``.await``: Awaits a future concurrently, as we know a ``Future<T>`` [[Data Type]] is returned immediately when an async function is called, ``await`` on the ``Future<T>`` concurrently waits on the ``Future`` and executes the function whenever the async runtime has time to do so or if the return value is required.
  For ex.:
  ```rust
  async fn learn_song() -> i32 {
   2
   }
  async fn sing_song(x: i32) {  }
  async fn dance() { }
  
  async fn learn_and_sing() {
      
      // We use `.await` here rather than `block_on` to prevent blocking the
      // thread, which makes it possible to `dance` at the same time.
      let song = learn_song().await; //returns a future, then await on it returns the value i32
      sing_song(song).await; //ok, whilst both are started at same time, since this needs the return value of learn_song, it is ran to completion
  }
  
  async fn async_main() {
      let f1 = learn_and_sing();
      let f2 = dance();
  
      // `join!` is like `.await` but can wait for multiple futures concurrently.
      // If we're temporarily blocked in the `learn_and_sing` future, the `dance`
      // future will take over the current thread. If `dance` becomes blocked,
      // `learn_and_sing` can take back over. If both futures are blocked, then
      // `async_main` is blocked and will yield to the executor.
      futures::join!(f1, f2);
  }
  
  fn main() {
      block_on(async_main());
  }
  ```
- Async [[Function]]s with explicit return type and async block
  Async functions are normal functions that return a ``Future<T>``, it is optional to write the whole syntax as we can just write ``T`` for the return and it means the same thing.
  
  Async can also be applied to [[Scope]] blocks, these blocks also return a ``Future<T>``. 
  For ex.:
  ```rust
  // `foo()` returns a type that implements `Future<Output = u8>`.
  // `foo().await` will result in a value of type `u8`.
  async fn foo() -> u8 { 5 }
   
  fn bar() -> impl Future<Output = u8> {
      // This `async` block results in a type that implements
      // `Future<Output = u8>`.
      async {
          let x: u8 = foo().await;
          x + 5
      }
  }
  
  
  fn main() {
   block_on(bar());
  }
  
  ```
  
  * Async [[Lifetime]]
  
  Async [[Function]]s that take [[Reference Type]]s (with non static lifetimes) and have no explicit return type automatically use the lifetime of the return type.
  
  For ex.:
  ```rust
  // This function:
  async fn foo(x: &u8) -> u8 { *x }
  
  // Is equivalent to this function:
  fn foo_expanded<'a>(x: &'a u8) -> impl Future<Output = u8> + 'a {
      async move { *x }
  }
  ```
  Here, if the ``Future<T>`` returned from ``foo_expanded`` used reference ``x`` and passed it as a reference again to the async block, then the async block won't have access to x as the rest of the function scope is over and future's aren't executed right away, so ``x`` is automatically ``move``d into the returned ``Future<T>``. And since the returned ``Future<T>`` uses a reference, it needs to define the lifetime as well.
  
  This also means, that if our async function passes a reference created inside it to another async function/block and returns it, then it must move it otherwise the same issue would occur.
  
  For ex.:
  ```rust
  async fn borrow_x(x: &i32) -> u8 {
   2
  }
  
  fn bad() -> impl Future<Output = u8> {
      let x = 5;
      //borrow_x(&x) // ERROR: `x` does not live long enough
  }
  
  fn good() -> impl Future<Output = u8> {
      async {
          let x = 5;
          borrow_x(&x).await //ok, since the async block has the reference and doesn't rely on the body of good() to be in scope
      }
  }
  ```
- Async ``move``
  Used async blocks.
  Just like the ``move`` in [[Closure]]s. Moves the references into the block.
  
  For ex.:
  ```rust
  fn yo() -> impl Future<Output= i32>{
    let x= 20;
  async move{
     *(&x) //moves x into this closure
    }
  }
  ```
- [[Pointer]]s from the [[Standard Library]] such as [[Rc<T>]], [[Mutex<T>]] etc. don't necessarily implement the ``Send`` [[Trait]]. This can cause one thread to lock up a value and another to request it when using [[Future]]s. 
  This is why it is recommended to use ``Future``-aware [[Pointer]]s such as ``Mutex`` from ``futures::lock`` [[Module]] instead of ``std::sync``.
- ``async_std`` is in [[Standard Library]]
  It provides some async features such as ``async_std::task::spawn``. This allows task spawning. Meaning, we can pass a [[Future]] to ``spawn(...)`` [[Function]] and it is executed asynchronously, it returns a ``JoinHandle`` which can be ``.await``ed.
- ``?`` with ``await`` on [[Result Type]]s
  For ex.:
  ```rust
  let fut = async {
      foo().await?;
      bar().await?;
      //Ok(()) //error
      Ok::<(), MyError>(()) // <- note the explicit type annotation here
  };
  ```
  We need to use the ``Turbofish`` [[Operator]] to explicitly define the return type with ``?``. This is a known limitation with Rust's compiler.