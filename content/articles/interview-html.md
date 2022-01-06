---
title: Interview topics - HTML
---

## ¿What is the DOCTYPE?

* The declaration is not an HTML tag. It is an "information" to the browser about what document type to expect.
* The HTML document type declaration, also known as DOCTYPE, is the first line of code required in every HTML or XHTML
  document. The DOCTYPE declaration is an instruction to the web browser about what version of HTML the page is written
  in. This ensures that the web page is parsed the same way by different web browsers.

<br />

### What happens if I don't add it?

* If document type is not mentioned, browser will go to Quirks mode. Quirks mode depends upon the web browser version,
  If is older version then this will not support HTML5 tags (Example: header tag, footer tag, section tag,...)
* For example: new features & tags in HTML5 such as `<article>,< footer >, <header>,<nav>, <section>` may not be
  supported if the `<!DOCTYPE>` is not declared.

<br />

## What are semantic elements?

* Semantic elements = elements with a meaning.
  * `<article>`
  * `<aside>`
  * `<details>`
  * `<figcaption>`
  * `<figure>`
  * `<footer>`
  * `<header>`
  * `<main>`
  * `<mark>`
  * `<nav>`
  * `<section>`
  * `<summary>`
  * `<time>`
* Semantic HTML elements are those that clearly describe their meaning in a human- and machine-readable way.
* Elements such as `<header>, <footer> and <article>` are all considered semantic because they accurately describe the
  purpose of the element and the type of content that is inside them.

<br />

### What are the benefits of using semantic elements?

* First, it is much easier to read. This is probably the first thing you will notice when looking at the first block of
  code using semantic elements. This is a small example, but as a programmer you can be reading through hundreds or
  thousands of lines of code. The easier it is to read and understand that code, the easier it makes your job.
* It has greater accessibility. You are not the only one that finds semantic elements easier to understand. Search
  engines and assistive technologies (like screen readers for users with a sight impairment) are also able to better
  understand the context and content of your website, meaning a better experience for your users.

<br />

## How to increase accessibility and/or usability?

* Despite their differences, usability and accessibility sometimes go hand-in-hand. The World Wide Web Consortium
  defines accessibility as an “equivalent user experience for people with disabilities, including people with
  age-related impairments”, while usability is defined as the “design of products to be effective, efficient and
  satisfying” for end users.
* Some techniques
  * Include a Site Map
  * Use Clear and Consistent Navigation Systems
  * Validate Your Pages and Stylesheets (automatically done by text editors)
  * Use HTML Markup to Provide Semantic Information
  * Include alt text for all images and graphics that convey information.
  * Test Your Pages on a Mobile Device

<br />

## What are different types of lists in HTML?

* unordered list, ordered list and definition list

<br />

## What is the difference between the ‘id’ attribute, and the ‘class’ attribute of HTML elements?

* Multiple elements in HTML can have the same class value, whereas a value of id attribute of one element cannot be
  associated with another HTML element.

<br />

## Define multipart form data?

* Multipart form data is one of the values of the enctype attribute. It is used to send the file data to the server-side
  for processing. The other valid values of the enctype attribute are text/plain and application/x-www-form-urlencoded.

<br />

## What is the significance of `<head>` and `<body>` tag in HTML?

* head tag provides the information about the document. It should always be enclosed in the <html> tag. This tag
  contains the metadata about the webpage and the tags which are enclosed by head tag like `<link>, <meta>, <style>
  , <script>`, etc. are not displayed on the web page. Also, there can be only 1 <head> tag in the entire Html document
  and will always be before the <body> tag.
* body tag defines the body of the HTML document. It should always be enclosed in the <html> tag. All the contents which
  needs to be displayed on the web page like images, text, audio, video, contents, using elements like `<p>, <img>
  , <audio>, <heading>, <video>, <div>`, etc. will always be enclosed by the <body> tag. Also, there can be only 1 body
  element in an HTML document and will always be after the <head> tag.

<br />

## Difference between session storage, local storage and cookies

