- ``crate``
  It's the smallest compiled entity in Rust. 
  It's simply a namespace that includes a project's contents which can then have [[Access Modifier]]s and the public items can be referred to by external code when the external code imports the crate holding them. 
  
  Like our project's crate. When [[Cargo]] compiles our project, the root crate includes our entire project directory and when [[rustc]] compiles a file, the root crate includes only the ``.rs`` file's contents.  Both also compile all the crates being imported into our file/project, and then their imported files and so on. But our project's crate only contains our project/file's contents.
  
  For ex.:
  ``std::prelude`` is a crate that includes all the public [[Access Modifier]] contents of the [[Prelude]] project.
- A [[Package]] and a Crate are 2 different things
  Whilst a Package contains all the crates in a project, a crate is simply a single ``.rs`` file and it may have [[Module]]s, which can then be their own ``.rs`` files. 
  So a Package is a collection of crates, and a crate is a collection of modules.
- Crate Root
  The source file where we start the compilation, we can then define all the various [[Crate]]s/ [[Module]]s/ [[Function]]s/ [[Struct]]s/ [[Enum]]s/ [[Trait Object]]s/ [[Method]]s and their ``impl`` blocks and so on in it.
  [[Cargo]] looks for ``main.rs`` or ``lib.rs`` and the one it picks becomes the Crate Root. 
  When we use [[rustc]] on a file then it becomes the Crate Root.
  
  * We can access the crate root's contents with the ``crate`` [[Module]].
  * All the contents of the source file, including the [[Module]]s, are bundled up and put in the ``crate`` [[Module]]. This is why it's called [[Crate]] Root, since it is the rootmost [[Module]] and is the starting point for accessing any [[Module]].
- Binary and Library Crate
  A Crate can only be either a binary or a library crate. 
  
  Binary Crate: A crate that can be compiled to an executable. It needs to have a ``main`` [[Function]]. Then this crate's executable can be ran on a system.
  
  Library Crate : This crate doesn't compile to an executable, doesn't have a ``main`` [[Function]] and hence can't be directly used by a system. It's purpose is to bundle up a project's [[Module]]s, [[Function]]s etc. allowing them to be imported into other project's and then adding predefined functionality to those projects.
- ``crate`` or ``<package-name>``
  The ``crate`` is the the Crate Root or rather, it's the namespace of the crate where the compilation begins. However, when external crates are being used, then we use their ``<package-name>`` to access their contents where ``package-name`` is the name of the [[Package]]. The ``<package-name>`` namespace has all the public items in all crates of a package. 
  When our [[Package]] is used externally, Rust compiler knows ``crate`` in our crate meant our ``<package-name>``.
  
  For ex.:
  If our ``<package-name>`` is  ``X`` then
  ```rust
  mod Y {
   pub fn yo(){}
  }
  
  fn main() {
   X::Y::yo(); 
   //is the same as
   crate::Y::yo();
  }
  ```
-