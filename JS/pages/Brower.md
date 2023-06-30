- Popup
  ``window.open("<url>", "<name>", <params Object>)`` uses [[window]] Object to do so. 
  The params Object can take properties like window's position, height, width etc. Check [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/open) for specs. 
  
  * They are blocked automatically by the browser if they are not called from a user-initiated event's handler.
  
  * The method returns a [[window]] Object, this refers to the window Object of the opened popup and we can access its [[DOM]] through here, but this access is only possible for [[Same Origin]] windows. 
  
  * Similarly, it is possible for the popup site to access its popup opener window using ``window.opener`` which returns a reference to the window Object of the opener, given [[Same Origin]].
  
  * In the popup window or the opener we can close the popup window with ``<popup window Object>.close()``. This method is available otherwise as well to [[window]] but browsers ignore it unless the popup window is being closed.
  
  * There are other methods of the window Object that work (sometimes only) on popups, moving and resizing the popup window using popup window's Object works only for popups
  There's ``.moveTo/By(x,y)``, ``.resizeTo/By(x,y)`` and even the ``resize`` [[Browser Event]] on the window Object to track these.  
  
  Scrolling works on any [[window]] Object
  There's the ``.scrollTo/by(x,y)``, ``scrollIntoView(<optional Boolean shouldbetop>)`` and even a ``scroll`` Event on the window. 
  
  * The focus and blur [[Browser Event]]s work on the window Object too, and we can do ``<popup window Object>.focus()`` and ``.blur()`` to manually do so as well. Though browsers may ignore these calls to prevent focus loop.
- Cross-Window Messaging
  The sender window sends a message using ``postMessage(<data Object>, "<target url>")`` method ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage)). The data Object is serialized but for compatibility with IE, it must be a string.
  
  Then if the target url matches a window, the window receives a ``message`` [[Browser Event]] event which has 3 properties 
  ``data``, ``origin`` and ``source``.
-
-