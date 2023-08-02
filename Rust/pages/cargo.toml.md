- A [[cargo.toml]] file is a [[Cargo]] configuration file for the project, so it defines the version of the language, it's dependencies etc., it looks like so
  ```toml
  [package]
  name = "citorust_alpha"
  version = "0.1.0"
  edition = "2021"
   
  [dependencies]
  ```
  Tom’s Obvious, Minimal Language.
- More keys for the cargo.toml config can be found at [docs](https://doc.rust-lang.org/cargo/reference/manifest.html).
- ``edition`` and ``rust-version``
  These define what [Rust Edition](https://doc.rust-lang.org/edition-guide/index.html) and version the project is compiled with. 
  The latter is optional, a Rust Edition defines the compiler's version itself, it has breaking changes, whilst the version itself is just the language of the version to be compiled to.
-
-