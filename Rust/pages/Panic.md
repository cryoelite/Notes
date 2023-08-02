- ``panic!``
  This macro immediately signals the system there has been an unrecoverable error and hence the program stops right there.
  Syntax: ``panic!("<string>")``
  
  For ex.:
  ```rust
  fn main() {
   panic!("crash and burn"); //panics
  } 
  ```
- Unwinding and Aborting on Panic
  When a program panics, the call-stack is *unwinded*, i.e. Rust's runtime requires the program to walk back its call-stack, then it has to clean up all the resources used in each [[Function]] and finally exit. This is a time-taking task and it also increases the size of the Rust binary/Executable. 
  
  However, we can avoid it and let the OS clean-up all the resources instead, which is a less cleaner solution but allows quicker exit and smaller binary size. This is called `Aborting on Panic`.
  We can enable it for our [[Package]] by configuring the [[cargo.toml]] like so
  ```toml
  [profile.release]
  panic = 'abort'
  ```
  This applies AOP on the release builds.
- Backtrace
  It's possible that our code itself doesn't cause a panic directly, such as if we use a [[Vector]] and access an invalid index it panics in the Vector's files and not in our code. This is not shown by the normal stacktrace printed by the panic [[Macro]].
  To see a detailed backtrace, we need the system environment over the Rust app to have the environment variable ``RUST_BACKTRACE`` set to any value other than 0. 
  Although this only works in debug builds where the debug symbols are loaded.
-