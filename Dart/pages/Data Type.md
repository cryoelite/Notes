- Primitive Data Types
id:: 6633e18f-d9eb-4359-990e-2e27bf271b93
  Dart natively supports the following types
  * Numbers ([[int]], [[double]])
  * Strings ([[String]])
  * Booleans ([[bool]])
  * Records \( [[Record]], like (value1, value2))
  * Lists ([[List]], also known as arrays)
  * Sets ([[Set]])
  * Maps ([[Map]])
  * Runes ([[Runes]]; often replaced by the characters API)
  * Symbols ([[Symbol]])
  * The value null ([[Null]])
- ``object``
  id:: 664dc836-dc7e-4d07-9dcf-731130436a8d
  An instance of a class.
- Every data type's value is an `object`.
- We can create objects in Dart using literals, like ``'hello'`` is a [[String]] literal, ``true`` is a boolean literal. And so on.
- In dart, every Data Type is a subclass of an [[Object]].
- Since [[Variable]]s store references to objects, and all values are objects, and their types [[Object]] [[Class]]es we can use ctors to initialize values too, such as ``Map()`` for [[Map]] types.
- There are other types too, such as
  [[Enum]]
  [[Future]] and [[Stream]]: These types are used for [[Asynchrony]]
  [[Iterable]]
  [[Never]]
  [[dynamic]]
  [[void]]
- For every Data type `T` exists a nullable data type `T?` which can either have a value of type `T` or [[Null]]. That is null is not assignable to `T`, Dart enforces sound null safety. Whilst ``T`` variables can be declared as uninitialized, they can't be accessed until initialized.
  id:: 66424513-072f-492a-afd4-a43fdfb810eb
    FE:
  ```dart
  String? x; //initialied to null automatically by default
  String y; //un-initialized, needs to be initialized before being used
  ```
  uses [[Variable]]s named `x` and `y`
   * We can't access properties/[[Method]]s of a nullable data type with the normal ((6639395b-7171-49a2-a9a1-8e4c41405c90)) ``.``, we have to use the null-aware version of it.
- Number Types
  These types have the base class [[num]], we have
  [[int]] and [[double]]
- [[Collection Type]]s 
  These types collect multiple values.