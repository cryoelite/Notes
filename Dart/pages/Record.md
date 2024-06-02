- Record Types are like [[Collection Type]], they're an anonymous, immutable aggregate data type that store multiple objects into a single object, are fixed-sized and typed.
  
  They are real values, so they can be stored in other [[Data Type]]s too.
   Syntax:
  ``(<type 1>, <type 2>,...<type n>)= (<value 1 of type 1>, <value of type 2>..., <value of type n>)``
  
  A record expression is a positioned, comma-delimited list of values. 
  FE:
  ```dart
  (int,int) a = (1,2);
  print(a); //prints (1,2)
  
  ```
- Fields for a record are initialized just like arguments are defined for [[Function]]s
  
  
  ```dart
  (int, int) a; 
  a = (1,2);
  ```
- To access individual elements of a record 
  We use ``<record variable>.$<int>`` where the ``<int>`` is the position of the field.
  i.e., Records expose getters for each field. No setter because records aren't mutable.
  
  
  ```dart
  var a= (1,2);
  print(a.$1); //prints 1
  ```
- Records can have named fields too
  To create we add a curly brace list with the name pairs after all the positional fields
  ```dart
  (int, {int a, bool b}) record;
  record = (2, b: false, a: 1); //and To initialize we simply define the names as we do for functions
  print(record.a); //Named fields can be accessed with their names
  ```
  
  * The named fields are part of the record's shape(the whole type), i.e., 2 records with different name for their named fields are different even if the types are same,
  FE:
  ```dart
  ({int a, int b}) recordAB = (a: 1, b: 2);
  ({int x, int y}) recordXY = (x: 3, y: 4);
  
  // Compile error! These records don't have the same type.
  // recordAB = recordXY;
  ```
  
  * Positional fields can be named too, but these names are purely for documentation and do not change anything about the record's shape,
  FE:
  ```dart
  (int a, int b) record =(1,2); //works
  print(record.a); //doesn't work, its not a named field, its a positional field which just has a name for documentation only.
  ```
- Records are structurally typed based on their fields, there's no general type declaration for them. 
  The shape of the record determines its type, and the shape is the set of its fields, their types and their names, if any.
  
  The type system is aware of the record's type at compile time itself, that is why it knows the position and names
  FE:
  ```dart
  (num, Object) pair = (42, 'a');
  
  var first = pair.$1; // Static type `num`, runtime type `int`.
  var second = pair.$2; // Static type `Object`, runtime type `String`.
  ```
- Records are compared on their shape and the values of their fields.
  Things like the order of their fields is not a part of their shape, so the order doesn't affect the equality.
  FE:
  
  ```dart
  (int, {int a, bool b}) recordA = (1, b: false, a:2);
  (int, {bool b, int a}) recordB = (1, b: false, a:2);
  
  recordB==recordA; //true
  ```
  They automatically implement [[hashCode]] and equality [[Operator]]
- Records can be used to initialize multiple [[Variable]]s too
  
  FE:
  ```dart
  (int, int) swap((int, int) record) {
    var (a, b) = record;
    return (b, a);
  }
  ```
  This uses [[Pattern Matching]] to destructure a record.
  
  *For named fields we can use the colon syntax from the [[Pattern Types]],
   ```dart
  ({int a, int b}) rec = (a: 2, b:3);
  var (:a, :b) = rec; //which creates 2 variables a and b and takes the same named fields from rec
  ```
-
-