---
title: Interviews
---

## Links to interviews

[https://epic-react-exercises.vercel.app/](https://epic-react-exercises.vercel.app/)
[https://dev.to/alexgurr/react-coding-challenges-for-interviews-beginners-1hlk](https://dev.to/alexgurr/react-coding-challenges-for-interviews-beginners-1hlk)
[https://www.reddit.com/r/reactjs/comments/frklcv/react_coding_challenges_for_interviewsbeginners/]([https://www.reddit.com/r/reactjs/comments/frklcv/react_coding_challenges_for_interviewsbeginners/])

<br />

## General

### Mention your strengths and weaknesses
###  Why Do You Want to Work at This Company?
###  Why Are You Leaving Your Current Job?
### What Do You Like to Do Outside of Work?
### What Are You Passionate About?
### How did work get distributed on your previous job?


<br />

## HTML 

<br />

### What is the difference between “display: none” and “visibility: hidden”?
* When we use the attribute “visibility: hidden” for an HTML element then that element will be hidden from the webpage but still takes up space. Whereas, if we use the “display: none” attribute for an HTML element then the element will be hidden, and also it won’t take up any space on the webpage.

<br />

### What are different types of lists in HTML?
* unordered list, ordered list and definition list
  
<br />

### What is the difference between the ‘id’ attribute and the ‘class’ attribute of HTML elements?
* Multiple elements in HTML can have the same class value, whereas a value of id attribute of one element cannot be associated with another HTML element.

<br />

### Define multipart form data?
* Multipart form data is one of the values of the enctype attribute. It is used to send the file data to the server-side for processing. The other valid values of the enctype attribute are text/plain and application/x-www-form-urlencoded.
  
<br />

###  What is the significance of <head> and <body> tag in HTML?
* head tag provides the information about the document. It should always be enclosed in the <html> tag. This tag contains the metadata about the webpage and the tags which are enclosed by head tag like <link>, <meta>, <style>, <script>, etc. are not displayed on the web page. Also, there can be only 1 <head> tag in the entire Html document and will always be before the <body> tag.
* body tag defines the body of the HTML document. It should always be enclosed in the <html> tag. All the contents which needs to be displayed on the web page like images, text, audio, video, contents, using elements like <p>, <img>, <audio>, <heading>, <video>, <div>, etc. will always be enclosed by the <body> tag. Also, there can be only 1 body element in an HTML document and will always be after the <head> tag.
  
<br />

### What are Semantic Elements?
* Semantic elements are those which describe the particular meaning to the browser and the developer. Elements like form, table, article, figure, etc., are semantic elements.
  


<br />

## CSS

<br />

### Mention the meaning of the position property, and some important features

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



### How does rem differ from em in CSS?
* While em is relative to the font-size of its direct or nearest parent, rem is only relative to the html (root) font-size. em gives the ability to control an area of a design. As in, scale the type in that specific area relatively. rem gives the ability to scale type across the entire page easily.
<br />

## Javascript overview

<br />

### Engine architecture


<br />




<br />

### What is an engine?

[how javascript engine works](https://dzone.com/articles/how-javascript-engine-works)

* The JavaScript engine executes and compiles JavaScript into native machine code. Every major browser has developed its own JS engine: Google's Chrome uses V8, Safari uses JavaScriptCore, and Firefox  uses  SpiderMonkey.
* ECMAScript Standards is being followed by the JavaScript engines. The job of these standards is to give a definition, how JavaScript engines should work. It also tells what all features it should have.
* A JavaScript engine is a program, or an interpreter which executes JavaScript code. A JavaScript engine can be implemented as a standard interpreter, or just-in-time compiler that compiles JavaScript to bytecode in some form.

### What is the difference between JavaScript Engine and JavaScript Runtime Environment


<br />

Engines

* V8 was first designed to increase the performance of JavaScript execution inside web browsers. In order to obtain speed, V8 translates JavaScript code into more efficient machine code instead of using an interpreter. It compiles JavaScript code into machine code at execution by implementing a JIT (Just-In-Time) compiler like a lot of modern JavaScript engines do such as SpiderMonkey or Rhino (Mozilla). The main difference here is that V8 doesn’t produce bytecode or any intermediate code.

<br />

How does it work?

The work of the JavaScript engine was taking JavaScript source code. Then perform the compilation to binary instructions (machine code) that are easily understandable by CPU.

A JavaScript engine which is limited consists of a baseline compiler whose work is to do the compilation of the code in the form of intermediate representation (IR), in other words, it is called byte code and then gives the byte code to the interpreter.

Further, the interpreter takes this byte code, and then make the conversion into machine code. This machine code will further run this code on the hardware of the machine.

"It is very similar to the working of Java but the generation of byte code is done by the programmer and byte code has been shared universally not the source code."

The job of a baseline compiler is to perform the compilation of the code as fast as possible. It also generates a less-optimized byte code. As in the interpreter, it doesn’t have an optimized byte code for working with, then the speed of the application will be very slow. On the other hand, the time of the application will be very less.


<br />

### Compiler vs interpreter

* Compiler: A compiler can be defined as a program that helps in transforming a code. This transformation is done for the code that has been written in any programming language (source language) into another programming language that was targeted by you. They perform this task by translating the source code from a much high-level programming language to a low-level programming language i.e. machine language.
* Interpreter: An interpreter analyses your source code line by line and instruction by instruction and then performs the execution of the corresponding machine code on the targeted machine directly without any involvement of a third party.
  
<br />

* Today’s modern compilers of JavaScript performs the Just-In-Time (JIT) compilation that occurs at the time of running.

[Compiled versus interpreted languages](https://www.freecodecamp.org/news/compiled-versus-interpreted-languages/)

Compiled

* In a compiled language, the target machine directly translates the program. 
* Compiled languages need a “build” step – they need to be manually compiled first. You need to “rebuild” the program every time you need to make a change.
* Advantages of compiled languages: programs that are compiled into native machine code tend to be faster than interpreted code. This is because the process of translating code at run time adds to the overhead, and can cause the program to be slower overall.
* Disadvantages of compiled languages:
  * Additional time needed to complete the entire compilation step before testing 
  * Platform dependence of the generated binary code

<br />

Interpreted

* In an interpreted language, the source code is not directly translated by the target machine. Instead, a different program, aka the interpreter, reads and executes the code. 
* Interpreters run through a program line by line and execute each command.
* Advantages of interpreted languages: Interpreted languages tend to be more flexible, and often offer features like dynamic typing and smaller program size. Also, because interpreters execute the source program code themselves, the code itself is platform independent.
* Disadvantages of interpreted languages: The most notable disadvantage is typical execution speed compared to compiled languages.

<br />

Just in time compilation

* Just-in-time compilation is a method for improving the performance of interpreted programs. During execution the program may be compiled into native code to improve its performance. It is also known as dynamic compilation. 
* Traditionally there are two methods for converting source code into a form that can be run on a platform. Static compilation converts the code into a language for a specific platform. An interpreter directly executes the source code. JIT compilation attempts to use the benefits of both. While the interpreted program is being run, the JIT compiler determines the most frequently used code and compiles it to machine code. Depending on the compiler, this can be done on a method or smaller section of code.
* A Just-In-Time (JIT) compiler is a feature of the run-time interpreter, that instead of interpreting bytecode every time a method is invoked, will compile the bytecode into the machine code instructions of the running machine, and then invoke this object code instead. Ideally the efficiency of running object code will overcome the inefficiency of recompiling the program every time it runs.

<br />


### What does it mean that JavaScript is “dynamic”?

[How Javascrpit works inside the v8 engine](https://blog.sessionstack.com/how-javascript-works-inside-the-v8-engine-5-tips-on-how-to-write-optimized-code-ac089e62b12e)

Most JavaScript interpreters use dictionary-like structures (hash function based) to store the location of object property values in the memory. This structure makes retrieving the value of a property in JavaScript more computationally expensive than it would be in a non-dynamic programming language like Java or C#. In Java, all of the object properties are determined by a fixed object layout before compilation and cannot be dynamically added or removed at runtime (well, C# has the dynamic type which is another topic). As a result, the values of properties (or pointers to those properties) can be stored as a continuous buffer in the memory with a fixed-offset between each. The length of an offset can easily be determined based on the property type, whereas this is not possible in JavaScript where a property type can change during runtime.

[stack overflow](https://stackoverflow.com/questions/32476680/what-does-it-mean-that-javascript-is-dynamic)

Answer #1

* Most languages have some aspect of dynamic behaviour. Even statically typed languages can have a dynamic or variant data type that can contain different data types.
* JavaScript is called a dynamic language because it doesn't just have a few dynamic aspects, pretty much everything is dynamic.
* All variables are dynamic (both in type and existance), and even the code is dynamic. You can create new variables at runtime, and the type of variables is determined at runtime. You can create new functions at any time, or replace existing functions. When used in a browser, code is added when more script files are loaded, and you can load more files any time you like.
* Nowadays JavaScript is compiled in many implementations, and static code and static types are generated in the background. However, the behaviour is still dynamic, the compiler only generates static types when it finds that the dynamic aspects are not used for a specific object.

Answer #2

The most meaningful well-defined way in which JS is dynamic is that it's dynamically typed: the language has data types, but does not check that a program's types are "okay" until the program is actually running. The opposite is statically typed, meaning that programs' types are verified by a program that inspects their source code before they are run.

<br />

### Javascript runtime

[Javascript runtime environment](http://dolszewski.com/javascript/javascript-runtime-environment/)

* It is a single-threaded language at runtime. This means that the execution of the code is done but only one piece at a time. As the code is being executed sequentially, so any code that is taking a longer time, as usual, will block the path of other code that is required to be executed after that.
* In the web development, you don’t usually use the engine directly. The JavaScript engine works inside an environment, which provides additional features to your scripts that you can use at runtime. These can be utility libraries or APIs which allow communicating with the world surrounding the engine. An example here might be access to information about the web browser in which your script is executed. Or a notification about a mouse click.
* It’s absolutely correct that your JavaScript code is executed in a single thread. But, it doesn’t mean that the whole JavaScript runtime environment works in a single thread. The thread pool exists in JavaScript runtime. Fortunately, you don’t have to worry about thread management because the environment does it for you.


<br />

### Memory Heap and memory stack

[JavaScript's Memory Management Explained](https://felixgerschau.com/javascript-memory-management/)

* Allocating memory is the process of reserving space in memory, while releasing memory frees up space, ready to be used for another purpose.
* Memory life cycle 
  * Allocate memory: JavaScript takes care of this for us: It allocates the memory that we will need for the object we created. 
  * Use memory: Using memory is something we do explicitly in our code: Reading and writing to memory is nothing else than reading or writing from or to a variable. 
  * Release memory: This step is handled as well by the JavaScript engine. Once the allocated memory is released, it can be used for a new purpose.
* All variables first point to the stack. In case it's a non-primitive value, the stack contains a reference to the object in the heap.

<br />

#### Stack: Static memory allocation

* A stack is a data structure that JavaScript uses to store static data. Static data is data where the engine knows the size at compile time. In JavaScript, this includes primitive values (strings, numbers, booleans, undefined, and null) and references, which point to objects and functions.
* Since the engine knows that the size won't change, it will allocate a fixed amount of memory for each value.
* The process of allocating memory right before execution is known as static memory allocation.
* Because the engine allocates a fixed amount of memory for these values, there is a limit to how large primitive values can be.

<br />

#### Heap: Dynamic memory allocation

* The heap is a different space for storing data where JavaScript stores objects and functions.
* Unlike the stack, the engine doesn't allocate a fixed amount of memory for these objects. Instead, more space will be allocated as needed.
* Allocating memory this way is also called dynamic memory allocation.
* The memory of the heap is not ordered in any particular way, which is why we need to keep a reference to it in the stack. You can think of references as addresses and the objects in the heap as houses that these addresses belong to.


<br />


### Garbage collection

* Once the JavaScript engine recognizes that a given variable or function is not needed anymore, it releases the memory it occupied.
* The main issue with this is that whether or not some memory is still needed is an undecidable problem, which means that there can't be an algorithm that's able to collect all the memory that's not needed anymore in the exact moment it becomes obsolete.

#### Reference-counting garbage collection

This one is the easiest approximation. It collects the objects that have no references pointing to them.


#### Memory leaks

* Global variables
* Forgotten timers and callbacks

<br />

### Call stack

* JavaScript is a single-threaded programming language, which means it has a single Call Stack. Therefore it can do one thing at a time.
* The Call Stack is a data structure which records basically where in the program we are. If we step into a function, we put it on the top of the stack. If we return from a function, we pop off the top of the stack. That’s all the stack can do.
* Each entry in the Call Stack is called a Stack Frame.

<br />

[JavaScript Event Loop And Call Stack Explained](https://felixgerschau.com/javascript-event-loop-call-stack/)

* JavaScript can do one single thing at a time because it has only one call stack.
* The call stack is a mechanism that helps the JavaScript interpreter to keep track of the functions that a script calls.
* The maximum call stack size ranges from 10 to 50 thousand calls, so if you exceed that, it's most likely that you have an infinite loop in your code.

1. Every time a script or function calls a function, it's added to the top of the call stack. Every time the function exits, the interpreter removes it from the call stack.
1. A function either exits through a return statement or by reaching the end of the scope.
1. The order in which the stack processes each function call follows the LIFO principle (Last In, First Out).


<br />


### Concurrency 

* While JavaScript could only do one thing at a time, you can still do things concurrently in the browser. As the title already suggests, this is possible through the APIs that browsers provide.
* Let's take a look at how we make an API request, for instance. If we executed the code within the JavaScript interpreter, we wouldn't be able to do anything else until we get a response from the server. Web browsers give us APIs that we can call in our JavaScript code. The execution, however, is handled by the platform itself, which is why it won't block the call stack. They enable you to make AJAX requests or manipulate the DOM, but also a range of other things, like geo-tracking, accessing local storage, service workers, and more.

<br />

### Callback queue

* Through callbacks, web APIs allow us to run code after the execution of the API call has finished.
* The callback queue follows the FIFO order (First In, First Out), meaning that the calls are processed in the same order they've been added to the queue.


<br />


### Event loop

* The JavaScript event loop takes the first call in the callback queue and adds it to the call stack as soon as it's empty.
* JavaScript code is being run in a run-to-completion manner, meaning that if the call stack is currently executing some code, the event loop is blocked and won't add any calls from the queue until the stack is empty again.


[Event loop animated](https://felixgerschau.com/video/event-loop-animated.mp4)

<br />



### Job queue

* Queue that exclusively accepts promises
* Also known as the promise queue
* Promise queue has priority over the callback queue

<br />

### What is a promise?

* 

<br />


### Single thread vs multi thread


#### Difference between process and thread 

* Both processes and threads are independent sequences of execution. The typical difference is that threads (of the same process) run in a shared memory space, while processes run in separate memory spaces.


#### Single thread

* Single threaded processes contain the execution of instructions in a single sequence. In other words, one command is processes at a time.

#### Multi thread

* Multi threaded allow the execution of multiple parts of a program at the same time. These are lightweight processes available within the process.
* Advantages
  * Program responsiveness allows a program to run even if part of it is blocked using multithreading. This can also be done if the process is performing a lengthy operation.
* Disadvantages
  * It is difficult to handle concurrency in multithreaded processes. This may lead to complications and future problems. 
  * Identification and correction of errors is much more difficult in multithreaded processes as compared to single threaded processes.


<br />


## Javascript execution

<br />

### Creation & Execution Phases

[Javascript execution context](https://www.javascripttutorial.net/javascript-execution-context/)
<br />


#### creation phase

1. Create a global object i.e., window in the web browser or global in Node.js.
1. Create a this object binding which points to the global object above.
1. Setup a memory heap for storing variables and function references.
1. Store the function declarations in the memory heap and variables within the global execution context with the initial values as undefined.

<br />

#### execution phase

* The JavaScript engine executes the code line by line. In this phase, it assigns values to variables and executes the function calls.
* For every function call, the JavaScript engine creates a new Function Execution Context. The Function Execution Context is similar to the Global Execution Context, but instead of creating the global object, it creates the arguments object that contains a reference to all the parameters passed into the function
* To keep track of all the execution contexts including the Global Execution Context and Function Execution Contexts, the JavaScript engine uses a data structure named call stack

<br />


### execution context

[Execution context in javascript](https://medium.com/@happymishra66/execution-context-in-javascript-319dd72e8e2c#id_token=eyJhbGciOiJSUzI1NiIsImtpZCI6IjNkZjBhODMxZTA5M2ZhZTFlMjRkNzdkNDc4MzQ0MDVmOTVkMTdiNTQiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJuYmYiOjE2Mjc3Njk2MTIsImF1ZCI6IjIxNjI5NjAzNTgzNC1rMWs2cWUwNjBzMnRwMmEyamFtNGxqZGNtczAwc3R0Zy5hcHBzLmdvb2dsZXVzZXJjb250ZW50LmNvbSIsInN1YiI6IjEwNDg1MDM4NzYwNjIxODY0Njk0OSIsImVtYWlsIjoibWF1cmljaW9hem5hcjk0QGdtYWlsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJhenAiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJuYW1lIjoibWF1cmljaW8gYXpuYXIiLCJwaWN0dXJlIjoiaHR0cHM6Ly9saDMuZ29vZ2xldXNlcmNvbnRlbnQuY29tL2EtL0FPaDE0R2dsclNHZzNxN1JYTGEzb1k4bmNZY0JVT2t3a1FiUDNERlJtR2p1V1E9czk2LWMiLCJnaXZlbl9uYW1lIjoibWF1cmljaW8iLCJmYW1pbHlfbmFtZSI6ImF6bmFyIiwiaWF0IjoxNjI3NzY5OTEyLCJleHAiOjE2Mjc3NzM1MTIsImp0aSI6ImI5MGU2ZDNmMjBiOWEzMzZlZjlhZmU3OTdlY2Y1N2U0ZTg3ZDI5MzcifQ.KsmtsWbc4YG38kNanP9jG4oRgsiwKMfadBNX2zuVxck4_CY7GMkGXxvmU-uLTRu-Yt6xMCKLrYKZ8Y3BxkPuZm6P6oM27S_qWPrCoZ_PtVLqkBmYf0ru4BZGZiMjbrgnYEwGqFdyKczp3pczZlCUl7B6pTswzClLPEtMkL05B1c34utjfQepyc8Klu8yit_JpDgDOA3sCCvdyLjnoRsLYV8XsCO5KlQiO1y7mhxQkXwZDqcLdmkVsoWaFEC5Az9is7eUoX9GK7DmAhm27Twd4MhSUXbSBdLVY6s4Bb-Cgsguhdzvp2wbdJVrHZThu-lqfr5n736JDHOCBXcQ4ZPuBQ)

* Execution context (EC) is defined as the environment in which the JavaScript code is executed. By environment, I mean the value of this, variables, objects, and functions JavaScript code has access to at a particular time.
  * Global execution context (GEC): This is the default execution context in which JS code start its execution when the file first loads in the browser. All of the global code i.e. code which is not inside any function or object is executed inside the global execution context. GEC cannot be more than one because only one global environment is possible for JS code execution as the JS engine is single threaded.
  * Functional execution context (FEC): Functional execution context is defined as the context created by the JS engine whenever it finds any function call. Each function has its own execution context. It can be more than one. Functional execution context has access to all the code of the global execution context though vice versa is not applicable. While executing the global execution context code, if JS engine finds a function call, it creates a new functional execution context for that function. In the browser context, if the code is executing in strict mode value of this is undefined else it is window object in the function execution context.
  * Eval: Execution context inside eval function.
  
* Execution context stack (ECS) (call stack): Execution context stack is a stack data structure, i.e. last in first out data structure, to store all the execution stacks created during the life cycle of the script. Global execution context is present by default in execution context stack and it is at the bottom of the stack. While executing the global execution context code, if JS engines find a function call, it creates a functional execution context for that function and pushes it on top of the execution context stack. JS engine executes the function whose execution context is at the top of the execution context stack. Once all the code of the function is executed, JS engines pop out that function’s execution context and start’s executing the function which is below it.
[Execution context animation](https://miro.medium.com/max/1400/1*bDebsOuhRx9NMyvLHY2zxA.gif)

* Scope chain: The scope chain is a list of all the variable objects of functions inside which the current function exists. Scope chain also consists of the current function execution object.

<br />



### Lexical environment

* A Lexical Environment is a specification type used to define the association of Identifiers to specific variables and functions based upon the lexical nesting structure of ECMAScript code [Lexical environment](https://amnsingh.medium.com/lexical-environment-the-hidden-part-to-understand-closures-71d60efac0e0)
* it's the internal js engine construct that holds identifier-variable mapping. (here identifier refers to the name of variables/functions, and variable is the reference to actual object [including function type object] or primitive value). A lexical environment also holds a reference to a parent lexical environment. [Source](https://stackoverflow.com/questions/12599965/lexical-environment-and-function-scope) 
* Now, for every execution context -- 1) a corresponding lexical environment is created and 2) if any function is created in that execution context, reference to that lexical environment is stored at the internal property ( [[Environment]] ) of that function. So, every function tracks the lexical environment related to the execution context it was created in.
* And every lexical environment tracks its parent lexical environment (that of parent execution context). As a result, every function has a chain of lexical environments attached to it. [Note: in js a function is an object, creating a function by a statement means creating an object of type Function. So like other objects, a function can hold properties both internal and user defined]



<br /> 

#### Variable environment

* A variable environment is just the part of a lexical environment within the execution context, essentially just the variables and functions declared within the current context.

<br />

### Block scope vs function scope

* A programming language has block scope if a variable declared inside some block of code enclosed by curly braces is only visible within that block of code, and that variable is not visible outside of that particular block of code.
* Function scope means that any variables declared inside a function will be visible in the entire function in which they are defined, regardless of whether those variables are declared inside a while loop, for loop, if statement, or some other block of code within that function. Even nested functions (which are basically functions declared within other functions) can access and view variables that are declared in their outer, enclosing, function(s).
* Basically, the difference between function scope and block scope is that in a language that uses function scope, any variables declared within a function are visible anywhere within that same function. But with block scope, the visibility of variables is confined to any given block (whether it's an if statement, where/for loop, etc) enclosed by curly braces.
* var variables are ‘function scope.’ What does this mean? It means they are only available inside the function they’re created in, or if not created inside a function, they are ‘globally scoped.’. In other words, var is not limited to the curly brackets. It is the function which defines the scope.
* What are the benefits of using let and const? Rather than being scoped to the function they are scoped to the block. What is the block? A block is a set of opening and closing curly brackets.

<br />

### Scope chain

* The scope chain is used to resolve the value of variable names in javascript. Without a scope chain the Javascript engine wouldn't know which value to pick for a certain variable name if there are multiple defined at different scopes. Scope chain in javascript is lexically defined, which means that we can see what the scope chain will be by looking at the code.
* At the top of the scope chain is the global scope, which is the window object in the browser (global in NodeJS). Besides from the global scope, functions have their own scoping of variables. The scope chain can be determined by looking at where functions are defined.
* When resolving a variable, inner functions first look at their own scope. If the variable cannot be found in its own scope it will climb up the scope chain and looks for the variable name in the environment where the function was defined.


<br />


### Global context, global variables

* The global context is the fallback context in JavaScript. If you run a function and no context is set (usually by an object, which we'll cover in upcoming videos) then the fallback is the global context. In Node.js, that means an object with information related to Node. In the browser, the global context is the Window object.
* If you run a file in strict mode, then JavaScript leaves this undefined if it's not actually set. It means there is no fallback object, and if you're using this you'll get undefined. If you try to access this.someAttribute, you'll get an uncaught type error for trying to access an attribute of undefined.
* All JavaScript code executes in some environment, most commonly in a browser. The code that executes must execute in some "root" scope referred to as the global context or global scope (think of it as the main container). In your browser, this "root" scope is the window object (unique window object per tab/page/iframe).
* Relying too much on global variables can result in collisions between various scripts on the same page

<br />

#### Scope

* it's the language agnostic concept, to refer to the visibility of variables or functions to the executing code. In js a variable or function is visible to the executing code, if it is there in the current lexical environment or in the lexical-environment-chain of the enclosing function. In case of global code, the chain does not exist.

<br />

### Dynamic scope vs lexical scope


* Dynamic Scope: In dynamic scoping, you search in the local function first, then you search in the function that called the local function, then you search in the function that called that function, and so on, up the call-stack. 
* Lexical scope: is a scope that is defined at lexing time. In other words, lexical scope is based on where variables and blocks of scope are authored, by you, at write-time, and thus is set in stone by the time the lexer processes your code.
  * It is also called as Static Scope. In a lexically scoped language, the scope of an identifier is fixed to some region in the source code containing the identifier’s declaration. This means that an identifier is only accessible within that region.
  * No matter where a function is invoked from, or even how it is invoked, its lexical scope is only defined by where the function was declared.
* The Key Contrast Between Lexical and Dynamic Scoping: Lexical scope is write-time, whereas dynamic scope is run-time. Lexical scope care where a function was declared, but dynamic scope cares where a function was called from.
  
<br />

### Scope lookup (shadowing)

* Scope look-up stops once it finds the first match. The same identifier name can be specified at multiple layers of nested scope, which is called shadowing (the inner identifier shadows the outer identifier). Scope look-up always start at the innermost scope being executed at the time, and works its way outward/upward until first match and stops.

<br />



### Explain the meaning of this

* The JavaScript this keyword refers to the object it belongs to
* It has different values depending on where it is used:
    * In a method, this refers to the owner object. 
    * Alone, this refers to the global object. 
    * In a function, this refers to the global object. 
    * In a function, in strict mode, this is undefined. 
    * In an event, this refers to the element that received the event.
  
    
<br />


### call(), apply() and bind()

* bind: The bind() method creates a new function that, when called, has its this keyword set to the provided value
* call: he call() method calls a function with a given this value and arguments provided individually.
  * Accepts additional parameters as well
  * Executes the function it was called upon right away. 
  * The call() method does not make a copy of the function it is being called on.
* call() and apply() serve the exact same purpose. The only difference between how they work is that call() expects all parameters to be passed in individually, whereas apply() expects an array of all of our parameters.

### What is a callback?

* A callback is a function that's passed as an argument to another function. The callback will usually be executed after the code has finished.
* You can create callback functions yourself by writing functions that accept a function as an argument. Functions like that are also known as higher-order functions. Note that callbacks aren't by default asynchronous.

<br />

### What is hoisting
* Hoisting is JS’s default behavior of defining all the declarations at the top of the scope before code execution.
* One of the benefits of hoisting is that it enables us to call functions before they appear in the code. JavaScript only hoists declarations, not initializations.
* JavaScript Hoisting refers to the process whereby the compiler allocates memory for variable and function declarations prior to execution of the code. Declarations that are made using var are initialized with a default value of undefined. Declarations made using let and const are not initialized as part of hoisting.
* Conceptually hoisting is often presented as the compiler "splitting variable declaration and initialization, and moving (just) the declarations to the top of the code". This allows variables to appear in code before they are defined. Note however, that any variable initialization in the original code will not happen until the line of code is executed.


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



## Javascript types


### What is falsy and truthy and give some examples
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


### What is a closure
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

  
### Mention different data types and how are they categorized?
* String, Number, Boolean, Undefined, Null, Object, Array, RegExp.
* They are categorized in primitive and non-primitive (referenced)
* If the value is a primitive value, when you access the variable, you manipulate the actual value stored in that variable. In other words, the variable that stores a primitive value is accessed by value. The size of a primitive value is fixed, therefore, JavaScript stores the primitive value on the stack. 
* Unlike a primitive value, when you manipulate an object, you work on the reference of that object, rather than the actual object. It means a variable that stores an object is accessed by reference. When you assign a value to a variable, the JavaScript engine will determine whether the value is a primitive or reference value.
  
<br />


#### Example #1

```js
let firstPerson = {name: "Mau"};

let secondPerson = firstPerson;

firstPerson.name = "Carlos";

console.log(secondPerson.name);
console.log(firstPerson.name);
```
  
<br />

### What is the difference between null and undefined?
* undefined means a variable has been declared but has not yet been assigned a value 
* null is an assignment value. It can be assigned to a variable as a representation of no value


<br />


### Immutable vs mutable 

*


<br />



## React


## Excercises

### Photo gallery


Hacer un "pagination component" para desplegar 10 fotos en vez de las 5000 de este url (https://jsonplaceholder.typicode.com/photos)


<br />
