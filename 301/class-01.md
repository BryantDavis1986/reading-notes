# Read: 01 - SMACSS and Responsive Web Design
## Shay Howe’s intro to RWD
* Responsive generally means to react quickly and positively to any change,
* adaptive means to be easily modified for a new purpose or situation, such as change
* Mobile, on the other hand, generally means to build a separate website commonly on a new domain solely for mobile users
* Responsive web design is broken down into three main components, including flexible layouts, media queries, and flexible media
* flexible layouts, is the practice of building the layout of a website with a flexible grid, capable of dynamically resizing to any width
* Flexible grids are built using relative length units, most commonly percentages or em units
* hese relative lengths are then used to declare common grid property values such as width, margin, or padding.
* Flexible layouts do not advocate the use of fixed measurement units, such as pixels or inches. Reason being, the viewport height and width continually change from device to device. Website layouts need to adapt to this change and fixed values have too many constraints. Fortunately, Ethan pointed out an easy formula to help identify the proportions of a flexible layout using relative values.
* `<div class="container">`  
  `<section>...</section>`  
  `<aside>...</aside>`  
`</div>`

* `.container {`  
 `width: 538px;`  
`}`  
`section,`  
`aside {`  
 ` margin: 10px;`  
`}`  
`section {`  
  `float: left;`  
  `width: 340px;`  
`}`  
`aside {`  
  `float: right;`  
 ` width: 158px;`  
`}`

## All About Floats
* Float is a CSS positioning property. To understand its purpose and origin, we can look to print design. In a print layout, images may be set into the page such that text wraps around them as needed. This is commonly and appropriately called “text wrap”. Here is an example of that.
* In web design, page elements with the CSS float property applied to them are just like the images in the print layout where the text flows around them. Floated elements remain a part of the flow of the web page. This is distinctly different than page elements that use absolute positioning. Absolutely positioned page elements are removed from the flow of the webpage, like when the text box in the print layout was told to ignore the page wrap. Absolutely positioned page elements will not affect the position of other elements and other elements will not affect them, whether they touch each other or not.
* `#sidebar {  `  
  `float: right;`  
  `}`  
* Aside from the simple example of wrapping text around images, floats can be used to create entire web layouts.
* ![](https://i1.wp.com/css-tricks.com/wp-content/csstricks-uploads/reflow-example-1.png?resize=540%2C177&ssl=1)
* ![](https://i1.wp.com/css-tricks.com/wp-content/csstricks-uploads/reflow-example-2.png?resize=540%2C177)
* 