- Holds sequence of UTF-16 code units.
- Defined with ``' '`` or ``" "``
  FE:
  ```dart
  var yo = "Hiiiii";
  ```
- Multi-line strings can be defined with ``''' '''`` or ``""" """``
  FE:
  ```dart
  var x = """ Yoooo
  asssss 
  nyaaa"""; //puts the string in this way in x.
  ```
- Escape `'` or `"` with `\`.
- Expressions can be embedded in strings with just `$<var>` for [[Variable]]s or `${<expression>}` for expressions, this is how string interpolation works in Dart.
  FE:
  ```dart
  var x = "Yoo ${2+3}"; //is Yoo 5
  ```
- Strings can be broken into multiple lines without need to be concatenated
  FE:
  ```dart
  var x = 'Hiii'
             ' Yoo'; //puts Hiii Yoo in x
  ```
  ofcourse we can concatenate too
  ```dart
  var x = 'Hiii' +
             ' Yoo'; //puts Hiii Yoo in x
  ``` 
  works but voids [[Dart Linter Rules]]
- Raw string
  Prefixing ``r`` to any string literal. 
  Doesn't parse any expression or escape characters in it.
- Literal Strings are [[Compile-Time Constant]], and when they have interpolated values, they are still [[Compile-Time Constant]]s as long as any interpolated values in them are also compile-time constants and evaluate to a constant [[String]], [[Null]], [[num]] or a [[bool]] value.
  That is,
  
  FE:
  ```dart
  // These work in a const string.
  const aConstNum = 0;
  const aConstBool = true;
  const aConstString = 'a constant string';
  
  // These do NOT work in a const string.
  var aNum = 0;
  var aBool = true;
  var aString = 'a string';
  const aConstList = [1, 2, 3];
  
  const validConstString = '$aConstNum $aConstBool $aConstString';
  // const invalidConstString = '$aNum $aBool $aString $aConstList';
  ```
- Unicode values
  Unicode is a system that maps user-perceived characters (called grapheme clusters, FE: '😂' which we perceive as a single character is a grapheme cluster) to unique integer values, these integer values are called runes.
   
  We can directly use unicode integer values in Dart Strings with `'\uXXXX'` within `' '` or `" "` where XXXX is the 4 digit unicode integer value, if it's more or less than 4 digits we use `'\u{<value>}'`.
  
  We can use the [Characters Package](https://pub.dev/packages/characters) to manipulate grapheme clusters because unlike normal string characters, a grapheme cluster can have multiple bytes (and hence string indices) for it. 
  
  FE:
  ```dart
  import 'package:characters/characters.dart';
  
  void main() {
    var hi = 'Hi 🇩🇰';
    print(hi);
    print('The end of the string: ${hi.substring(hi.length - 1)}'); //prints ???
    print('The last character: ${hi.characters.last}'); //prints 🇩🇰
  }
  ```
  Here even though both methods are accessing the last character, one is accessing the literal last character and the other is accessing the actual user-defined/perceived last character (aka grapheme cluster).
-
-
-
-
-