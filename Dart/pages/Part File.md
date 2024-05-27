- `part` and `part of` [[Directive]]s links files to a [[Library]] , where a single library can have many part files. These files simply allow the code to be maintained across multiple files instead of 1 big file, and they have access to the library and its other part files. After [[Compilation]], all part files are merged into the library's compilation unit.
  
  To use:
  
  In a file `myLib.dart`, filenames don't matter except for [[Path URI]]
  ```dart 
  library myLib;
  
  part 'some/other/file.dart'; //Denotes this file is a part file for this library
  //rest of the file
  ```
  And then in `file.dart` in `some/other/` directory,
  
  ```dart
  part of '../../myLib.dart'; //denotes this file is a part of this library, can use library name too but it is unrecommended as it introduce ambiguity
  
  //rest of the file
  ```
-
-