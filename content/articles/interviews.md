---
title: Interviews
---

## Links to interviews

[https://epic-react-exercises.vercel.app/](https://epic-react-exercises.vercel.app/)
[https://dev.to/alexgurr/react-coding-challenges-for-interviews-beginners-1hlk](https://dev.to/alexgurr/react-coding-challenges-for-interviews-beginners-1hlk)
[https://www.reddit.com/r/reactjs/comments/frklcv/react_coding_challenges_for_interviewsbeginners/]([https://www.reddit.com/r/reactjs/comments/frklcv/react_coding_challenges_for_interviewsbeginners/])

<br />

## General

* Mention your strengths and weaknesses
* Why Do You Want to Work at This Company?
* Why Are You Leaving Your Current Job?
* What Do You Like to Do Outside of Work?
* What Are You Passionate About?
* How did work get distributed on your previous job?


<br />

## HTML 

<br />

* What is the difference between “display: none” and “visibility: hidden”?
    * When we use the attribute “visibility: hidden” for an HTML element then that element will be hidden from the webpage but still takes up space. Whereas, if we use the “display: none” attribute for an HTML element then the element will be hidden, and also it won’t take up any space on the webpage.

<br />

* What are different types of lists in HTML?
    * unordered list, ordered list and definition list
  
<br />

* What is the difference between the ‘id’ attribute and the ‘class’ attribute of HTML elements?
    * Multiple elements in HTML can have the same class value, whereas a value of id attribute of one element cannot be associated with another HTML element.

<br />

* Define multipart form data?
    * Multipart form data is one of the values of the enctype attribute. It is used to send the file data to the server-side for processing. The other valid values of the enctype attribute are text/plain and application/x-www-form-urlencoded.
  
<br />

*  What is the significance of <head> and <body> tag in HTML?
    * head tag provides the information about the document. It should always be enclosed in the <html> tag. This tag contains the metadata about the webpage and the tags which are enclosed by head tag like <link>, <meta>, <style>, <script>, etc. are not displayed on the web page. Also, there can be only 1 <head> tag in the entire Html document and will always be before the <body> tag.
    * body tag defines the body of the HTML document. It should always be enclosed in the <html> tag. All the contents which needs to be displayed on the web page like images, text, audio, video, contents, using elements like <p>, <img>, <audio>, <heading>, <video>, <div>, etc. will always be enclosed by the <body> tag. Also, there can be only 1 body element in an HTML document and will always be after the <head> tag.
  
<br />

* What are Semantic Elements?
    * Semantic elements are those which describe the particular meaning to the browser and the developer. Elements like form, table, article, figure, etc., are semantic elements.
  


<br />

## CSS

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

## Javascript

<br />

* Explain the meaning of this
    *   It has different values depending on where it is used:.
        In a method, this refers to the owner object. 
        Alone, this refers to the global object. 
        In a function, this refers to the global object. 
        In a function, in strict mode, this is undefined. 
        In an event, this refers to the element that received the event.
        Methods like call(), and apply() can refer this to any object.
        
<br />
        
* What is falsy and truthy and give some examples
    * In JavaScript, a truthy value is a value that is considered true when encountered in a Boolean context.
    * A falsy (sometimes written falsey) value is a value that is considered false when encountered in a Boolean context. 
    * false, null, undefined, 0, NaN, '', "", ``(Empty template string), document.all , 0n: BigInt
       , -0
    * Type coercion is the process of converting value from one type to another (such as string to number, object to boolean, and so on). Any type, be it primitive or an object, is a valid subject for type coercion. To recall, primitives are: number, string, boolean, null, undefined + Symbol (added in ES6).
  
<br />

Examples

```js
if (true) {

}
if ({}) {

}

if ([]) {

}

if (42) {
}

if ("0") {
}

if ("false") {
}

if (new Date()) {
}

if (-42) {
}

if (12n) {
}

if (3.14) {
}

if (-3.14) {
}

if (Infinity) {
}

if (-Infinity) {
}

```

<br />
  
* What is hoisting
    * Hoisting is JS’s default behavior of defining all the declarations at the top of the scope before code execution.
    * One of the benefits of hoisting is that it enables us to call functions before they appear in the code. JavaScript only hoists declarations, not initializations. 

<br />

Example hoisting

```js
function myFunc(){
    let foo;
    console.log(foo);
    foo = 'bar'
}
myFunc();
```

<br />

* What is a closure
    * When you declare a local variable, that variable has a scope. Generally, local variables exist only within the block or function in which you declare them.
    * A closure is a persistent scope which holds on to local variables even after the code execution has moved out of that block.
  
<br />

Example closure

```js 
outer = function() {
    let a = 1;
    return function() {
        console.log(a);
    };
}

let fnc = outer();
fnc();
```

<br />

  
* Mention different data types and how are they categorized?
    * String, Number, Boolean, Undefined, Null, Object, Array, RegExp.
    * They are categorized in primitive and non-primitive (referenced)
    * If the value is a primitive value, when you access the variable, you manipulate the actual value stored in that variable. In other words, the variable that stores a primitive value is accessed by value. The size of a primitive value is fixed, therefore, JavaScript stores the primitive value on the stack. 
    * Unlike a primitive value, when you manipulate an object, you work on the reference of that object, rather than the actual object. It means a variable that stores an object is accessed by reference. When you assign a value to a variable, the JavaScript engine will determine whether the value is a primitive or reference value.
  
<br />

Example #1

```js
let firstPerson = {name: "Mau"};

let secondPerson = firstPerson;

firstPerson.name = "Carlos";

console.log(secondPerson.name);
console.log(firstPerson.name);
```
  
<br />




## Excercises

### Photo gallery


Hacer un "pagination component" para desplegar 10 fotos en vez de las 5000 de este url (https://jsonplaceholder.typicode.com/photos)


<br />