* This web storage helps in storing some of the static data in the local storage of the browser so that we do not need
  to fetch it from the server every time we need it. There is a size limit based on different browsers. This helps in
  decreasing the load time and a smooth user experience. There are two types of web storage that are used to store data
  locally in HTML5

<br />

* Local Storage - This helps in storing data that will be retained even though the user reopens the browser. It is
  stored for each webapp on different browsers.
* Session Storage - This is used for one session only. After the user closes the browser this gets deleted.
* Cookies

<br />

## Are the HTML tags and elements the same thing?

* No. HTML elements are defined by a starting tag, may contain some content and a closing tag.For
  example, `<h1>Heading 1</h1> is a HTML element but just <h1> is a starting tag and </h1>` is a closing tag.

<br />

## What are tags and attributes in HTML

* Tags are the primary component of the HTML that defines how the content will be structured/ formatted, whereas
  Attributes are used along with the HTML tags to define the characteristics of the element. For
  example,` <p align=” center”>Interview questions</p>`, in this the ‘align’ is the attribute using which we will align
  the paragraph to show in the center of the view.

<br/>

## What is the difference between the ‘id’ attribute and the ‘class’ attribute of HTML elements?

* Multiple elements in HTML can have the same class value, whereas a value of id attribute of one element cannot be
  associated with another HTML element.

<br />

## What is new about the relationship between the `<header>` and `<h1>` tags in HTML5?

* As HTML5 was all about better semantics and arrangements of the tags and elements, the <header> tag specifies the
  header section of the webpage. Unlike in previous version there was one `<h1>` element for the entire webpage, now
  this is the header for one section such as `<article>` or `<section>`. According to the HTML5 specification,
  each `<header>`
  element must at least have one `<h1>` tag.

<br />

## What is the difference between `<figure>` tag and `<img>` tag?

* The `<figure>` tag specifies the self-contained content, like diagrams, images, code snippets, etc. `<figure>` tag is
  used to semantically organize the contents of an image like image, image caption, etc., whereas the `<img>` tag is used
  to embed the picture in the HTML5 document.

<br />

## When to use scripts in the head and when to use scripts in the body?

* If the scripts contain some event-triggered functions or jquery library then we should use them in the head section.
  If the script writes the content on the page or is not inside a function then it should be placed inside the body
  section at the bottom. In short, follow below three points:
  * Place library scripts or event scripts in the head section.
  * Place normal scripts that do not write anything on the page, in the head section until there is any performance
    issue.
  * Place scripts that render something on the web page at the bottom of the body section.

<br />

## Difference between link tag `<link>` and anchor tag `<a>`?

* The anchor tag `<a>` is used to create a hyperlink to another webpage or to a certain part of the webpage and these
  links are clickable, whereas, link tag `<link>` defines a link between a document and an external resource and these are
  not clickable.

<br />

## In how many ways can we specify the CSS styles for the HTML element?

* There are three ways in which we can specify the styles for HTML elements:
  * Inline: Here we use the ‘style’ attribute inside the HTML element.
  * Internal: Here we use the `<style>` tag inside the `<head>` tag. To apply the style we bind the elements using ‘id’ or
    ‘class’ attributes.
  * External: Here we use the `<link>` tag inside `<head>` tag to reference the CSS file into our HTML code. Again the
    binding between elements and styles is done using ‘id’ or ‘class’ attributes.

<br />


## What is SEO?

* SEO stands for “search engine optimization.” It’s the process of increasing your website traffic through search engine results.
* This helps your website become more discoverable. When potential customers search terms related to your brand, they’ll have a better chance of discovering your website and becoming a customer.

<br />


## How to optimize for SEO?

* Keywords
  * Identify and target a specific keyword phrase for each page on your website. Think about how your reader might search for that specific page with search terms like
* Use alt tags
* Metadata
  * Title Metadata 
  * Description Metadata
  * Keyword Metadata
* Updating content regularly
* Site speed
* Cross-device compatibility
  * Is your website and its content equally optimised for any given screen size or device? Bear in mind that Google has stated that responsive design is its preferred method of mobile optimisation.
* Headlines and permalinks
  * The headlines for your articles should be under 55 characters 
