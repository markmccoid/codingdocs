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

## [Whitespace](https://developer.mozilla.org/en-US/docs/Web/CSS/white-space)

You can preserve whitespace in your text by using the *white-space* css attribute.

- **normal** - will collapse all whitespace to a single character.  This is the default before.  If you have ever been flabbergasted as to why you can't put two spaces in your HTML and have it show up, it is because of this.
  This setting, however, will wrap the text within whatever container's borders it is in.
- **nowrap** - your text will be one big long line.
- **pre** - Sequences of whitespace are preserved. Lines are only broken at newline characters in the source and at *br* elements.
- **pre-wrap** - same as pre, except it will create new lines as needed to stay within container.
- **pre-line** - Sequences of whitespace are collapsed. Lines are broken at newline characters, at *br*, and as necessary to fill line boxes.



## Custom Fonts [@font-face](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face)

Multiple ways to do this, but if you have fonts on your web server, you can use the way described here to pull in those fonts and use them.

```css
@font-face {
  font-family: "MyCustomFont";
  src: url("/fonts/OpenSans-Regular-webfont.woff2") format("woff2"),
       url("/fonts/OpenSans-Regular-webfont.woff") format("woff");
}

p {
    font-family: MyCustomFont;
}
```

