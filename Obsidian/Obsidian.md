* Markdown
* Each page (stored as <\pagename>.md) is called a Note
* \# for headings, more for bigger level
* \[\<name>](list) for hyperlinks with name
* \* for unordered list
* \1. for ordered list
* \> blockquote
  FE:
> yo
* Callout
>[!tip] Callout
> A blockquote that looks cooler in obsidian

\>[\!\<callout name>] (optional -/+ for collapsible callout) <\header>
\> lines following it add body to the callout
[Available Callouts](https://help.obsidian.md/Editing+and+formatting/Callouts)
* Linking 
  \[\[Note Name/Blockquote Definition(^Optional Blockquote inside given note)(|optional name for this link)]] 
  Links another note or blockquote in the same or that note and optionally names the link whatever we want. 
  FE:
  [[Ayo]]
  [[Ayo#^46f0b4]]
  [[Ayo#^46f0b4|Yo in Ayo]]
  
* \** for bolds, \__ for italics
* Obsidian uses MathJax for math equations, which is a display engine that supports the same syntax as LaTex, LaTeX is the name of the typesetting software that has a syntax which supports maths equations, and it does so with preloaded packages. MathJax comes preloaded with Obsidian.
  [MathJax on SO](https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference)
  * tags:
    Keywords that help group notes together, in the search bar search for ``tag:#<tagname>``
    FE:
    #filename 
* \`\` Code blocks, can be 1 \` or 2\` (1 or 2 are same) or 3\` for multi line code blocks, enclose with same number of backticks
  `yo`
  ``yo``
```rust
  let v:int32=2;
  #Can specify language name too, the appropriate language server is used
```
  
  



 



