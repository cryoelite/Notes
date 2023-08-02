- For individual files, we compile them with ``rustc <filename>.rs``. It generates an *.exe in windows and that can be directly run with ``./filename.exe``.
- Regardless of the approach used to generate an executable, it is always AOT (ahead-of-time) compiled, meaning, if we pass the executable to another system, it does not need to have Rust installed in order to run it. In contrast, ``*.py`` or ``*.js`` files need Python or JavaScript installed in
  order to be run as they are interpreted languages.
-