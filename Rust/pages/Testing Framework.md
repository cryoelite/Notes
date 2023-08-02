- Rust already has a testing framework built-in, this allows us to do *TDD*.
- Tests in rust are [[Function]] annotated with ``#[test]`` [[Macro Attribute]] inside a test [[Module]] which can be annotated with ``#[cfg(test)]``.
  A test module is generated automatically for any new [[Cargo]] library created with ``cargo new <name> --lib``.
  
  Then to run all test functions in test [[Module]s we run ``cargo test``.
  
  For ex.:
  ```rust
  #[cfg(test)]
  mod tests {
      #[test]
      fn it_works() {
          let result = 2 + 2;
          assert_eq!(result, 4);
      }
  }
  ```
  prints
  ```sh
  running 1 test
  test tests::it_works ... ok
  
  test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
  
     Doc-tests <package name>
  
  running 0 tests
  
  test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s
  
  ```
  
  ``0 measured`` is for benchmark Tests, which is only available on [[Nightly Rust]] right now. 
  ``0 ignored``  as we can ignore tests
  ``0 filtered out`` as we can specify only certain tests to run by passing args to``cargo test``
  
  ``Doc-tests`` are Documentation Tests
- Tests are written in 3 steps
  * Set up any needed data or state using test functions and modules.
  * Run the code we want to test with test ``cargo test``.
  * Assert the results are what we expect using [[Assert]].
- Failure
  Tests can fail if [[Assert]]s fail or if a test [[Function]] [[Thread]] dies. 
  That is, each test is ran in a different thread and if a thread becomes unresponsive or dies (like when it calls [[Panic]]) it is marked as failed.
- ``#[should_panic(expected= "<optional string message>")]``
  If this [[Macro Attribute]] annotates any test [[Function]] then the test function should [[Panic]], if it panics then the test passes. 
  By specifying the ``expected`` arg with a [[String]], we constraint the panic's message to have the provided string as a substring.
  For ex.:
  ```rust
  #[test]
  #[should_panic(expected= "hello")]
  fn yo() {
   panic!("hello world")
  } //this test passes, as it panics, and the panic message has the string "hello" as a substring
  ```
- We can return [[Result Type]] from test functions. 
  If the test function returns ``T`` of ``Result<T,E>`` then it is assumed as success and if it returns ``E`` failure is assumed.
  
  For ex.:
  ```rust
  #[test]
  fn yo() -> Result<(),String> {
   if (2==3) {
    Ok(());
   }
   else  {
   Err(String::from("naa"));
   //we can use assert like so with Result return types, assert!(something.is_err()); 
  //where is_err is defined for Result Types, we can also use the ? operator like something? which immediately 
  // returns if Err() and returns the Err
   }
  }
  ```
  As we see, we can use [[Assert]] with [[Result Type]]s of normal [[Function]]s as ``.is_err()`` returns true if Result fails, as this [[Method]] is defined on Result. 
  
  We can't use ``#[should_panic]`` with test functions that return Result Types.
- Ignoring tests
  We can ignore certain test [[Function]]s by using ``#[ignore]`` [[Macro Attribute]] .
  For ex.:
  ```rust
  #[test]
  #[ignore]
  fn yo(){ }
  //this test method will be ignored.
  ```
- [[Cargo]] test args
  ``cargo test`` compiles the code in test mode and accepts some args, then it generates a test binary/executable which also accepts some args. Both of these take args and just running ``cargo test`` runs the compiler, then the test binary. So if we wish to pass CLI args to both of them we have to use a separator token, which is ``--``.
  For ex.:
  ```sh
  cargo test --help 
  #is passed to the test runner, displays test options.
  cargo test -- --help 
  #is passed to the test binary, displays test binary options.
  ```
  * ``cargo test -- --test-threads=1``: Sets the number of [[Thread]]s tests use. By default test functions run parallelly, meaning if they mustn't depend on each other's result. By setting the ``threads`` to 1 we serially run them.
  * ``cargo test -- --show-output``: Prints any output printed by any function.
  * ``cargo test <testname>``: Runs only tests whose function names have the ``<testname>`` as substring.
  * ``cargo test -- --ignored`` :Runs only the ignore attributed tests.
  * ``cargo test -- --include-ignored``: Runs all tests including ignored ones.
  * ``cargo test --test <someTest>``: Runs the given file's tests, used with Integration Tests, so if ``<someTest>`` is ``x`` then it looks for ``<packageName>/tests/x.rs`` and runs all test functions inside ``x.rs``.
- Test Categories and locations
  * ``Unit Tests``: These test individual [[Function]]s/ [[Method]]s , covering all of them and ensuring they by theirselves work without issue. 
  * ``Integration Tests``: These test a feature, like if a [[Struct]] [[Method]] is correctly generating a new instance and doing all its startup tasks successfully. 
  
  * The UT tests are put in their own [[Modules]] in the ``src/`` folder, by convention they must be put in the same ``.rs`` file for which they cover the test cases at the top in a [[Module]]. The module name and its test functions name could be anything.
  We annotate the [[Module]] with ``#[cfg(test)]`` [[Macro Attribute]]. This attribute tells the Rust Compiler, it should only compile the given module when the ``Configuration`` of [[Cargo]] is provided with ``test``, i.e., ``cargo test`` is invoked.
  
  * UTs can freely access all private/public [[Access Modifier]] items in their file.
  For ex.:
  ```rust
  pub fn add_two(a: i32) -> i32 {
      internal_adder(a, 2)
  }
  
  fn internal_adder(a: i32, b: i32) -> i32 {
      a + b
  }
  
  #[cfg(test)]
  mod tests {
      use super::*;
  
      #[test]
      fn internal() {
          assert_eq!(4, internal_adder(2, 2));
      }
  } //works
  ```
  
  * Integration tests are ``.rs`` files defined inside ``<package name>/tests/`` directory. 
  Like so
  ```
  somepackage
  ├── Cargo.lock
  ├── Cargo.toml
  ├── src
  │   └── lib.rs
  └── tests
      └── integration_test.rs
  ```
  All ``.rs`` files directly inside ``tests/`` folder are executed by the test runner.
  They test out functionalities and are created in their own crates (outside crate root) by the test runner. So they can only access public items of our crates.
  For ex.:
  In ``x/tests/it.rs`` where ``x`` is our package name,
  ```rust
  use x;
  
  #[test]
  fn yo() { } //normal test method
  ```
  then running ``cargo test`` runs this IT.
  
  * Submodules in ITs
  By default, all files directly inside the ``tests`` folder are ran and their test [[Function]]s executed. This means if we use some modules inside our IT files then they are executed by the test runner too, to solve this we put them in a ``.../tests/<modname>/mod.rs`` file and they aren't executed separately, so the IT file can use them directly.
  For ex.:
  If we have a [[Module]] ``yo`` then we will put it in ``tests/yo/mod.rs``
  ```rust
  mod yo {}
  ```
  and use it in ``tests/someFile.rs`` with
  ```rust
  mod yo;
  ```
  
  * IT can only work with library crates and not binary crates. This is because it is its own crate and a binary crate compiles to an executable directly with its content not exposed. This is why it is recommended in [[Module System]] to use a library crate with a binary crate when we only need a binary crate.
-