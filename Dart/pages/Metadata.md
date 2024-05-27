alias:: Annotation

- We can provide additional information about any code in Dart, this is done using Annotations, they have a syntax `@<constant value>`, where the constant value can be either ((66391976-dd8e-434f-a43a-4148f8d2757a)) or a call to a ((66396b29-b407-4b27-a417-7b34c32ffdbb)), and these define metadata about any piece of code.
  
  Dart provides `@override` used to indicate that the given code is ((66396b2f-15f1-4e6a-ad0d-024a03f9d5bd)), `@pragma` used to provide information to tools, i.e., external tools can use pragma annotations to modify their behavior, `@deprecated` and `@Deprecated("<message>")` to indicate if the given code is deprecated, the former doesn't need a message but the latter does, it is recommended to use the latter.
  
  We can define custom annotations too,
  FE:
  ```dart
  const String SOMETHING="Something"; 
  
  
  @SOMETHING //works
  class Tip{
   final String value;
   const Tip(this.value);
  }
  
  @Tip("ayo") //works
  void yo(){
  
  }
  
  @Deprecated("No more no")
  void no(){
  }
  ```
  works, uses ((66396b29-b407-4b27-a417-7b34c32ffdbb)) and ((66391976-dd8e-434f-a43a-4148f8d2757a)).
-
-