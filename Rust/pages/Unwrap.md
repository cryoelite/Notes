- ``<T obj>.unwrap()`` or ``<Tobj>.unwrap_or(<V>)``
  Works when T is [[Option Type]] or [[Result Type]].
  There are other ``unwrap`` [[Method]]s too.
- ``.unwrap()``
  if the value is an ``Ok``/``Some`` it doesn't do anything. If it is ``Err``/``None`` then it [[Panic]]s.
- ``.unwrap_or(<V value>)``
  if the value is an ``Ok``/Some it doesn't do anything. If it is ``Err``/``Some`` then it returns the given value.
- ``.unwrap_or_else(<closure>)`` 
  Takes a [[Closure]], runs it and returns its value if ``Err``/``None``.