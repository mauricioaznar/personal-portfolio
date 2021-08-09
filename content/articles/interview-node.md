---
title: Interview NodeJs
---

## What is node?

* Node.js is a web application framework built on Google Chrome's JavaScript Engine(V8 Engine). Node.js comes with
  runtime environment on which a Javascript based script can be interpreted and executed (It is analogus to JVM to JAVA
  byte code). This runtime allows to execute a JavaScript code on any machine outside a browser. Because of this runtime
  of Node.js, JavaScript is now can be executed on server as well. Node.js also provides a rich library of various
  javascript modules which eases the developement of web application using Node.js to great extents.

## What do you mean by Asynchrounous API?

* All APIs of Node.js library are asynchronous that is non-blocking. It essentially means a Node.js based server never
  waits for a API to return data. Server moves to next API after calling it and a notification mechanism of Events of
  Node.js helps server to get response from the previous API call.

## What happens after you await an already awaited function?

<br />

## What do you understan by RESTful web services?

* RESTful web services are services that follow REST architecture. REST stands for Representational State Transfer and
  uses HTTP protocol (web protocol) for implementation. These services are lightweight, provide maintainability,
  scalability, support communication among multiple applications that are developed using different programming
  languages. They provide means of accessing resources present at server required for the client via the web browser by
  means of request headers, request body, response body, status codes, etc.

## What are the best practices to developp RESTful web services?

* Use JSON. Majority of programming languages have the ability to parse data into JSON
* Http methods: Using verbs in the resource (getUsers) name doesn’t contribute much information because an HTTP request
  already has what the request is doing in its HTTP method/verb. An appropriate HTTP verb should be used to represent
  the task of the API endpoint. Below are the most commonly used HTTP methods to define the verb:
  * GET - indicates get/retrieve the resource data
  * POST - indicates create new resource data
  * PUT - indicates update the existing resource data
  * DELETE - indicates remove the resource data
* Nesting hierarchies: To represent the hierarchy of resources, use the nesting in the naming convention of the
  endpoints. In case, you want to retrieve data of one object residing in another object, the endpoint should reflect
  this to communicate what is happening. For example, to get the address of an author, we can use the GET method for the
  URI /authors/:id/address' Please ensure there are no more than 2 or 3 levels of nesting as the name of the URI can
  become too long and unwieldy.
* Error Handling: error handling should be done gracefully by returning appropriate error codes the application has
  encountered. REST has defined standard HTTP Status codes that can be sent along with the response based on the
  scenario.
* Filtering and pagination: While retrieving huge resource data, it is advisable to include filtering and pagination of
  the resources. This is because returning huge data all at once can slow down the system and reduce the application
  performance. Hence, filter some items reduces the data to some extent. Pagination of data is done to ensure only some
  results are sent at a time. Doing this can increase the server performance and reduce the burden of the server
  resources.
* Security: Good security practices are a must while developing REST APIs. The client-server communication must be
  private due to the nature of data sensitivity. Hence, incorporating SSL/TLS becomes the most important step while
  developing APIs as they facilitate establishing secure communication. SSL certificates are easier to get and load on
  the server. Apart from the secure channels, we need to ensure that not everyone should be able to access the resource.
  For example, normal users should not access the data of admins or another user. Hence, role-based access controls
  should be in place to make sure only the right set of users can access the right set of data.
* Caching: Since REST supports the feature of caching, we can use this feature to cache the data in order to improve the
  application performance. Caching is done to avoid querying the database for a request repeated times. Caching makes
  data retrieval fast. However, care must be taken to ensure that the cache has updated data and not outdated ones.
  Frequent cache update measures need to be incorporated. There are many cache providers like Redis that can assist in
  caching.
* API Versioning: Versioning needs to be done in case we are planning to make any changes with the existing endpoints.
  We do not want to break communication between our application and the apps that consume our application while we are
  working on the API release. The transition has to be seamless. Semantic versioning can be followed. For example, 3.0.1
  represents 3rd major version with the first patch. Usually, in the API endpoints, we define /v1,/v2, etc at the
  beginning of the API path.

