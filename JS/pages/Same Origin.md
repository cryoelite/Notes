alias:: Same Site Policy

- This policy is enforced by the [[Brower]]s to protect users from having data of one window/frame  being stolen by other windows/frames. 
  Browsers enforce this by only allowing windows/frames from same origin to be able to access other windows/frames. 
  Same origin requires 2 windows to have the same
  ``protocol://domain:port``
  
  * The only exception is when a popup is opened or [[iframe]] is created, the popup/iframe's  ``<window>.location`` property on the [[window]] can be set to another Object, this Object has a ``.href`` property which can be set to another ``"<url>"``, it can only be set not read. This effectively redirects the popup window or the iframe.
  
  * If 2 sites have the same Second-Level-[[Domain]] then they may bypass Same Origin.
  For ex.:
  ``x.site.com`` and ``y.site.com`` have the same SLDs, but by default Same Origin policy still restricts it, 
  To bypass it
  ``document.domain=site.com`` on both the windows will lift this restriction.
  
  This [[DOM Class Property]] on the ``document`` is going to be deprecated soon.
-
-
-
-