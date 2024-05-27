- A library is a single compilation unit, made up of a single primary file and can have many [[Part File]]s. It has it's own ((663a2f7f-ee3d-411d-aba3-2857c50d6bd6)). It provides encapsulation and privacy to its components whilst providing APIs for other libraries.
- Every dart file (including its [[Part File]]s) is a library, even if it doesn't use the `library` [[Directive]].
- `library` [[Directive]]
  Every library can define its name by giving it to `library <name>` where `name` is the library's name and it is optional, however if we define it we can add ((66395786-a715-4697-ab54-5de52efe3dbe)) to it. 
  FE:
  ```dart
  @pragma("Yo")
  library myLib;
  
  //... rest of the file
  ```
- If any identifier inside a library uses `_` prefix to its name, it is only visible inside the [[Scope]] of the library.
- Libraries can be distributed using [[Package]]s
- To open the namespace of one library in another, i.e., to import a library in the current file (and it's compilation unit), 
  we use
  ``import'<lib name/path>';``
  where we can either define the full [[Path URI]] of the other library such as `'../libs/abc.dart'`, or  use a `<package/dart>:<package name>/<path>` combination where 'package' is an identifier used for packages added through package managers like [[Pub]] 
  FE:
  ```dart
  import 'test:test/abc.dart';
  ```
  and there's a special syntax for for inbuilt `dart` libraries such as `dart:html` 
  FE:
  ```dart
  import 'dart:html';
  ```
- Prefix
  Imported libraries can be given a custom identifier/prefix to avoid namespace clashes, i.e., say we import 2 libraries and they both have the same [[Function]] ``abc()`` then it will be ambiguous for the current library to select the right [[Function]], to avoid this Dart allows putting contents of any library a specified namespace, called a `prefix`. 
  Syntax
  `import <lib> as <name>;`
  where `<name>` can be anything except [[Reserved Keywords]].
  And then to use anything from `<lib>`'s namespace we prefix `<name>.` to it.
  
  FE:
  ```dart
  import 'test:test/abc.dart' as yo;
  
  void aye(){
   var x = yo.x(); //will call `x()` in abc.dart
  }
  ```
- Importing only part of a library
  We can import specific part(s) of a library or exclude them from being imported using `show` and `hide` [[Reserved Keywords]] respectively.
  Syntax
  `import <lib> show <identifier>,<identifier2>;` to only import these parts
  `import <lib> hide <identifier>,<identifier2>;` to exclude these parts from being imported
  FE:
  ```dart
  import 'test:test/abc.dart' show x; //if x is a function then only function x() is imported from abc.dart
  ```
- Deferred/Lazy Loading of a library
  Only for [[WebApp]]s. 
  By default a library is imported at the compilation time itself and this increases the importing library size and opens the namespace at compile time itself. But we can choose to import the library on demand and hence avoid having it included in the current library, which avoids it being in the namespace until manually imported and reduces size until it is loaded.
  
  To do this we use ``deferred as <namespace>`` on an importing library like ``import '<library>' deferred as yo;`` and then when we wish to load the library, we call a [[Function]] ``loadlibrary()`` on the library, this function inserted automatically into the namespace by Dart and returns a [[Future]] which we can and ((664c94dc-087d-4269-8ac2-623a603b34a9)) to know when the library is fully loaded.
  
  FE:
  ```dart
  import 'test:test/abc.dart' deferred as yo;
  
  Future<void> ayo() async{
   await yo.loadLibrary(); //yo gets loaded
   yo.haiyoo(); //any function inside yo works
  }
  ```
  
  * We can call ``loadLibrary()`` as many  times as we want, it is loaded only once.
  * [[Data Type]]s defined in the deferred library aren't available to the loading library as it's namespace is not loaded until dynamically requested. 
  * Constants in the deferred imported library aren't constants for the importing library as it's constants aren't available at compile-time.
- Built-In Libraries
  id:: 66342d21-0997-462e-9dee-e1fe8428fe6d
-