## Statelessness in REST APIs

* As per the REST (REpresentational “State” Transfer) architecture, the server does not store any state about the client
  session on the server-side. This restriction is called Statelessness. Each request from the client to server must
  contain all of the information necessary to understand the request, and cannot take advantage of any stored context on
  the server. Session state is therefore kept entirely on the client. client is responsible for storing and handling all
  application state-related information on client side.

### Json web tokens

* Using the JSON Web Token this information is signed and stored on the client. The information cannot be changed
  without knowing the secret, so in this design it is considered secure provided a "strong enough" secret key is used.
* The client does need to store the token and therefore carries state. However, the server does not, meaning the
  implementation on the server side can be considered stateless.

## What's your favourite HTTP framework and why?

* There is no right answer for this. The goal here is to understand how deeply one knows the framework she/he uses, if
  can reason about it, knows the pros, cons.

## What's the oAuth procedure?

* OAuth(Open Authorization) is an open standard for access granting/deligation protocol. It used as a way for Internet
  users to grant websites or applications access to their information on other websites but without giving them the
  passwords. It does not deal with authentication.
* OAuth 2.0 is a protocol that allows a user to grant limited access to their resources on one site, to another site,
  without having to expose their credentials.
  * Analogy 1: Many luxury cars today come with a valet key. It is a special key you give the parking attendant and
    unlike your regular key, will not allow the car to drive more than a mile or two. Some valet keys will not open the
    trunk, while others will block access to your onboard cell phone address book. Regardless of what restrictions the
    valet key imposes, the idea is very clever. You give someone limited access to your car with a special key, while
    using your regular key to unlock everything. src from auth0
  * Analogy 2: Assume, we want to fill an application form for a bank account. Here Oauth works as, instead of filling
    the form by applicant, bank can fill the form using Adhaar or passport.
    * Applicant i.e. Owner
    * Bank Account is OAuth Client, they need information
    * Adhaar/Passport ID is OAuth Provider

## How do you store passwords?

* storing the representation of a password in the database is the proper thing to do. By representation I mean that you
  want to hash the password using a salt (which should be different for every user) and a secure 1-way algorithm and
  store that, throwing away the original password. Then, when you want to verify a password, you hash the value (using
  the same hashing algorithm and salt) and compare it to the hashed value in the database.

## What is basic authentication?

## What is CSRF prtection?

* Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce
  users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same
  origin policy, which is designed to prevent different websites from interfering with each other.

## What happens when you call next with a parameter?

## Which are the request methods used by REST?

* POST Create 201 (Created), 'Location' header with link to /customers/{id} containing new ID. 404 (Not Found), 409 (
  Conflict) if resource already exists..
* GET Read 200 (OK), list of customers. Use pagination, sorting and filtering to navigate big lists. 200 (OK), single
  customer. 404 (Not Found), if ID not found or invalid.
* PUT Update/Replace 405 (Method Not Allowed), unless you want to update/replace every resource in the entire
  collection. 200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.
* PATCH Update/Modify 405 (Method Not Allowed), unless you want to modify the collection itself. 200 (OK) or 204 (No
  Content). 404 (Not Found), if ID not found or invalid.
* DELETE Delete, 405 (Method Not Allowed), unless you want to delete the whole collection—not often desirable. 200 (OK).
  404 (Not Found), if ID not found or invalid.

## Difference between GET and POST

* Authors of services which use the HTTP protocol SHOULD NOT use GET based forms for the submission of sensitive data,
  because this will cause this data to be encoded in the Request-URI. Many existing servers, proxies, and user agents
  will log the request URI in some place where it might be visible to third parties. Servers can use POST-based form
  submission instead
