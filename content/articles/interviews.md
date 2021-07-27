---
title: Web dev resources
---

## General

<br />

## Javascript

1. Explain the meaning of this
    1.  It has different values depending on where it is used:.
        In a method, this refers to the owner object. 
        Alone, this refers to the global object. 
        In a function, this refers to the global object. 
        In a function, in strict mode, this is undefined. 
        In an event, this refers to the element that received the event. 
        Methods like call(), and apply() can refer this to any object.
        
1. What is falsy and truthy and give some examples
    1. In JavaScript, a truthy value is a value that is considered true when encountered in a Boolean context.
    1. A falsy (sometimes written falsey) value is a value that is considered false when encountered in a Boolean context. 
    1. false, null, undefined, 0, NaN, '', "", ``(Empty template string), document.all , 0n: BigInt
       , -0
    1. Type coercion is the process of converting value from one type to another (such as string to number, object to boolean, and so on). Any type, be it primitive or an object, is a valid subject for type coercion. To recall, primitives are: number, string, boolean, null, undefined + Symbol (added in ES6).
  
1. What is hoisting
    1. Hoisting is JS’s default behavior of defining all the declarations at the top of the scope before code execution.
    1. One of the benefits of hoisting is that it enables us to call functions before they appear in the code. JavaScript only hoists declarations, not initializations. 

1. What is a closure
    1. When you declare a local variable, that variable has a scope. Generally, local variables exist only within the block or function in which you declare them.
    1. A closure is a persistent scope which holds on to local variables even after the code execution has moved out of that block.
  
1. Mention different data types and how are they categorized?
    1. String, Number, Boolean, Undefined, Null, Object, Array, RegExp.
    1. They are categorized in primitive and non-primitive (referenced)
    1. If the value is a primitive value, when you access the variable, you manipulate the actual value stored in that variable. In other words, the variable that stores a primitive value is accessed by value. The size of a primitive value is fixed, therefore, JavaScript stores the primitive value on the stack. 
    1. Unlike a primitive value, when you manipulate an object, you work on the reference of that object, rather than the actual object. It means a variable that stores an object is accessed by reference. When you assign a value to a variable, the JavaScript engine will determine whether the value is a primitive or reference value.
