# CSS Grid

CSS Grid allows you to layout your site/app using a Grid system.  You can then combine cells and move cells around.  Very powerful and a bit confusing!

## Grid Terminology

When defining the CSS, you first define the container using `display: grid`

```html
<style>
    .container {
        display: grid;
    }
</style>

<div class="container">
  <div class="item item-1"></div>
  <div class="item item-2"></div>
  <div class="item item-3"></div>
</div>	
```

You can see we have a **Grid Container** and **Grid Items**

**Grid Lines** are the dividing lines that make up the structure of the grid.  

**Grid Track** - A grid track is the space between two adjacent grid lines. A grid track will show you a Row or a Column.

**Grid Cell** - A single unit in the grid.

**Grid Area** - in contrast to a Grid Cell, a Grid Area is comprised of a number of Grid Cells.

So, just like flexbox you will have properties for the Container element and for all the Grid Item elements.