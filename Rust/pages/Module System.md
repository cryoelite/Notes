- Rust, through [[Cargo]], allows multiple files and modules for managing big programs/projects.
  Rust uses these features to allow us to organize our projects,
   *Packages*: A Cargo feature that lets us build, test, and share [[Crate]]s
  [[Crate]]s: A tree of [[Crate]]s that produces a library or executable.
  [[Module]]s and [[use]]: Lets us control the organization, [[Scope]] , and privacy of *Paths*
  *Paths*: A way of naming an item, such as a [[Struct]] , [[Function]] , or [[Module]]
- Package
  One or more [[Crate]]s that provides a set of functionality. Every package needs a [[cargo.toml]] file to define how to build the crates, it also needs at-least 1 [[Crate]], and it can contain at-most 1 library crate but any number of binary crates.
- Folder Structure
  When we create a new project with using [[Cargo]]'s ``cargo new <package-name>``, cargo creates a folder ``<package-name>`` with a [[cargo.toml]] file in it and places a folder named ``src`` in it as well. 
  
  Inside the ``src`` folder, a file ``main.rs`` is created. This file is the [[Crate]] Root of this binary crate and this binary crate is named the same as the ``<package-name>``. We can create a ``src/lib.rs`` and this file is assumed as the library crate of the package, the name of this crate is the same as the ``<package-name>`` as well. If a crate contains both ``main.rs`` and ``lib.rs`` then 2 crates are created for the package, with the same names as the ``<package-name>``. All the other binary crates can be placed in the ``src/bin`` directory, every file inside it is treated as a separate binary crate.
  
  For ex.:
  ```bash
  $ cargo new my-project
       Created binary (application) `my-project` package
  $ ls my-project
  Cargo.toml
  src
  $ ls my-project/src
  main.rs
  ```
  
  * We can define both ``lib.rs`` and ``main.rs`` for a [[Crate]]. When our crate is used as a binary crate, the ``main.rs`` file is ran and when it is used as a library crate, ``lib.rs`` is executed. So by always having the ``lib.rs`` and defining all functionality in it, and just having ``main.rs`` call the ``lib.rs`` we make a pattern that allows our crate to be directly used as a library crate as well and allows us to have our binary crate be a client of our own library crate, which  is a better architecture as we get the same behavior as any external .
  
  For ex.:
  For a ``lib.rs``,
  
  ```rust
  pub fn yo() {…}
  ```
  
  the ``main.rs`` would be,
  ```rust
  use myCrate;
  
  fn main() {
  	myCrate::yo();
  }
  
  ```
  
  and the [[cargo.toml]] would be,
  ```toml
  [package]
  name = "myCrate"
  version = "0.1.0"
  edition = "2021"
  ```
  
  All 3 steps are important. The ``lib.rs`` becomes the crate with the same name as the ``<package-name>``, hence [[Access Modifier]] rules are followed even through [[Crate]] root which is ``main.rs``. We have abstracted away the crate root’s content into a different crate hence even it must access the ``lib.rs`` like external crates would. And ``main.rs`` needs to open the path to it, the ``lib.rs`` items aren’t in ``main.rs``’ scope. 
  Lastly, the ``cargo.toml`` definition describes important metadata. Even without “edition” key, the ``main.rs`` wouldn’t be able to access the lib.rs.
- [[Cargo]]'s compilation flow
  
  First the [[Crate]] Root is opened, i.e. either the ``main.rs`` and if it’s not there then ``lib.rs``. 
  Then all the [[Module]]s defined at top indicate modules to be imported. Then to look for the code of the [[Module]], 
  the compiler first checks if the line is followed by ``{…}`` instead of a semicolon, if it is then that’s assumed as the code for it, otherwise it goes to ``src/<module-name>.rs``. If this file is not found then it goes to ``src/<module-name>/mod.rs``. 
  Then, if the [[Module]]'s crate defines its own sub-modules then they are looked for inside the Module's [[Crate]]'s directory ``src/<module-name>/<sub-module-name>.rs`` or ``src/<module-name>/<sub-module-name>/mod.rs``.
  
  For ex.:
  If we have the [[Module]] ``mod garden`` in our [[Crate]] Root ``backyard``, then it is looked for in ``src/garden.rs`` and ``src/garden/mod.rs``.  
  Then, if ``garden.rs`` [[Crate]] defines a [[Module]] ``mod vegetables;`` 
  then it is looked for in ``src/garden/vegetables.rs`` and finally ``src/garden/vegetables/mod.rs``.
  The folder structure looks like so
  ```
  backyard
  ├── Cargo.lock
  ├── Cargo.toml
  └── src
      ├── garden
      │   └── vegetables.rs
      ├── garden.rs
      └── main.rs
  ```
  
  
  Finally from the last [[Crate]] to the first one, all the crates are compiled.
-
-