* GET requests a representation of the specified resource. Note that GET should not be used for operations that cause
  side-effects, such as using it for taking actions in web applications. One reason for this is that GET may be used
  arbitrarily by robots or crawlers, which should not need to consider the side effects that a request should cause.
* POST submits data to be processed (e.g., from an HTML form) to the identified resource. The data is included in the
  body of the request. This may result in the creation of a new resource or the updates of existing resources or both.

## What is middleware?

* Middleware comes in between your request and business logic. It is mainly used to capture logs and enable rate limit,
  routing, authentication, basically whatever that is not a part of business logic. There are third-party middleware
  also such as body-parser and you can write your own middleware for a specific use case.

## What is the purpose of module.exports?

This is used to expose functions of a particular module or file to be used elsewhere in the project. This can be used to
encapsulate all similar functions in a file which further improves the project structure.

## Http code responses

* HTTP Status Code 200 - OK
* 400 Bad Request
* 401 Unauthorized (RFC 7235)
* 403 Forbidden
* HTTP Status Code 404 - Not Found
* HTTP Status Code 500 - Internal Server Error

## What is caching?

* REST APIs allow clients to store frequently accessed data on their side instead of requesting them again and again. As
  a result, the app makes fewer calls, which reduces the load on the server and its latency. In turn, the application
  becomes more responsive and reliable.

## How do you validate incoming data?

## Authentication vs authorization

* authentication is the process of verifying who a user is, while authorization is the process of verifying what they
  have access to.
* a 401 Unauthorized response should be used for missing or bad authentication, and a 403 Forbidden response should be
  used afterwards, when the user is authenticated but isn’t authorized to perform the requested operation on the given
  resource.

## What is the difference between dependencies and devDependencies?

* Both are defined in the package.json. dependencies lists the packages that the project is dependent on.
  devDependencies lists the dependencies which are only required during testing and development.

## What is libuv?

* The C library that implements the event loop and all of the asynchronous behaviors.

## What is __dirname?

* A global variable which returns the absolute path of the directory containing the currently executing file.

## Which global variable can be used to access information about the app and the environment that it runs in?

* The process variable.

## What is a blocking function?

* Unlike non-blocking functions, a blocking function's execution must be completed before other statements are
  executed.?

## How are objects that generate events called and which class from the events module are they instances of?

* These objects are called event emitters and are instances of the EventEmitter class.

## What is the event loop?

* Node js is a single threaded application but it support concurrency via concept of event and callbacks. As every API
  of Node js are asynchronous and being a single thread, it uses async function calls to maintain the concurrency. Node
  uses observer pattern. Node thread keeps an event loop and whenever any task get completed, it fires the corresponding
  event which signals the event listener function to get executed.

### What will the output be?

```javascript
console.log("first");
setTimeout(() => {
  console.log("second");
}, 0);
console.log("third");
```

## What is a callback?

* Callback is an asynchronous equivalent for a function. A callback function is called at the completion of a given
  task. Node makes heavy use of callbacks. All APIs of Node are written is such a way that they supports callbacks. For
  example, a function to read a file may start reading file and return the control to execution environment immidiately
  so that next instruction can be executed. Once file I/O is complete, it will call the callback function while passing
  the callback function, the content of the file as parameter. So there is no blocking or wait for File I/O. This makes
  Node.js highly scalable, as it can process high number of request without waiting for any function to return result.

## What is the purpose of Buffer in Node?

* Buffer class is a global class and can be accessed in application without importing buffer module. A Buffer is a kind
  of an array of integers and corresponds to a raw memory allocation outside the V8 heap. A Buffer cannot be resized.

## What is difference between synchronous and asynchronous method of fs module?

* Every method in fs module have synchronous as well as asynchronous form. Asynchronous methods takes a last parameter
  as completion function callback and first parameter of the callback function is error. It is preferred to use
  asynchronous method instead of synchronous method as former never block the program execution where the latter one
  does.

## What are streams?

* Streams are objects that let you read data from a source or write data to a destination in continuous fashion.
