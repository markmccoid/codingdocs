## Breaking Long Text that doesn't have white space
Sometimes you may have a long URL or a long filename that you want to display in a 
<pre><div></pre>, you will need some special CSS to make this work.
I have had luck with the below CSS:
```css
.breakword {
  word-wrap: break-word;
  word-break: break-word;
}
```
The CSS below is a more thorough list of the CSS properties that can deal with this issue.
```css
.dont-break-out {
  /* These are technically the same, but use both */
  overflow-wrap: break-word;
  word-wrap: break-word;

  -ms-word-break: break-all;
  /* This is the dangerous one in WebKit, as it breaks things wherever */
  word-break: break-all;
  /* Instead use this non-standard one: */
  word-break: break-word;

  /* Adds a hyphen where the word breaks, if supported (No Blink) */
  -ms-hyphens: auto;
  -moz-hyphens: auto;
  -webkit-hyphens: auto;
  hyphens: auto;
}
```
