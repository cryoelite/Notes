- Single line `//`, multi-line `/* */` and [[Documentation]] Comments `///` for single line or `/** **/`for multi-line.
- Documentation Comment
  id:: 66395786-a715-4697-ab54-5de52efe3dbe
  These are used to document a given [[Class]]/[[Method]]/[[Function]]/[[Variable]]/[[Interface]]/Function args etc. The ``dart doc`` [[Dart CLI]] tool generates documentation from all documentation comments for a project.
  They are different from normal comments as everything inside them is ignored except anything between `[ ]` brackets, it resolves the given variable/function/fields etc. in the current Lexical [[Scope]]. 
  Then in the documentation, the items between the `[ ]` brackets have a hyperlink to the documentation for the given type.
  
  FE:
  ```dart
  ///Uses [food]
  void eat(String food) {
  }
  
  void main(){ 
   eat("yo");
  }
  ```