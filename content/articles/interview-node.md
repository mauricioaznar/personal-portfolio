---
title: Interview NodeJs
---

## What happens after you await an already awaited function?

<br />

## Statelessness in REST APIs

* As per the REST (REpresentational “State” Transfer) architecture, the server does not store any state about the client session on the server-side. This restriction is called Statelessness. Each request from the client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is therefore kept entirely on the client. client is responsible for storing and handling all application state-related information on client side.

### Json web tokens

* Using the JSON Web Token this information is signed and stored on the client. The information cannot be changed without knowing the secret, so in this design it is considered secure provided a "strong enough" secret key is used.
* The client does need to store the token and therefore carries state. However, the server does not, meaning the implementation on the server side can be considered stateless.


## What's the oAuth procedure?

* OAuth(Open Authorization) is an open standard for access granting/deligation protocol. It used as a way for Internet users to grant websites or applications access to their information on other websites but without giving them the passwords. It does not deal with authentication.
* OAuth 2.0 is a protocol that allows a user to grant limited access to their resources on one site, to another site, without having to expose their credentials.
  * Analogy 1: Many luxury cars today come with a valet key. It is a special key you give the parking attendant and unlike your regular key, will not allow the car to drive more than a mile or two. Some valet keys will not open the trunk, while others will block access to your onboard cell phone address book. Regardless of what restrictions the valet key imposes, the idea is very clever. You give someone limited access to your car with a special key, while using your regular key to unlock everything. src from auth0
  * Analogy 2: Assume, we want to fill an application form for a bank account. Here Oauth works as, instead of filling the form by applicant, bank can fill the form using Adhaar or passport.
    * Applicant i.e. Owner 
    * Bank Account is OAuth Client, they need information
    * Adhaar/Passport ID is OAuth Provider
  
  
## How do you store passwords?

* storing the representation of a password in the database is the proper thing to do. By representation I mean that you want to hash the password using a salt (which should be different for every user) and a secure 1-way algorithm and store that, throwing away the original password. Then, when you want to verify a password, you hash the value (using the same hashing algorithm and salt) and compare it to the hashed value in the database.


## What is basic authentication?


## What is CSRF prtection?

* Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.

## What happens when you call next with a parameter?

## Which are the request methods used by REST?

* POST	Create	201 (Created), 'Location' header with link to /customers/{id} containing new ID.	404 (Not Found), 409 (Conflict) if resource already exists..
* GET	Read	200 (OK), list of customers. Use pagination, sorting and filtering to navigate big lists.	200 (OK), single customer. 404 (Not Found), if ID not found or invalid.
* PUT	Update/Replace	405 (Method Not Allowed), unless you want to update/replace every resource in the entire collection.	200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid.
* PATCH	Update/Modify	405 (Method Not Allowed), unless you want to modify the collection itself.	200 (OK) or 204 (No Content). 404 (Not Found), if ID not found or invalid. 
* DELETE Delete, 405 (Method Not Allowed), unless you want to delete the whole collection—not often desirable.	200 (OK). 404 (Not Found), if ID not found or invalid.


## Difference between GET and POST

* Authors of services which use the HTTP protocol SHOULD NOT use GET based forms for the submission of sensitive data, because this will cause this data to be encoded in the Request-URI. Many existing servers, proxies, and user agents will log the request URI in some place where it might be visible to third parties. Servers can use POST-based form submission instead
* GET requests a representation of the specified resource. Note that GET should not be used for operations that cause side-effects, such as using it for taking actions in web applications. One reason for this is that GET may be used arbitrarily by robots or crawlers, which should not need to consider the side effects that a request should cause.
* POST submits data to be processed (e.g., from an HTML form) to the identified resource. The data is included in the body of the request. This may result in the creation of a new resource or the updates of existing resources or both.

## What is middleware?

* Middleware comes in between your request and business logic. It is mainly used to capture logs and enable rate limit, routing, authentication, basically whatever that is not a part of business logic. There are third-party middleware also such as body-parser and you can write your own middleware for a specific use case.


## What is the purpose of module.exports?

This is used to expose functions of a particular module or file to be used elsewhere in the project. This can be used to encapsulate all similar functions in a file which further improves the project structure.

## Http code responses

* HTTP Status Code 200 - OK
* 400 Bad Request
* 401 Unauthorized (RFC 7235)
* 403 Forbidden
* HTTP Status Code 404 - Not Found
* HTTP Status Code 500 - Internal Server Error


## What is caching?

* REST APIs allow clients to store frequently accessed data on their side instead of requesting them again and again. As a result, the app makes fewer calls, which reduces the load on the server and its latency. In turn, the application becomes more responsive and reliable.


## How do you validate incoming data?



## Authentication vs authorization

* authentication is the process of verifying who a user is, while authorization is the process of verifying what they have access to.
* a 401 Unauthorized response should be used for missing or bad authentication, and a 403 Forbidden response should be used afterwards, when the user is authenticated but isn’t authorized to perform the requested operation on the given resource.

