- ``join!``
  We can execute multiple futures with ``join!(…)`` or ``try_join!(…)``. The try variant just drops all futures if a future returns an ``Err`` [[Result Type]].
- ``select!``
  It looks much like ``Match`` from [[Pattern Matching]], each arm in a ``select!`` accepts a ``<pattern> = <expression> => <code>``, where expression is a Future that implements the ``Unpin`` and ``FusedFuture`` [[Trait]]s. 
  In a ``select!``, each future is concurrently ran (meaning all futures are polled, if a future waits, another is continued and so on), and whichever future finishes first returns its value, stops all the other futures and the code corresponding it is executed.
  
  For ex.:
  ```rust
  use futures::{
      future::FutureExt, // for `.fuse()`
      pin_mut,
      select,
     future::ready
  };
   
  async fn task_one() { /* ... */ }
  async fn task_two() { /* ... */ }
   
  async fn race_tasks() {
      let t1 = task_one().fuse();
      let t2 = task_two().fuse();
   
      pin_mut!(t1, t2);
   
      select! {
          () = t1 => println!("task one completed first"),
          () = t2 => println!("task two completed first"),
      }
  }
  
  async fn count() {
      let mut a_fut = future::ready(4); //future::ready already implements FusedFuture and Unpin
      let mut b_fut = future::ready(6);
      let mut total = 0;
  
      loop {
          select! {
              a = a_fut => total += a, //assign returned value to a 
              b = b_fut => total += b,
              complete => break, //ran if all futures are completed, as would be case in a loop
              default => unreachable!(), // never runs (futures are ready, then complete)
          };
      }
      assert_eq!(total, 10);
  }
  ``` 
  ``FusedFuture`` is necessary in ``select!`` as it can be looped over, when a ``select!`` is looped over, it continues the other futures which are not completed, this is why ``complete`` is a keyword in ``select!``, it's code is ran if all the futures are completed and we are still looping over it. ``default`` on the other hand is ran if no future has immediately returned, it's code is ran if no future is completed yet.
  
  [[Pinning]] is necessary with ``select!`` as the futures aren't taken by value, but by mutable [[Reference Type]] by ``select!`` and are continued, this requires their values to not move around between each ``select!`` call in a [[Loop]].
  
  * ``Fuse::terminated()`` returns a ``FutureUnordered`` that is a Future which is already completed and terminated, but we can pass it a new future later and it will continue it. 
  Here [Docs](https://rust-lang.github.io/async-book/06_multiple_futures/03_select.html#concurrent-tasks-in-a-select-loop-with-fuse-and-futuresunordered).