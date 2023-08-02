- Just like Go, Rust has the concept of passing messages between [[Thread]]s using channels. These are defined in ``std::sync``. Every channel has 2 ends, a *Transmitter* (let's call it tx) which we use to send some data and a *Receiver* (rx), which we use to receive the sent data.
- ``mpsc``
  ``mpsc`` is a multiple-producer single-consumer channel. It allows multiple txs but only a single rx. 
  
  For ex.:
  ```rust
  use std::sync::mpsc;
  use std::thread;
  
  fn main() {
      let (tx, rx) = mpsc::channel();
  
      thread::spawn(move || {
          let val = String::from("hi");
          tx.send(val).unwrap();
      });
       let received = rx.recv().unwrap();
      println!("Got: {}", received);
  }
  ```
  We use [[Destructuring]] here to get them from the [[Tuple]] returned by ``channel()`` [[Method]].
  
  * The sender thread needs [[Ownership]] of both the tx and the value it is sending.
  ``<tx>.send(<obj>)`` returns a [[Result Type]] which has denotes if the sending was successful or not.
  
  Similarly, the receiver thread needs ownership of the rx. Calling ``<rx>.recv()`` pauses the thread until the receiver receives the data. It then returns a [[Result Type]] if it got a value or an error (when a tx dies, all rx's receive an err attached to those tx's). There's a ``try_recv()`` variant which checks the channel and immediately returns without pausing the thread, it's receives an err if there was no value.
  
  * ``mpsc`` allows multiple tx's, these can be passed to different threads. But the same single receiver would receive them. We get multiple txs by calling ``<tx>.clone()`` on any tx.
- Multiple values in the channel
  For ex.:
  ```rust
  use std::sync::mpsc;
  use std::{thread, time::Duration};
  fn main() {
      let (tx, rx) = mpsc::channel();
      thread::spawn(move || {
          for i in 1..10 {
              tx.send(i).unwrap();
              thread::sleep(Duration::from_millis(10))
          }
      });
      for i in rx {
          println!("{}", i);
     }
  } 
  
  ```
  tx's and rx's of Rust channels implement [[Iterator]] and the rx loop waits for all values being sent to it. Then when the tx dies, the rx loop also finishes.
-