- ``macro_rules!``
  These [[Macro]]s work much like ``Match`` and use [[Pattern Matching]]. 
  A declarative macro like ``macro_rules!`` looks for given patterns in the passed source code, and replaces the matched code with provided code.
  
  For ex.:
  ```rust
  #[macro_export]
  macro_rules! vec {
      ( $( $x:expr ),* ) => {
          {
              let mut temp_vec = Vec::new();
              $(
                  temp_vec.push($x);
              )*
              temp_vec
          }
      };
  }
  ```
  This is kind of how ``vec!`` declarative macro is defined for [[Vector]]s.
  
  ``#[macro_export]`` is an [[Macro Attribute]] itself, and it defines this macro wherever it is imported. 
  
  ``macro_rules! <macro name>`` defines the declarative macro, for a ``<macro name>``, it is called with ``<macro name>!``, the ``!`` character denotes that a declarative macro is being used.
  
  The ``(<pattern>) => {<expression>}`` syntax defines a single arm of this declarative macro, it means that only code that matches this pattern can be passed to this macro. If we define more patterns, we can have pass more variety of code to this macro. Other patterns can be found at [Docs](https://doc.rust-lang.org/reference/macros-by-example.html).
  
  In the pattern, the outer ``()``  defines the whole value passed to the macro ``$()`` defines a [[Macro Variable]], this variable will be replaced with a value when the code matches the pattern, in it ``$x:expr`` matches any Rust expression and assigns its resultant value to x. Lastly the ``,*`` say that the literal character ``,`` will be followed by an expression and ``*`` says it can be any number of times. So ``vec![1,2,3]`` will pass ``1``, ``2`` and ``3`` to ``x`` [[Macro Variable]].
  
  In the expression, we have normal Rust code and then the ``$()*`` block, this block indicates that it must be replaced itself with some concrete code after completing all patterns inside it, the pattern here is ``temp_vec.push($x)`` and here the ``$x`` is the macro variable which is replaced with its concrete value, the ``*`` means, it can be generated any number of times.