- Generally uses [[HTML]] and [[CSS]]
	- [[<script>]]:
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
-