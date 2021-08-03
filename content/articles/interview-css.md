---
title: Interview CSS
---

<br />

## Mention the meaning of the position property, and some important features

* When you use position relative you are making a div that will be relative to other divs with position absolute. Absolute will basically make the div or that element float above the document. Without having to follow the current dom or what you call it.
* When you are simply using position: relative; you are not placing the div any where. But you are actually just creating a relative point for other elements if there is no relative div the position: absolute; will be following the document as relative.
* z-index now works with this element.


<br />

## Explain the concept of specificity in CSS.
* Specificity is the means by which browsers decide which CSS property values are the most relevant to an element and, therefore, will be applied. Specificity is based on the matching rules which are composed of different sorts of CSS selectors.
* Specificity is a weight that is applied to a given CSS declaration, determined by the number of each selector type in the matching selector. When multiple declarations have equal specificity, the last declaration found in the CSS is applied to the element. Specificity only applies when the same element is targeted by multiple declarations. As per CSS rules, directly targeted elements will always take precedence over rules which an element inherits from its ancestor.
* Values
  * tag - 1
  * class 10
  * id - 100


```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>Specificity</title>
    <style>
      .main .group li a {
        color: red;
      }
      .main .group .active a {
        color: green;
      }
      .main li a {
        color: black;
      }
      #products_link {
        color: blue;
      }
    </style>
  </head>
  <body>  
  
    <nav class="main">
      <ul class="group">
        <li class="active"><a href="#">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#" id="products_link">Products</a></li>
        <li><a href="#" style="color: yellow">Contact</a></li>
      </ul>
    </nav>
  
  </body>
</html>
```

<br />


## Should CSS always preceed Javascript?

* CSS should be loaded first in the head followed by font stylesheets or google font stylesheets so that the layout doesn't appear broken for a while especially on slow connections.


## How does rem differ from em in CSS?

* While em is relative to the font-size of its direct or nearest parent, rem is only relative to the html (root) font-size. em gives the ability to control an area of a design. As in, scale the type in that specific area relatively. rem gives the ability to scale type across the entire page easily.

<br />

## What is the difference between “display: none” and “visibility: hidden”?

* When we use the attribute “visibility: hidden” for an HTML element then that element will be hidden from the webpage but still takes up space. Whereas, if we use the “display: none” attribute for an HTML element then the element will be hidden, and also it won’t take up any space on the webpage.

<br />


## In how many ways you can display HTML elements?
* inline: Using this we can display any block-level element as an inline element. The height and width attribute values of the element will not affect.
* block: using this, we can display any inline element as a block-level element.
* inline-block: This property is similar to inline, except by using the display as inline-block, we can actually format the element using height and width values.
* flex: It displays the container and element as a flexible structure. It follows flexbox property.
* inline-flex: It displays the flex container as an inline element while its content follows the flexbox properties.
* grid: It displays the HTML elements as a grid container.
* none: Using this property we can hide the HTML element.

<br />

## In how many ways can we position an HTML element? Or what are the permissible values of the position attribute?

* static: Default value. Here the element is positioned according to the normal flow of the document.
* absolute: Here the element is positioned relative to its parent element. The final position is determined by the values of left, right, top, bottom.
* fixed: This is similar to absolute except here the elements are positioned relative to the <html> element.
* relative: Here the element is positioned according to the normal flow of the document and positioned relative to its original/ normal position.
* initial: This resets the property to its default value.
* inherit: Here the element inherits or takes the property of its parent.

<br />

### Example position center box

<img
src="/articles/img/position-center.png"
alt="This is a picture"
style="width: 70%;"
/>

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width,initial-scale=1.0">
  <title>Center box</title>
  <style>
    html, body {
      padding: 0;
      margin: 0;
      height: 100%;
    }

    body {
      background-color: orangered;
    }

    #box {
      height: 200px;
      width: 200px;
      background-color: green;
    }
  </style>
</head>
<body>

