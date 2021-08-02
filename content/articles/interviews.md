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


### What is the difference between JavaScript Engine and JavaScript Runtime Environment

* Imagine a robot is playing a music:
  * The JavaScript code would be the music notes to the robot.
  * TheJavaScript engine would be the robot which can understand the notes and act on it.
  * The JavaScript runtime would be the instruments the robot can use in order to play the music.
* Imagine a robot is putting out a fire:
  * The JavaScript code would be the instructions to the robot to put out a fire.
  * The JavaScript engine would be the robot which can understand the instructions and act on it.
  * The JavaScript runtime would be the fire truck, and the water gun.

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

<br />

#### Reference-counting garbage collection

* This one is the easiest approximation. It collects the objects that have no references pointing to them.
* Limitation: Circular references. There is a limitation when it comes to circular references. In the following example, two objects are created with properties that reference one another, thus creating a cycle. They will go out of scope after the function call has completed. At that point they become unneeded and their allocated memory should be reclaimed. However, the reference-counting algorithm will not consider them reclaimable since each of the two objects has at least one reference pointing to them, resulting in neither of them being marked for garbage collection. Circular references are a common cause of memory leaks.

<br />

#### Mark-and-sweep algorithm

* This algorithm reduces the definition of "an object is no longer needed" to "an object is unreachable".
* This algorithm assumes the knowledge of a set of objects called roots. In JavaScript, the root is the global object. Periodically, the garbage collector will start from these roots, find all objects that are referenced from these roots, then all objects referenced from these, etc. Starting from the roots, the garbage collector will thus find all reachable objects and collect all non-reachable objects.
* This algorithm is an improvement over the previous one since an object having zero references is effectively unreachable. The opposite does not hold true as we have seen with circular references.

<br />

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
* Concurrency is when two or more tasks can start, run, and complete in overlapping time periods. It doesn't necessarily mean they'll ever both be running at the same instant. For example, multitasking on a single-core machine.
* Interruptability


<br />


### Parallelism 

* Parallelism is when tasks literally run at the same time, e.g., on a multicore processor.
* Independentability

<br />

### Difference between concurrency and parallelism

