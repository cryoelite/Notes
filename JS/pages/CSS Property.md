- As defined in [CSS Properties List MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
- ``padding``
  Sets the padding.
  For ex.:
  ```css
    padding: 0 20px 20px 20px;
  ```
  Defines padding in TRBL. 
  Can specify 1 (all sides), 2 (top and bottom take first, left and right take second), 3(TRB) or 4(TRBL) values.
  
  More details: [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/padding#syntax)
- ``margin``
  Same as padding but defines the margins.
  These properties can also take a value of ``auto`` which divides the remaining space on the axis equally.
  For ex.:
  ```css
    margin: 0 auto;
  ```
  Gives 0 margin to top and bottom and then equally divides left and right space for the element.
  This ``centers`` the element horizontally.
- ``border``
  Defines the size, line style and color of the [border](https://developer.mozilla.org/en-US/docs/Web/CSS/display).
  For ex.:
  ```css
  border: 5px solid black;
  ```
  Sets the border line width as 5px, line style as solid and border line color as black.
- ``display``
  A box in CSS around any [[HTML Element]] is treated as either ``inline`` or ``block`` [Flow Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flow_layout).
  Inline basically means it is treated as it is a part of an ongoing text, like the word ``yo`` is inline in this paragraph. So text and other content simply wraps around in the text direction of an inline element.
  
  Block elements are those that are treated as Blocks, much like how this paragraph and the previous one are their own ``Blocks``.
  
  These are the outer display types, there's also the inner display types. We can set both for a given using this property. [display](https://developer.mozilla.org/en-US/docs/Web/CSS/display)
  For ex.:
  ```css
  display: block; /*Sets the outer display type to block.*/
  display: flex; /* Sets the inner display type to flex. */
  display: inline-flex; /* Sets both*/
  ```
  The inner display types define how the children are laid out, it can be either flow, grid or flex.
  
  For ex.:
  ```css
  img {
    display: block;
    margin: 0 auto;
  }
  ```
  ``centers`` an img block, img is an inline element and this turns it into a block.
- Positioning
  CSS can define where and how an element is positioned on the page. This is done using the [position](https://developer.mozilla.org/en-US/docs/Web/CSS/position) property which defines the type of position, a value from 
  ``fixed``: Fixed to the viewport. So no space is created for the element in the Document and then offset by LTRB. Can be thought of as fixed from the current window's top left.
  ``static``: Positioning according to the flow of the document, so if its ``display is ``inline/block then it applies normally. LTRB doesn't affect it.
  ``relative``: Like static, then it is offset by LTRB but without moving other elements.
  ``sticky``: Kind of like fixed but fixed to the nearest scrolling element.
  ``absolute``: Like fixed but fixed to the nearest element with the ``position`` property set and then affected by LTRB. 
  
  This property is combined with any or all of the properties ``left``, ``top``, ``right`` and ``bottom`` (LTRB) to finally determine the actual position of the [[HTML Element]].
  For ex.:
  ```css
  position: relative;
  top: 40px; 
  left: 40px;
  ```