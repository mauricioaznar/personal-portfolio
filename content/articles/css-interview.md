---
title: Interview CSS
---

## CSS

<br />

## Mention the meaning of the position property, and some important features

* When you use position relative you are making a div that will be relative to other divs with position absolute. Absolute will basically make the div or that element float above the document. Without having to follow the current dom or what you call it.
* When you are simply using position: relative; you are not placing the div any where. But you are actually just creating a relative point for other elements if there is no relative div the position: absolute; will be following the document as relative.
* z-index now works with this element.


<br />

Example position center box

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
      vertical-align: middle;
      line-height: 100%;
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

<br />

Example position absolute


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
            vertical-align: middle;
            line-height: 100%;
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
* A CSS pseudo-element is a keyword added to a selector that lets you style a specific part of the selected element(s). They can be used for decoration (:first-line, :first-letter) or adding elements to the markup (combined with content: ...) without having to modify the markup (:before, :after).
  * :first-line and :first-letter can be used to decorate text.
  * Triangular arrows in tooltips use :before and :after. This encourages separation of concerns because the triangle is considered a part of styling and not really the DOM. It’s not really possible to draw a triangle with just CSS styles without using an additional HTML element.

<br />

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
