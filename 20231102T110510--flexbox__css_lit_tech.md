---
title:      "flexbox"
date:       2023-11-02T11:05:10-04:00
tags:       ["css", "lit", "tech"]
identifier: "20231102T110510"
---

layout items as ether a column or row

start with a container like a div and give it a display property flex
`display: flex;`
- the flex container can now controll how it's children are laidout
  * row 
    `flex-direction: row;`
	- main axis : horizontal
    - cross axis : vertical

  * column
    `flex-direction: column;`

you set the flex property to know how to distribute the width in the container.
`flex: 2`

for content
`justify-content: space-evenly`
`justify-content: space-between`
`justify-content: flex-start`
`justify-content: flex-end`
`justify-content: center`

for cross axis
`align-items: center`

flex container
``` css
.container {
  display: flex; /* this makes it a flex container */
  flex-direction: row; /* it can be a column or row */
  justify-content: flex-start; /* aligns main axis: flex-start,flex-end,center,space-between,space-around,space-evenly */
  align-items: center;  /* handles cross axis alignment: flex-start,flex-end,center,baseline(align items along text content) */
  flex-wrap: wrap:  /* wrap items when it hits a boundry: nowrap, wrap  */
  align-content: flex-start; /* how to align content incase we wrap: flex-start,flex-end,center,space-between,space-evenly*/
  column-gap: 1em; /* adds gap between columns for wraped content */
  row-gap: 1em: /* add gap between rows for wraped content */
  gap: 1em; /* shorthand for both row and column gap for wraped content*/
}
```
- creates a main axis and cross axis and their alignment


flex item

``` css
.item {
  flex-grow: 2; /* who get priority in receiving extra space */
  flex-shrink: 1; /* how fast it shrinks in comparison to others, if you set it to zero it won't shrink at all */
  flex-basis: 300px; /* bais width, override width for item, the basis width we start adding extra space to */
  flex: 1;  /* shorthand for flex-grow, flex-shrink, and flex-basis */
  align-self: flex-end; /* overide align-item from container: center,flex-start,flex-end */
  order: 3; /* you can change the order in which items appear: you shouldn't do this */
}
```

html
====

``` html
<div class="container">
  <div class="item"></div>
  <div class="item"></div>
  <div class="item"></div>
</div>
```
