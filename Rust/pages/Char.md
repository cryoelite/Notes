- ``char``
  A primitive type to store alphabetic values, A character in rust is a 4 byte Unicode Scalar Value stored inside ‘ ‘ (and not “ “, which is used for [[String]]s). Since it is Unicode, it can store ASCII but also other language characters and even emojis,
  in the range U+0000 to U+D7FF and U+E000 to U+10FFFF inclusive. So a value ‘❤️’ is a single char in rust.
-