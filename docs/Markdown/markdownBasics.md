# Markdown Basics

## Inline HTML

Yes you can simply write HTML inline if you can't get something to look the way you want.  As you go through all the defaults, if you ever get to a place where you need something and can't find the syntax, do it in html.

You can also include a \<style>\</style> tag to inject your own style to the doc.

## Links
Links can be expressed by square brackets [Link Text] followed by paranthesis (web URL "optionsl tool tip"). 

Example:

[Google](http://google.com "A Link to Google")

Sometimes, you don't want the URL inline with your link.  You can create a reference to a link by following the Link Text with another set of square brackets that has a token in it that will be linked to another spot:

[Google][1]

Now somewhere else in the md text you will resolve that token:

[1]: http:google.com "A link to google"

This is helpful if you need to link to the same URL multiple times.  You just create a token and reference it multiple times.

## Images

Very similar to links, except you start it with an "!" to indicate it is an image.

Example:
![Optional Alt Text](URL_to_Image "Optional Tool Tip")

You have the (URL) piece "tokenized", like you can do with links:

![Test image][image1]

[image1]: http://unsplash.it/500

You can also have an image Link to a larger version or just to a web page:

[![A Image](http://unsplash.it/500)](http://google.com) OR

[![A Image][image1]](http://google.com) OR

[<img src="http://unsplash.it/200" />](http://google.com)

You are just embedding an image tab within a link's text area.

## Code
Use three backticks (tilde key) followed by the name of the language you are using:

```javascript
var x = 100;
var y = 299;
```

If you want inline code, you simply enclose the inline code in single backticks:

The way to set a varaible is ` var x=100 `

You can also use the keyword "diff" after the three backticks for a code block and this will allow you to identify deleted bits of code (precede with -) or added bits of code (precede with +)

```diff
var x = 100;
- var y = 299;
+ var y = 200;
```

## Tables

Tables are not in the standard, so all parsers may not work with them, but you need to separate all columns with pipes and then you MUST put a row with pipe colon hyphens to designate the header.

| Dog Name | Dog Age | Color |
|:---|:---|:----:|
| Abby | 9 | black |
| Peanut | 17 | tan|


Placement of the colon determines alignment.  To the left or right, then left or right aligned.  Colon on both left and right will center.