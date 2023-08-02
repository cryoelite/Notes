alias:: DST
aka Unsized Type

- Rust needs to know the size of any [[Data Type]] used at compile time. But there are some types like [[str]] (``str``) whose size can't be known at compile time but only at runtime, these are known as DSTs. 
  Like the size of 2 ``str``s, ``"abc"`` and ``"x"`` isn't the same, but they would be represented by the same data type. 
  Rust solves this by using special [[Reference Type]]s. That is, normal ``&T``s are just single values which are memory addresses, but DSTs are ``&T``s with atleast 2 values, a memory address value and a ``usize`` size. So, DSTs are a reference to a starting address of a contiguous segment of memory and the number of contiguous addresses used is the size of a DST. 
  Meaning ``&str`` is not a normal [[Reference Type]], but a special reference type which actually stores 2 values internally. 
  
  Since the initial size can be easily calculated, and the memory address size is known at compile time, DSTs are possible.
- DSTs are required for storing unfixed size types on the stack, for the heap we can already use [[Pointer]]s. So ``&str`` and ``Box<str>`` are both valid.
- [[Trait]]s are DSTs too, and traits as [[Trait Object]]s are also DSTs, this is why we use ``Box<dyn <trait>>`` syntax. And this is why ``&dyn <trait>`` is valid syntax too.
- ``Sized`` [[Trait]]
  is applied to all [[Generic Type]]s implicitly, this is why we can't pass DSTs to normal [[Generic Type]]s.
  Using ``?Sized`` Trait on any generic type lifts this limitation.
-