<div id="box">.</div>
<!-- built files will be auto injected -->
</body>
</html>
```

### Solution


```css
.box {
  height: 200px;
  width: 200px;
  background-color: green;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

<br />

### Example position absolute

<img
src="/articles/img/position-absolute.png"
alt="This is a picture"
style="width: 70%;"
/>


```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width,initial-scale=1.0">
  <title>Center box</title>
  <style>
    html, body {
      padding: 0;
      margin: 0;
      height: 100%;
    }

    body {
      background-color: orangered;
    }

    .parent {
      height: 200px;
      width: 200px;
      background-color: blue;
    }

    .child {
      position: absolute;
      height: 50px;
      width: 50px;
      background-color: lightblue;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
  </style>
</head>
<body>

<div class="parent">

  <div class="child">

  </div>

</div>
<!-- built files will be auto injected -->
</body>
</html>

```

<br />


## What is the box model?

* In CSS, the term "box model" is used when talking about design and layout.
* The CSS box model is essentially a box that wraps around every HTML element. It consists of: margins, borders, padding, and the actual content. 

<br />

## What is the meaning of cascading? How do style sheets cascade?

* Elements –  The same CSS style can be applied to multiple elements to achieve the same style.
* Multiple Style One Element – Multiple styles can be applied to a particular HTML element to achieve a unique style.
* Same style, Multiple Pages – The same stylesheet can be applied to different HTML pages altogether to achieve a template styling very quickly.

<br />

##  What is the z-index in CSS?
* The z-index helps specify the stack order of positioned elements that may overlap one another. The z-index default value is zero and can take on either a positive or negative number.
* An element with a higher z-index is always stacked above than a lower index.
* Z-Index can take the following values:
  * Auto: Sets the stack order equal to its parents.
  * Number: Orders the stack order.
  * Initial: Sets this property to its default value (0).
  * Inherit: Inherits this property from its parent element.

<br />

## What are pseudo-elements in CSS?
* APseudo-elements behave in a similar way. However, they act as if you had added a whole new HTML element into the markup, rather than applying a class to existing elements. Pseudo-elements start with a double colon ::.
  * ::first-line and :first-letter can be used to decorate text.
  * Triangular arrows in tooltips use ::before and ::after. This encourages separation of concerns because the triangle is considered a part of styling and not really the DOM. It’s not really possible to draw a triangle with just CSS styles without using an additional HTML element.

<br />

## What are pseudo-classes in CSS?

* A pseudo-class is a selector that selects elements that are in a specific state, e.g. they are the first element of their type, or they are being hovered over by the mouse pointer. They tend to act as if you had applied a class to some part of your document, often helping you cut down on excess classes in your markup, and giving you more flexible, maintainable code.
  * :hover — mentioned above; this only applies if the user moves their pointer over an element, typically a link.
  * :focus — only applies if the user focuses the element using keyboard controls.
  
<br/ >

## How will you target a h2 and h3 with the same styling?

You can target multiple elements by separating the separators with a comma (,)

```css
h2, h3 {

color: blue;

} 
```

<br />

## What is the float property used for in CSS?
* The float CSS property places an element on the left or right side of its container, allowing text and inline elements to wrap around it. The element is removed from the normal flow of the page, though it still remains a part of the flow (in contrast to absolute positioning). Below is the usage of float

<br />


## What will this piece of CSS code do to an element? .container { margin: 0 auto; }

```css
div.box {
  width: 200px;
  margin: 0 auto;
}
```

<br />

* When you have specified a width on the object that you have applied margin: 0 auto to, the object will sit centrally within its parent container. Specifying auto as the second parameter basically tells the browser to automatically determine the left and right margins itself, which it does by setting them equally. It guarantees that the left and right margins will be set to the same size. The first parameter 0 indicates that the top and bottom margins will both be set to 0.


## What is the overflow property in CSS used for?

* The overflow property specifies what should happen if content overflows an element’s box. This property specifies whether to clip content or to add scrollbars when an element’s content is too big to fit in a specified area. Below are the overflow options available in CSS –
  * overflow: auto;
  * overflow: none;
  * overflow: scroll;
  * overflow: visible;

<br />

## What is the property that is used for controlling image-scroll?
* The background-attachment property sets whether a background image scrolls with the rest of the page, or is fixed. Here is an example of a background-image that will not scroll with the page (fixed):

```css
body {
  background-image: url("./img_tree.gif");
  background-repeat: no-repeat;
  background-attachment: fixed;
}

```

<br /> 

## What is responsive web design?
* Responsive design is an approach to web page creation that makes use of flexible layouts, flexible images and cascading style sheet media queries. The goal of responsive design is to build web pages that detect the visitor’s screen size and orientation and change the layout accordingly.

<br />

## How would you style an image or element to have rounded corners?
* Use the border-radius property to add rounded corners to an image. 50% will make the image circular.

```css
.image {
  border-radius: 50%;
}
```

<br />

## Explain the scenario you would use translate() instead of absolute positioning?
* Translate is a value of CSS transform. Changing transform or opacity does not trigger browser reflow or repaint but does trigger compositions; whereas changing the absolute positioning triggers reflow. Transform causes the browser to create a GPU layer for the element but changing absolute positioning properties uses the CPU. Hence translate() is more efficient and will result in shorter paint times for smoother animations.
* When using translate(), the element still occupies its original space (sort of like position: relative), unlike in changing the absolute positioning.

<br />

## Explain the difference in approach when designing a responsive website over a mobile-first strategy?
* These two approaches are not exclusive. Making a website responsive means some elements will respond by adapting its size or other functionality according to the device’s screen size, typically the viewport width, through CSS media queries.
* A mobile-first strategy is also responsive, however, it agrees we should default and define all the styles for mobile devices, and only add specific responsive rules to other devices later. Following the previous example:

<br />

## What effect would this piece of CSS code have? {box-sizing: border-box;}

```css
* {
    box-sizing: border-box;
}
```


* By default, elements have box-sizing: content-box applied, and only the content size is being accounted for.
* box-sizing: border-box changes how the width and height of elements are being calculated, border and padding are also being included in the calculation.
* The height of an element is now calculated by the content’s height + vertical padding + vertical border width.
* The width of an element is now calculated by the content’s width + horizontal padding + horizontal border width.
* Taking into account paddings and borders as part of our box model resonates better with how designers actually imagine content in grids.

<br />

## What is a CSS pre-processor? When do you recommend a pre-processor be used in a project?
* A CSS preprocessor is a program that lets you generate CSS from the preprocessor’s own unique syntax. There are many CSS preprocessors to choose from, however, most CSS preprocessors will add some features that don’t exist in pure CSS, such as mixin, nesting selector, inheritance selector, and so on. These features make the CSS structure more readable and easier to maintain.

* Advantages:
  * CSS is made more maintainable.
  * Easy to write nested selectors.
  * Variables for consistent theming. Can share theme files across different projects.
  * Mixins to generate repeated CSS.
  * Sass features like loops, lists, and maps can make configuration easier and less verbose.
  * Splitting your code into multiple files. CSS files can be split up too but doing so will require an HTTP request to download each CSS file.
* Disadvantages
  * Requires tools for preprocessing. Re-compilation time can be slow. 