* [stack overflow](https://stackoverflow.com/questions/1050222/what-is-the-difference-between-concurrency-and-parallelism)

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


### Single thread vs multi thread


<br />

#### Thread

* A thread is an independent set of values for the processor registers (for a single core). Since this includes the Instruction Pointer (aka Program Counter), it controls what executes in what order. It also includes the Stack Pointer, which had better point to a unique area of memory for each thread or else they will interfere with each other.
* Threads are the software unit affected by control flow (function call, loop, goto), because those instructions operate on the Instruction Pointer, and that belongs to a particular thread. Threads are often scheduled according to some prioritization scheme (although it's possible to design a system with one thread per processor core, in which case every thread is always running and no scheduling is needed).
* In fact the value of the Instruction Pointer and the instruction stored at that location is sufficient to determine a new value for the Instruction Pointer. For most instructions, this simply advances the IP by the size of the instruction, but control flow instructions change the IP in other, predictable ways. The sequence of values the IP takes on forms a path of execution weaving through the program code, giving rise to the name "thread".

<br />

#### Process

* A computer program becomes a process when it is loaded from some store into the computer's memory and begins execution. A process can be executed by a processor or a set of processors. A process description in memory contains vital information such as the program counter which keeps track of the current position in the program (i.e. which instruction is currently being executed), registers, variable stores, file handles, signals, and so forth.

<br />

#### Difference between process and thread 

* Both processes and threads are independent sequences of execution. The typical difference is that threads (of the same process) run in a shared memory space, while processes run in separate memory spaces.
* Processes are like two people using two different computers, who use the network to share data when necessary. Threads are like two people using the same computer, who don't have to share data explicitly but must carefully take turns.

<br />

#### Single thread

* Single threaded processes contain the execution of instructions in a single sequence. In other words, one command is processes at a time.

<br />

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

<br />

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

<br />

### Types overview

<br />

* six datatypes:
  * undefined : typeof instance === "undefined"
  * Boolean : typeof instance === "boolean"
  * Number : typeof instance === "number"
  * String : typeof instance === "string"
  * BigInt : typeof instance === "bigint"
  * Symbol : typeof instance === "symbol"
  
<br />

* Structural Types:
  * Object : typeof instance === "object". Special non-data but Structural type for any constructed object instance also used as data structures: new Object, new Array, new Map, new Set, new WeakMap, new WeakSet, new Date and almost everything made with new keyword;
  * Function : a non-data structure, though it also answers for typeof operator: typeof instance === "function". This is merely a special shorthand for Functions, though every Function constructor is derived from Object constructor.
  
<br />
  
* Structural Root Primitive:
  * null : typeof instance === "object". Special primitive type having additional usage for its value: if object is not inherited, then null is shown;

<br />


### Mention different data types and how are they categorized?
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

### Primitives

* All types except objects define immutable values (that is, values which can't be changed).
* If the value is a primitive value, when you access the variable, you manipulate the actual value stored in that variable. In other words, the variable that stores a primitive value is accessed by value. The size of a primitive value is fixed, therefore, JavaScript stores the primitive value on the stack.
  * Boolean: Boolean represents a logical entity and can have two values: true and false. in JavaScript, Boolean conditionals are often used to decide which sections of code to execute (such as in if statements) or repeat (such as in for loops).
  * Null: The value null represents the intentional absence of any object value. It is one of JavaScript's primitive values and is treated as falsy for boolean operations. null expresses a lack of identification, indicating that a variable points to no object. In APIs, null is often retrieved in a place where an object can be expected but no object is relevant.
  * Undefined: A variable that has not been assigned a value has the value undefined
  * Number: The Number type is a double-precision 64-bit binary format IEEE 754 value (numbers between -(2^53 − 1) and 2^53 − 1). In addition to representing floating-point numbers, the number type has three symbolic values: +Infinity, -Infinity, and NaN ("Not a Number"). To check for the largest available value or smallest available value within ±Infinity, you can use the constants Number.MAX_VALUE or Number.MIN_VALUE.
  * BigInt: The BigInt type is a numeric primitive in JavaScript that can represent integers with arbitrary precision. With BigInts, you can safely store and operate on large integers even beyond the safe integer limit for Numbers. A BigInt is created by appending n to the end of an integer or by calling the constructor. You can obtain the safest value that can be incremented with Numbers by using the constant Number.MAX_SAFE_INTEGER. With the introduction of BigInts, you can operate with numbers beyond the Number.MAX_SAFE_INTEGER.
  * String: JavaScript's String type is used to represent textual data. It is a set of "elements" of 16-bit unsigned integer values. Each element in the String occupies a position in the String. The first element is at index 0, the next at index 1, and so on. The length of a String is the number of elements in it. Unlike some programming languages (such as C), JavaScript strings are immutable. This means that once a string is created, it is not possible to modify it. However, it is still possible to create another string based on an operation on the original string. For example:
    * A substring of the original by picking individual letters or using String.substr().
    * A concatenation of two strings using the concatenation operator (+) or String.concat().
  * Symbol type: A Symbol is a unique and immutable primitive value and may be used as the key of an Object property (see below). In some programming languages, Symbols are called "atoms".
  

<br />


### Objects

* Objects can be seen as a collection of properties. With the object literal syntax, a limited set of properties are initialized; then properties can be added and removed. Property values can be values of any type, including other objects, which enables building complex data structures. Properties are identified using key values. A key value is either a String or a Symbol value.
* There are two types of object properties which have certain attributes: The data property and the accessor property.
  * Data property: Associates a key with a value. 
  * Accessor property: Associates a key with one of two accessor functions (get and set) to retrieve or store a value,
  
* Object methods
  * Object.freeze(): Freezes an object. Other code cannot delete or change its properties. 
  * Object.keys(): Returns an array containing the names of all of the given object's own enumerable string properties.


<br />

### Arrays

[Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

* Arrays are regular objects for which there is a particular relationship between integer-keyed properties and the length property.
* Additionally, arrays inherit from Array.prototype, which provides to them a handful of convenient methods to manipulate arrays. For example, indexOf (searching a value in the array) or push (adding an element to the array), and so on. This makes Arrays a perfect candidate to represent lists or sets.



<br />


#### Check if object is array


```javascript
!!obj && obj.constructor === Array;

Array.isArray(obj)

Object.prototype.toString.call(obj) === '[object Array]'


```

<br />

#### Add an item to the end of an array


```javascript
let fruits = ['Apple', 'Banana']
let newLength = fruits.push('Orange')
// ["Apple", "Banana", "Orange"]
```


<br />

#### Remove an item from the end of an array

```javascript
let fruits = ["Apple", "Banana", "Orange"]
let last = fruits.pop() // remove Orange (from the end)
// ["Apple", "Banana"]
```

<br />

#### Add an item to the beginning of the array

```javascript
let fruits = ["Banana"]
let newLength = fruits.unshift('Strawberry') // add to the front
// ["Strawberry", "Banana"]
```

<br />

#### Find the index of an item in the array


```javascript
let fruits = ['Apple', 'Banana']

fruits.push('Mango')

// ["Strawberry", "Banana", "Mango"]

let pos = fruits.indexOf('Banana')
// 1
```

<br />

#### Remove an item by index position

```javascript

let fruits = ['Apple', 'Banana']

fruits.push('Mango')

// ["Strawberry", "Banana", "Mango"]

let removedItem = fruits.splice(pos, 1) // this is how to remove an item

// ["Strawberry", "Mango"]
```


<br />

### Pass by value vs pass by reference
[Stack overflow](https://stackoverflow.com/questions/6605640/javascript-by-reference-vs-by-value)

* Javascript is always pass by value, but when a variable refers to an object (including arrays), the "value" is a reference to the object.
* Changing the value of a variable never changes the underlying primitive or object, it just points the variable to a new primitive or object.
* However, changing a property of an object referenced by a variable does change the underlying object.

<br />

* Primitives are passed by value, and Objects are passed by "copy of a reference".
* Specifically, when you pass an object (or array) you are (invisibly) passing a reference to that object, and it is possible to modify the contents of that object, but if you attempt to overwrite the reference it will not affect the copy of the reference held by the caller - i.e. the reference itself is passed by value

<br />

```javascript
function replace(ref) {
    ref = {};           // this code does _not_ affect the object passed
}

function update(ref) {
    ref.key = 'newvalue';  // this code _does_ affect the _contents_ of the object
}

var a = { key: 'value' };
replace(a);  // a still has its original value - it's unmodfied
update(a);   // the _contents_ of 'a' are changed
```

<br />

### Strict & loose equality

[Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)

<br />

#### === - Strict Equality Comparison ("strict equality", "identity", "triple equals")

* Strict equality compares two values for equality. Neither value is implicitly converted to some other value before being compared. If the values have different types, the values are considered unequal. If the values have the same type, are not numbers, and have the same value, they're considered equal. Finally, if both values are numbers, they're considered equal if they're both not NaN and are the same value, or if one is +0 and one is -0.

```javascript
var num = 0;
var obj = new String('0');
var str = '0';

console.log(num === num); // true
console.log(obj === obj); // true
console.log(str === str); // true

console.log(num === obj); // false
console.log(num === str); // false
console.log(obj === str); // false
console.log(null === undefined); // false
console.log(obj === null); // false
console.log(obj === undefined); // false
```

#### == - Abstract Equality Comparison ("loose equality", "double equals")

*The behavior for performing loose equality using == is as follows:
  * Loose equality compares two values for equality after converting both values to a common type. After conversions (one or both sides may undergo conversions), the final equality comparison is performed exactly as === performs it.
  * Loose equality is symmetric: A == B always has identical semantics to B == A for any values of A and B (except for the order of applied conversions).
  * undefined and null are loosely equal; that is, undefined == null is true, and null == undefined is true

<br />

### Types coercion

* Type coercion is the process of converting value from one type to another (such as string to number, object to boolean, and so on). Any type, be it primitive or an object, is a valid subject for type coercion
  * When a developer expresses the intention to convert between types by writing the appropriate code, like Number(value), it’s called explicit type coercion (or type casting).
  * Since JavaScript is a weakly-typed language, values can also be converted between different types automatically, and it is called implicit type coercion. It usually happens when you apply operators to values of different types, like
    1 == null, 2/’5', null + new Date(), or it can be triggered by the surrounding context, like with if (value) {…}, where value is coerced to boolean.
* 3 types of coercion
  * String: To explicitly convert values to a string apply the String() function. Implicit coercion is triggered by the binary + operator, when any operand is a string
  * Boolean: To explicitly convert a value to a boolean apply the Boolean() function. Implicit conversion happens in logical context, or is triggered by logical operators ( || && !).
  * Numeric: For an explicit conversion just apply the Number() function,
    * comparison operators (>, <, <=,>=)
    * bitwise operators ( | & ^ ~)
    * arithmetic operators (- + * / % ). Note, that binary+ does not trigger numeric conversion, when any operand is a string.
    * unary + operator
    * loose equality operator == (incl. !=).
    * Note that == does not trigger numeric conversion when both operands are strings.

<br />


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

### What is the difference between null and undefined?

* undefined means a variable has been declared but has not yet been assigned a value 
* null is an assignment value. It can be assigned to a variable as a representation of no value
* As we can see the meaning of every primitive type is obvious except of undefined and null which are almost the same. This happens as the concept of Time is strictly connected with the purpose of algorithms. We can purport something that does not yet exist or does not exist anymore: undefined. But when we wish to be able to represent something that exists being empty, we have to invent another keyword. And that is what null stands for: the beginning of structural meaning.

<br />

## Modern futures (ES6+)
 
<br />

### var, const & let

* var
    * function scope or globally scoped
      * The scope is global when a var variable is declared outside a function. This means that any variable that is declared with var outside a function block is available for use in the whole window.
      * var is function scoped when it is declared within a function. This means that it is available and can be accessed only within that function.
    * variables can be re-declared and updated
    * hoisted and initialized with undefined
* let
    * block scope, A block is a chunk of code bounded by {}. A block lives in curly braces. Anything within curly braces is a block.
    * let can be updated but not re-declared. However, if the same variable is defined in different scopes, there will be no error
    * let declarations are hoisted to the top. Unlike var which is initialized as undefined, the let keyword is not initialized. So if you try to use a let variable before declaration, you'll get a Reference Error.
* const
    * block scope,
    * Cant be updated or re-declared. This behavior is somehow different when it comes to objects declared with const. While a const object cannot be updated, the properties of this objects can be updated.
    * const declarations are hoisted to the top but are not initialized.
    * While var and let can be declared without being initialized, const must be initialized during declaration.

<br />


### Arrow functions 

* This is the new arrow syntax of ES6. It differs by the treatment of this: function gets a this according to the calling context (traditional semantics), but the arrow functions keep the this of the context of definition.
* Arrow functions bind this lexically, bind return in the Block body case so it returns from the immediately enclosing arrow function, and preclude break and continue from referencing statements outside the immediately enclosing arrow function.

<br />

### Array functions

[Array methods](https://www.javatpoint.com/es6-array-methods)

#### ES6 >=

1. 	Array.from()	It converts array-like values and iterable values into arrays.	ECMAScript 6
2.	Array.of()	It creates an instance from a variable number of arguments instead of the number of arguments or type of arguments.	ECMAScript 6
3.	Array.prototype.copyWithin()	It copies the part of an array to a different location within the same array.	ECMAScript 6
4.	Array.prototype.find()	It finds a value from an array, based on the specific criteria that are passed to this method.	ECMAScript 6
5.	Array.prototype.findIndex()	The Array.prototype.findIndex() returns the index of the first element of the given array that satisfies the given condition.	ECMAScript 6
6.	Array.prototype.entries()	It returns an array iterator object, which can be used to loop through keys and values of arrays.	ECMAScript 6
7.	Array.prototype.keys()	It returns an array iterator object along with the keys of the array.	ECMAScript 6
8.	Array.prototype.values()	it provides the value of each key.	ECMAScript 6
9.	Array.prototype.fill()	It fills the specified array elements with a static value	ECMAScript 6

<br />


#### ES6 <

1.	concat()	This method returns a new array object which contains two or more merged arrays.	ECMAScript Version 1
2.	join()	This method joins the array elements as a string.
3.	pop()	This method is used to remove and return the last element of an array.
4.	push()	The push() adds one or more elements at the end of an array.
5.	reverse()	This method reverses the elements of the given array.
6.	shift()	This method is used to remove and return the first element of an array.
7.	slice()	This method returns a new array that contains a copy of the part of the given array.
8.	sort()	This method returns the element of the given array in sorted order.
9.	toString()	This method returns the strings with all array elements separated by commas.
10.	unshift()	The unshift() adds one or more elements at the starting of the given array.
11.	splice()	This method adds/removes the elements to/from the given array.
12.	every()	This method is used to determine whether all the elements of an array satisfy the provided function conditions.	ECMAScript version 5
13.	filter()	This method returns the new array, which contains the elements that pass the provided function conditions.
14.	forEach()	This method invokes the provided function once for each element of an array.
15.	isArray()	This method determines whether an object is an array or not. It returns true if the object is an array and returns false if not.
16.	indexOf()	It searches the specified element in the given array and returns the index of the first match.
17.	lastIndexOf()	It searches the specified element in the given array and returns the index of the last match.
18.	map()	It calls the specified function for every array element and returns the new array
19.	reduce()	This method reduces the array to a single value.
20.	some()	This method returns a Boolean value. Returns true if an array element passes the test else it returns false.	ECMAScript version 3



<br />

### Object/Array destructuring

* The destructuring assignment is a cool feature that came along with ES6. Destructuring is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables. That is, we can extract data from arrays and objects and assign them to variables.

<br />

#### Array destructuring

<br />

Skipping
```javascript
let [greeting,,,name] = ["Hello", "I" , "am", "Sarah"];
```

<br />

Assigning the rest of an array

```javascript
let [greeting,...intro] = ["Hello", "I" , "am", "Sarah"];
```

<br />

Using default values

```javascript
let [greeting = "hi",name = "Sarah"] = ["hello"];
```

<br />

Swapping
```javascript
let a = 3;
let b = 6;

[a,b] = [b,a];
```

<br />

#### Object destructuring

<br />

Basic

```javascript
let person = {name: "Sarah", country: "Nigeria", job: "Developer"};

let {name, country, job} = person;

let {name, country, job} = {name: "Sarah", country: "Nigeria", job: "Developer"};
```

<br />

New variable name
```javascript
let person = {name: "Sarah", country: "Nigeria", job: "Developer"};

let {name: foo, job: bar} = person;
```

<br />

Default values
```javascript
let person = {name: "Sarah", country: "Nigeria", job: "Developer"};

let {name = "myName", friend = "Annie"} = person;
```

<br />

Computed property name

```javascript
let prop = "name";

let {[prop] : foo} = {name: "Sarah", country: "Nigeria", job: "Developer"};
```


<br />

Nesting in object destructuring

```javascript
let person = {
  name: "Sarah",
  place: {
    country: "Nigeria",
    city: "Lagos" },
  friends : ["Annie", "Becky"]
};

let {name:foo,
  place: {
    country : bar,
    city : x}
} = person;
```

<br />

Rest in object destructuring

```javascript
let person = {name: "Sarah", country: "Nigeria", job: "Developer", friends: ["Annie", "Becky"]};

let {name, friends, ...others} = person;
```

<br />

### Template strings

* Template literals are string literals allowing embedded expressions. You can use multi-line strings and string interpolation features with them.
* Template literals are enclosed by the backtick (` `) (grave accent) character instead of double or single quotes.
* Template literals can contain placeholders. These are indicated by the dollar sign and curly braces (${expression}). The expressions in the placeholders and the text between the backticks (` `) get passed to a function.
* The default function just concatenates the parts into a single string. If there is an expression preceding the template literal (tag here), this is called a tagged template. In that case, the tag expression (usually a function) gets called with the template literal, which you can then manipulate before outputting.

<br />

#### Multi-line strings

```javascript
console.log(`string text line 1
string text line 2`);
// "string text line 1
// string text line 2"
```

<br />

#### Expression interpolation

```javascript
let a = 5;
let b = 10;
console.log(`Fifteen is ${a + b} and
not ${2 * a + b}.`);
// "Fifteen is 15 and
// not 20."
```

<br />

#### Nesting templates

```javascript
const classes = `header ${ isLargeScreen() ? '' :
  `icon-${item.isCollapsed ? 'expander' : 'collapser'}` }`;

```

<br />

#### Tagged templates

* Tags allow you to parse template literals with a function. The first argument of a tag function contains an array of string values. The remaining arguments are related to the expressions.
* The tag function can then perform whatever operations on these arguments you wish, and return the manipulated string. (Alternatively, it can return something completely different, as described in one of the following examples.)

<br />

```javascript
let person = 'Mike';
let age = 28;

function myTag(strings, personExp, ageExp) {
  let str0 = strings[0]; // "That "
  let str1 = strings[1]; // " is a "
  let str2 = strings[2]; // "."

  let ageStr;
  if (ageExp > 99){
    ageStr = 'centenarian';
  } else {
    ageStr = 'youngster';
  }

  // We can even return a string built using a template literal
  return `${str0}${personExp}${str1}${ageStr}${str2}`;
}

let output = myTag`That ${ person } is a ${ age }.`;

console.log(output);
// That Mike is a youngster.

```

<br />

## Closures and prototypal inheritance

<br />

### First class objects

* A first-class object is an entity within a programming language that can:
  * Appear in an expression
  * Be assigned to a variable
  * Be used as an argument
  * Be returned by a function call
  
<br />

### Functions are objects

* functions are first-class objects, because they can have properties and methods just like any other object. What distinguishes them from other objects is that functions can be called.
* In the case of a constructor called with the new keyword, the default value is the value of its this parameter. For all other functions, the default return value is undefined.
* The parameters of a function call are the function's arguments. Arguments are passed to functions by value.

<br />


### higher order functions 

* A function that accepts and/or returns another function is called a higher-order function.
* It’s higher-order because instead of strings, numbers, or booleans, it goes higher to operate on functions.
* Examples:
  * map
  * filter
  * reduce

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


### Encapsulation

* Encapsulation means information hiding. It’s about hiding as much as possible of the object’s internal parts and exposing a minimal public interface.
* The simplest and most elegant way to create encapsulation in JavaScript is using closures. A closure can be created as a function with private state. When creating many closures sharing the same private state, we create an object.

<br />

Example encapsulation

```javascript
function Stack(){
  let list = [];
  
  function push(value){ list.push(value); }
  function pop(){ return list.pop(); }
  
  return Object.freeze({
    push,
    pop
  });
}
```

<br />


### Prototypal inheritance

[Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

* When it comes to inheritance, JavaScript only has one construct: objects. Each object has a private property which holds a link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with null as its prototype. By definition, null has no prototype, and acts as the final link in this prototype chain.
* JavaScript objects are dynamic "bags" of properties (referred to as own properties). JavaScript objects have a link to a prototype object. When trying to access a property of an object, the property will not only be sought on the object but on the prototype of the object, the prototype of the prototype, and so on until either a property with a matching name is found or the end of the prototype chain is reached.
* A function in JavaScript will always have a default prototype property — with one exception: an arrow function doesn't have a default prototype property
* To check whether an object has a property defined on itself and not somewhere on its prototype chain, it is necessary to use the hasOwnProperty method which all objects inherit from Object.prototype.

<br />

#### True prototypal inheritance

```javascript
var circle = {
    radius: 5,
    create: function (radius) {
        var circle = Object.create(this);
        circle.radius = radius;
        return circle;
    },
    area: function () {
        var radius = this.radius;
        return Math.PI * radius * radius;
    },
    circumference: function () {
        return 2 * Math.PI * this.radius;
    }
};

var circle2 = circle.create(10);
```

<br />

### Class vs prototype inheritance 

[Stackoverflow](https://stackoverflow.com/questions/19633762/classical-inheritance-vs-prototypal-inheritance-in-javascript)
[Stackoverflow](https://stackoverflow.com/questions/2800964/benefits-of-prototypal-inheritance-over-classical)


<br />

## Javascript oop

<br />

### ES6 classes

* Classes are like blueprints that are used to build instances - the actual objects. Subclasses inherit earlier blueprints and add extra features. You may end up with functionality from up the chain you do not need in the sub-classed instances. An alternative to inheritance is composition where you have nested classes.

<br />

### Polymorphism

* The polymorphism is a core concept of an object-oriented paradigm that provides a way to perform a single action in different forms. It provides an ability to call the same method on different JavaScript objects. As JavaScript is not a type-safe language, we can pass any type of data members with the methods.
* Polymorphism is one of the tenets of Object Oriented Programming (OOP). It is the practice of designing objects to share behaviors and to be able to override shared behaviors with specific ones. Polymorphism takes advantage of inheritance in order to make this happen.
* In OOP everything is considered to be modeled as an object. This abstraction can be taken all the way down to nuts and bolts for a car, or as broad as simply a car type with a year, make, and model.
* To have a polymorphic car scenario there would be the base car type, and then there would subclasses which would inherit from car and provide their own behaviors on top of the basic behaviors a car would have. For example, a subclass could be TowTruck which would still have a year make and model, but might also have some extra behaviors and properties which could be as basic as a flag for IsTowing to as complicated as the specifics of the lift.


<br />

## Functional programming

<br />

### What is a pure function?

* Pure Function is a function (a block of code ) that always returns the same result if the same arguments are passed. It does not depend on any state, or data change during a program’s execution rather it only depends on its input arguments

```javascript
function calculateGST( productPrice ) {
    return productPrice * 0.05;
}
```

<br />

### Immutable vs mutable

* A mutable object is an object whose state can be modified or changed over time. An immutable object, on the other hand, is an object whose state cannot be modified after it is created.
* Numbers, for instance, are immutable because you can’t change its value. For example, you can’t literarily change the value of 7 to 8. That doesn’t make sense. Instead, you can change the value stored in the variable x from 7 to 8.
* Arrays and objects are not immutable in JavaScript because they can indeed change their value over time.
* Mutable array methods:
  * opyWithin
  * fill
  * pop
  * push
  * reverse
  * shift
  * sort
  * splice
  * unshift
* Benefits:
  * Predictability: Mutation hides change, which create (unexpected) side effects, which can cause nasty bugs. When you enforce immutability you can keep your application architecture and mental model simple, which makes it easier to reason about your application.
  * Performance: Even though adding values to an immutable Object means that a new instance needs to be created where existing values need to be copied and new values need to be added to the new Object which cost memory, immutable Objects can make use of structural sharing to reduce memory overhead.
  * Mutation Tracking: Besides reduced memory usage, immutability allows you to optimize your application by making use of reference- and value equality. This makes it really easy to see if anything has changed. For example a state change in a react component. You can use shouldComponentUpdate to check if the state is identical by comparing state Objects and prevent unnecessary rendering. You can read more about this here.  

<br />


### Memoization

* Memoization is an optimization technique that speeds up applications by storing the results of expensive function calls and returning the cached result when the same inputs occur again.
* A cache is simply a temporary data store that holds data so that future requests for that data can be served faster.
* An expensive function call is a function call that consumes huge chunks of time and memory during execution due to heavy computation.
* Where should it be used
  * For expensive function calls i.e functions that carry out heavy computations.
  * For functions with a limited and highly recurring input range such that cached values don't just sit there and do nothing.
  * For recursive functions with recurring input values.
  * For pure functions i.e functions that return the same output each time they are called with a particular input.

<br />

## Async Javascript

<br />

### What is a callback?

* A callback is a function that's passed as an argument to another function. The callback will usually be executed after the code has finished.
* You can create callback functions yourself by writing functions that accept a function as an argument. Functions like that are also known as higher-order functions. Note that callbacks aren't by default asynchronous.

<br />

### Promises

<br />

[Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

<br />

* The Promise object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.
* Promises represent a proxy for a value which are getting in some point in the future.
* A promise can have 3 states which are the following:
  * Pending: This is the initial state of the promise, the promise is now waiting for either to be resolved or rejected. For example, when are reaching out to the web with an AJAX request and wrapping the request in a promise. Then the promise will be pending in the time window in which the request is not returned.
  * Fulfilled: When the operation is completed succesfully, the promise is fulfilled. For example, when we are reaching out to be web using AJAX for some JSON data and wrapping it in a promise. When we are succesfully getting data back the promise is said to be fulfilled.
  * Rejected: When the operation has failed, the promise is rejected. For example, when we are reaching out to be web using AJAX for some JSON data and wrapping it in a promise. When we are getting a 404 error the promise has been rejected.
* The finally method receives a callback which is executed on both promise fulfillment and rejection. Here we can write 'cleanup' code which need to be executed always regardless of promise outcome.  

<br />

```javascript
let prom = new Promise((res, rej) => {
  console.log('synchronously executed');
  if (Math.random() > 0.5) {
    res('Success');
  } else {
    rej('Error');
  }
})

prom.then((val) => {
  console.log('asynchronously executed: ' + val);
}).catch((err) => {
  console.log('asynchronously executed: ' + err);
}).finally(() => {
  console.log('promise done executing');
});
```

<br />


### Async/await

<br />

* [Mozilla](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)

<br />

* syntactic sugar on top of promises
* their return values are guaranteed to be converted to promises.
* use `try {} catch (e) {} ` or attach `.catch()` at  the end of a function to handle errors


<br />


### Generators and iterators

```javascript
var foo = function* () {
  console.log('first call to next runs to yield #1');
  var a = yield 'A'; // this yield pushes 'A' and returns 'a'
  console.log('second call to next runs to yield #2');
  var b = yield 'B'; // this yield pushes 'B' and returns 'b'
  console.log('third call to next runs to the end of the generator ');
  // there's no return statement here so we are returning undefined
};

var f = foo();

console.log(f.next('this gets ignored')); // { value: 'A', done: false }
console.log(f.next('a'));                 // { value: 'B', done: false }
console.log(f.next('b'));                 // { value: undefined, done: true }

// any further call just returns whatever the last returned value was
console.log(f.next('this also gets ignored since we are done')); // { value
```


<br />


## Javascript module

<br />

### IIFE

* Anonymous function that is executed right after it's created.
* Variables created via var have a scope limited to a function so this construct (which is a function wrapper around certain code) will make sure that your variable scope doesn't leak out of that function.
* This pattern is often used when trying to avoid polluting the global namespace, because all the variables used inside the IIFE (like in any other normal function) are not visible outside its scope.

<br />

```javascript
(function(){
  // all your code here
  var foo = function() {};
  window.onload = foo;
  // ...
})();
// foo is unreachable here (it’s undefined)
```

<br />


### CommonJS

<br />

* [Medium](https://medium.com/@cgcrutch18/commonjs-what-why-and-how-64ed9f31aa46)

<br />

* What is CommonJS? CommonJS is a module formatting system. It is a standard for structuring and organizing JavaScript code. CJS assists in the server-side development of apps and it’s format has heavily influenced NodeJS’s module management.
* What is a module? A module is just a bit of code encapsulated in a file, and exported to another file. Modules focus on a single part of functionality and remain loosely coupled with other filed in an application. This is because there are no global or shared variables between modules, as they only communicate via the module.exports object. Any code that you want to be accessible in another file can be a module!
* How can I use CommonJS? CommonJS wraps each module in a function called ‘require’, and includes an object called ‘module.exports’, which exports code for availability to be required by other modules.
* Why would I need CommonJS?? CommonJS allows for code encapsulation, as modules with no global variables won’t conflict with each other when your application is run. CommonJS aids in dependancy-injection management. Modules are loaded synchronously, so modules that are dependent on other modules must be read further down in the code. The separation of functionality makes for much easier testing and debugging of code.

<br />


### ES6 Modules

* You can use named imports to selectively load only the pieces you need. That can save memory.

<br />

#### Dynamic module loading

* This new functionality allows you to call import() as a function, passing it the path to the module as a parameter. It returns a Promise, which fulfills with a module object (see Creating a module object) giving you access to that object's exports
* This allows you to dynamically load modules only when they are needed, rather than having to load everything up front

<br />



```javascript
import('./modules/myModule.js')
  .then((module) => {
    // Do something with the module.
  });
```

<br />


### Debugging

<br />

#### Debugging with Chrome DevTools

* [Developer chrome](https://developer.chrome.com/docs/devtools/javascript/)

<br />

* Use brakepoints they are faster than the console.log statements

<br />


## React

<br /> 

### SPA’s

* [Mozilla](https://developer.mozilla.org/en-US/docs/Glossary/SPA)

<br />

* An SPA (Single-page application) is a web app implementation that loads only a single web document, and then updates the body content of that single document via JavaScript APIs such as XMLHttpRequest and Fetch when different content is to be shown.
* This therefore allows users to use websites without loading whole new pages from the server, which can result in performance gains and a more dynamic experience, with some tradeoff disadvantages such as SEO, more effort required to maintain state, implement navigation, and do meaningful performance monitoring.
* A benefit is that you only transfer the minimum amount of data needed to make the update changes after the main page has been loaded. The main page loads all the content (js, css, etc) that the SPA needs to run so the initial page load can take longer, but after that it is very fast.


<br />

### Declarative vs. Imperative

<br />

#### Imperative

* With imperative programming, you tell the compiler what you want to happen, step by step.

<br />

#### Declarative

* With declarative programming, on the other hand, you write code that describes what you want, but not necessarily how to get it (declare your desired results, but not the step-by-step):
* Some well-known examples of declarative domain specific languages (DSLs) include CSS, regular expressions, and a subset of SQL (SELECT queries, for example) Many markup languages such as HTML, MXML, XAML, XSLT


<br />

### File structure patterns

<br />

#### Atomic design





<br />

### Component react patterns

<br />

#### Smart & dumb

* One way we can organise our app is into smart and dumb components (a.k.a. container & presentational components). This seperates complex logic from UI.
* Dumb components are small, take an input, do minimal work and then return some UI output. They are easy to test and reason about.
* Presentational components utilize props, render, and context (stateless API’s) and can be the syntactically-pretty functional, stateless component:
* Presentational components receive data and callbacks from props only, which can be provided by its container or parent component.
* Smart components reside at the top level and do most of the heavy lifting. In PM UI we are somewhat following this pattern and have smart components at the top level that fetch data, parse the JSON response, etc. (e.g. FeedsPage.jsx)
* Containers are your data or logic layer and utilize stateful API’s. Using lifecycle events, you can connect to a state management store such as Redux or Flux, and pass down data and callbacks as props to children components. In the container’s render method is where you compose your UI consisting of presentational children components. In order to have access to all stateful API’s, a container must be a class component as opposed to a functional component.
* Tip: Most of these type of components can currently be found in the 'router' directory in PM UI whilst "dumb" components are mainly found in the 'components' directory

<br />

#### It do go down

* Data flows "down" in React. A component may choose to pass its state down as props to its child components
* Any state is always owned by some specific component, and any data or UI derived from that state can only affect components below them in the tree. Never up. This is called uniderectional data flow
* So how can components affect one another and not just their children? These componenets must share some common ancestorage. The parent component can pass a function as a property into a child component. When the function is called from the child component it can affect the parent component and so also affect a different child.

<br />

#### Conditional rendering


* Render a certain JSX code based on the state.
* Conditional rendering is very useful as it allows you to create distinct components based on your needs and then render only the ones that are required by the application.

<br />

#### Render props

* technique for sharing code between React components using a prop whose value is a function
* Render props is when you have a prop that is a function which returns JSX. This of course should work for function components because aside from life cycle methods there really isnt much that is different than class components.

<br />

#### Controlled components

* The controlled component takes the state through props. It can notify any changes by means of callbacks like onChange.
* Parent components can control it by handling the callback and managing its own state meanwhile, the new values are passed to the controlled component as props.
* `<input type = "text" value = {value} onChange = {handleChange} />`


<br />

### One Way Data Flow

* You typically pass down callbacks from parent components to child components as props. When the state changes in any of the child components, it invokes that callback and passes whatever data is appropriate in each use case. Your "controller-view" (the root component that implements the actual callback) then does whatever business logic you need based on the current state and then updates its state accordingly (causing a re-render of the component tree from that component down).
* In general this concept means that data has one, and only one, way to be transferred to other parts of the application.
  * state is passed to the view and to child components
  * actions are triggered by the view
  * actions can update the state 
  * the state change is passed to the view and to child components

<br />

### React is a UI Library

<br />

## React app setup

<br />

### create-react-app

[docs](https://create-react-app.dev/docs/getting-started)

* Create React App is an officially supported way to create single-page React applications. It offers a modern build setup with no configuration.
* Features:
  * A recommended starting folder structure
  * A solid build setup with webpack and Babel (that you don't have to worry about setting up)
  * Scripts to run our React application
  * Scripts to bundle for production
  * Scrips and build for running tests
  * Ability to extend with Sass, TypeScript, and more

<br />

## JSX

<br />

### Dynamic content

```jsx
function App() {
  const [data, setData] = useState([
    { id: 1 }
  ])
  
  
  return (
    <div className="App">
      {data.map(block => <ComponentFoo  key={block.id} block={block} ></ComponentFoo> )}
    </div>
  )
}
```

<br />

### Rendering cycle

* mounting, updating and unmounting.

<br />

### React DOM

* ReactDOM is the glue between React and the DOM. Often, you will only use it for one single thing: mounting with ReactDOM.render(). Another useful feature of ReactDOM is ReactDOM.findDOMNode() which you can use to gain direct access to a DOM element. (Something you should use sparingly in React apps, but it can be necessary.)

<br />


### JSX Expressions

[Introducing JSX](https://reactjs.org/docs/introducing-jsx.html)

<br />

#### Specifying Attributes with JSX

```jsx
const element = <div tabIndex="0"></div>;
const element = <img src={user.avatarUrl}></img>;
```

<br />

#### JSX is an Expression Too

* After compilation, JSX expressions become regular JavaScript function calls and evaluate to JavaScript objects. This means that you can use JSX inside of if statements and for loops, assign it to variables, accept it as arguments, and return it from functions:

<br />

```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

<br />

### JSX Attributes

[JSX in depth](https://reactjs.org/docs/jsx-in-depth.html)




<br />

#### Containment

<br />

## Excercises

### Photo gallery


Hacer un "pagination component" para desplegar 10 fotos en vez de las 5000 de este url (https://jsonplaceholder.typicode.com/photos)


<br />
