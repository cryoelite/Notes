- Whilst the [[HTML]] spec has these defined, we can also refer to [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element) as it is more developer oriented than specification oriented.
- Headings
  There's 6 of these elements, ``<h1> </h1>`` to ``<h6> </h6>`` and each level has a smaller font size than the previous from 1 to 6.
  It is recommended to use sequential pairs (like h1 then h2 )than arbitrary (like h1 then h4) for [[SEO]]
- Paragraph
  ``<p> </p>``
- Lists
  2 types
  Ordered List: ``<ol> </ol>`` which assigns number to its items
  Unordered List: ``<ul> </ul>``
  
  Each item in a list is defined inside list item element``<li> </li>``.
- Links
  ``<a> </a>``
  Everything inside is hyperlinked, and the link/url is defined using the ``href="<url>"`` attribute.
- Link
  ``<link />``
  A void element, called the [external resource link element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link). This element establishes a relation between the current document and the given resource. 
  
  For ex.:
  Used to link external [[CSS]] resource
  ```html
  <link href="main.css" rel="stylesheet" />
  ``` 
  
  Can also link fonts, images, etc. any external resource.
- [[<script>]]
  This tag allows embedding executable code in an [[HTML]] Document.
  Can be inserted anywhere in the webpage.
  For ex.:
  ```html
  <!DOCTYPE HTML>
  <html>
  
  <body>
  
    <p>Before the script...</p>
  
    <script>
      alert( 'Hello, world!' );
    </script>
  
    <p>...After the script.</p>
  
  </body>
  
  </html>
  
  ```
  
  * Attributes:
  type: for ex. <script type="text/javascript">. 
  Old html4 attr to denote type of a script, with modern HTML it defines [[Javascript]] modules.
  
  Language: <script language=…>
  Old attr. Denotes the language of the script, now defaults and sticks to javascript.
  
  src: <script src="www.abc.com/something.js"></script>
  Instead of providing a body to this tag, we can reference a js file somewhere either through absolute/relative path or URL.
  The benefit of using src is that browsers can cache the individual scripts and use them instead of fetching them again, thereby reducing the amount of data used in each webpage loading.
  If both the body and src is provided, the body is ignored.