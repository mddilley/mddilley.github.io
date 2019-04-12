---
layout: post
title:      "Hoisting and Scope in JavaScript ES6"
date:       2019-04-09 15:19:25 -0400
permalink:  hoisting_and_scope_in_javascript_es6
---


In this post, I will explore two concepts in JavaScript: scope and hoisting. This will include how ES6 variable declaration keywords (`var, let, and const`) affect scope and hoisting.
## Scope
Scope determines the accessibility of variables, functions, and objects in a particular part of code during runtime. In JavaScript, functions have **lexical scope** which means that they are linked to the object that they are defined within. The following examples will explore global and local scope and scope in block statements.

**Global Scope**

A variable defined outside of a function is in global scope and is accessible and can be altered in any other scope.
```
let name = “Name”;

console.log(name); // logs “Name”

function logName() {
	console.log(name);
}

logName(); // logs “Name” since the global variable, name, is accessible within the function logName
```

In addition, variables declared and assigned without the use of `let, const, or var` keywords are global variables - no matter where they are declared and assigned.

**Local Scope**

A variable defined inside a function is considered to be in local scope and is not accessible in other functions.
```
function logName() {
	let name = “Name”;
	console.log(name);
}
logName(); // logs “Name”
console.log(name); // returns a ReferenceError since the declared and assigned variable, name, is only accessible within the local scope of the function, logName
```

Another way to demonstrate global scope and local scope is with the following function declarations.
```
// Global Scope
function firstFunction(){
        // Local Scope inside firstFunction
		    function secondFunction(){
		    // Local Scope inside secondFunction
		    }
	}
// Global Scope
```
**Block Statements**

Block statements, like `if`, `switch`, `for` loops, or `while` loops, do not create a new scope. Upon the introduction of ES6 keywords `let` and `const`, some differences emerged concerning the declaration of local scope inside block statements. Both `let` and `const` support declaration of local scope inside block statements while keyword `var` does not.

```
if(true) {
	var dog = ‘Ben’; // dog is in global scope
	let cat = ‘Meow’; // cat is in local scope
	const person = ‘Mike’; // person is in local scope
}
console.log(dog); // logs ‘Ben’
console.log(cat); // Uncaught ReferenceError: cat is not defined
console.log(person); // Uncaught ReferenceError: person is not defined
```
## Hoisting
To begin, hoisting is a mechanism in JavaScript that moves variable and function declarations to the top of their scope. It is important to note that only the declaration is moved and not the assignment which stays in place.

**Hoisting Variables**

```
console.log(hoisted); // logs undefined since the declaration is hoisted but not the assignment
var hoisted = ‘This is a variable’;
```
To demonstrate hoisting within a function, consider the following.
```
function hoisted() {
	console.log(dog);
	var dog = ‘Ben’;
}
hoisted(); // logs undefined
```
Due to hoisting, the following example is equivalent to the previous.
```
function hoisted(){
	var dog;
	console.log(dog);
	dog = ‘Ben’;
}
```
The ES6 variable declaration keywords `let` and `const` behave a bit differently. They do not support hoisting of variable declarations.
```
console.log(hoist); // outputs ReferenceError since the declaration is not hoisted
let hoist = ‘Hoist’;

console.log(hoist); // outputs ReferenceError since the declaration is not hoisted
const hoisted = ‘Hoisted’;
```
**Hoisting Functions**

When discussing hoisting and functions, it is important to differentiate between function declarations (which are hoisted) and function expressions (which are not hoisted). Let’s take a look at an example of a hoisted function declaration.
```
hoisted(); // logs “I’ve been hoisted!”
function hoisted() {
	console.log(“I’ve been hoisted!”);
}
```
Now, let’s compare that to a function expression.
```
hoist(); // Outputs TypeError: hoist is not a function
let hoist = function(){
	console.log(“Not so lucky”);
}
```
These examples will help you navigate scope and hoisting in JavaScript ES6. Understanding these topics will help you make sense of the behavior of variable declarations and assignments and also function declarations and expressions and calling those functions.

