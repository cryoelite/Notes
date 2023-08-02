- ``cargo``
  Rust's package manager and build system. Whilst the Rust files are compiled with [[rustc]], cargo builds the rest of the project.
- ``cargo new``
  We can create a new project with ``cargo new <folder/projectname>``.
  We can then build the project with ``cargo build``. And then run it with ``cargo run``.
- ``cargo build``
  Builds the project and creates an executable in ``src/debug/`` . Use ``--release`` flag to build in release mode. A general difference between debug build and release build is that debug build is compiled faster but runs slower whereas release compiles slower but runs faster.
- ``cargo run``
  Builds the project or skips it if there already exists a latest build and then runs the executable.
- ``cargo check``
  Checks the project by building it but not creating an executable, faster than cargo build.
- [crates.io](https://crates.io) is the Rust community’s central crate repository. [[Cargo]] searches for dependencies here by default.
- To add a crate to our project,
  * Find the crate version and name on crates.io 
  * Add ``<packge-name>=”<version>”`` to the [[cargo.toml]]
  * Then open the items of the package in the [[Scope]] with [[use]]
  
  The [[Standard Library]] crates are already present in most Rust installations, we don’t need to specify its crates in the cargo.toml but we do need to open the items in the scope (using [[use]]) for easy access.
- Cargo projects, by default, needs their ``.rs`` files inside an ``src`` folder, this folder must be at the same level as the [[cargo.toml]] file.
- Release Profile
  Cargo has 2 main profiles, ``dev`` and ``release`` profile. These profile define the configs applied to the compilation of the project. The ``dev`` profile is used when using ``cargo build`` by default and the ``release`` profile is used when we use ``cargo build --release``. 
  The primary difference between the 2 is the level of optimization applied to the executable (using ``opt-level = <number>`` in [[cargo.toml]]), the higher the level the greater the runtime performance but greater compile time too. This is why ``release`` is intended for release mode.
  
  To further configure them, in [[cargo.toml]]
  ```toml
  [profile.dev]
  opt-level = 0
  
  [profile.release]
  opt-level = 3
  ```
  This is their default optimization level.
- Publishing a [[Crate]]
  Check [here](https://doc.rust-lang.org/book/ch14-02-publishing-to-crates-io.html)
- Workspace
  By using workspace, which is a cargo feature, we can have multiple [[Package]]s in a single project. They share the same [[cargo.lock]] and ``target/`` folder, which is from common parent which is a ``workspace``.
  
  For ex.:
  ```sh
  mkdir x
  touch cargo.toml
  # creates a new folder x and a cargo.toml in it
  ```
  
  Then we make ``x``  a Workspace by modifying its [[cargo.toml]]
  ```toml
  [workspace]
  
  members = [
    y,
    z,
  ]
  ```
  By replacing the ``[package]`` block with ``[workspace]`` we make a ``cargo.toml``'s folder a workspace. Here ``y`` and ``z`` are its child [[Package]]s.
  
  To create them,
  ```sh
  cd x
  cargo new y
  # creates crate y inside x with x being its workspace and hence a cargo.lock and target/ isn't created inside y/
  cargo new z --lib
  ```
  
  Finally, we get this structure
  ```
  x
  ├── Cargo.lock
  ├── Cargo.toml
  ├── y
  │   ├── Cargo.toml
  │   └── src
  │       └── main.rs
  ├── z
  │   ├── Cargo.toml
  │   └── src
  │       └── lib.rs
  └── target
  ```
  The reason they share the ``target`` directory is because the packages in a workspace are  meant to depend on each other, by sharing the ``target`` folder they keep track of which package has been already built and hence avoid rebuilding each dependent package each time for each package.
  
  * To use a package like ``z`` in ``y`` we would do something like so
  In ``lib.rs`` in ``z``,
  ```rust
  pub fn yo() {}
  ```
  Now to use ``yo`` in ``y``,
  modify ``y``'s [[cargo.toml]],
  ```toml
  [dependencies]
  z = { path = "../z" }
  ```
  
  Now we can use ``z``'s items in ``y``, like so
  ```rust
  use z;
  
  fn main() {
   yo(); //works
  }
  ```
  
  * ``cargo test -p <package name>``
  By default cargo runs test on all crates inside a workspace, we can select a specific package with this arg.
- ``cargo install``
  Installs a binary to be used directly. By default ``cargo install`` installs a binary to ``$Home/.cargo/bin`` so we have to add that path to ``$PATH``. Only packages on crates.io that are a binary crate can be installed like this. 
  For ex.:
  ```sh
  cargo install ripgrep
  #installs rigrep, a rust package for searching files.
  ```
-
-