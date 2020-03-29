# Javascript

## Table of Contents

- [Notes](#notes)
- [Utilities](#utilities)
- [Primitive Datatypes](#primitive-datatypes)
- [Variable Declaration](#variable-declaration)
- [Objects](#objects)
- [Arrays](#arrays)
- [Functions](#functions)
- [Constructors](#constructors)
- [Type Coercion](#type-coercion)
- [Scopes and Closures](#scopes-and-closures)
- [References](#references)

## Notes

* Root object holds all the global variables. Root object depends on the **context**. 
For **browser** context, root object is __window__ object. For **node** context, root object is __node__ object.
* There is a root global object **window** every time we execute javascript in browser. All the properties associated with windows objects
are also global.
* Whenever a **global variable** or function is created, a __new property__ on window object is created.
* Global variables can be accessed via __variable_name__ or __window.variable_name__
* Execution of javascript takes place in 2 steps
  1. **Compilation** Look for all the var declaration and create a scope
  2. **Interpretation** Use the scopes created in step 1 and start assigning values & executing
* Javascript is **single threaded** application. It is **event driven** and uses **callbacks**.
* Global Math Object https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math
* Global Date object https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date

## Utilities

```javascript
// Print output
console.log("The value is " + val);

// Check the type of variable
console.log(typeof val);

// Find length of string/array
myvar.length

// Wait for 1 sec and execute a function using callback
setTimeout(fn, 1000);
var fn = function(){
  console.log("Hello World !!");
};

```

## Primitive Datatypes
* number (no separate float datatype)
* string
* boolean
* undefined
* null

## Variable Declaration

```javascript
// Declare number 
// Numbers are in "double-precision 64-bit" format
var value = 10;

// String
var value = "Hello World!!";

// Boolean
var value = true;

// undefined
// This type contains only 1 value which is 'undefined'
var value;

// null
// This type contains only 1 value which is 'null'
var value = null;
```

## Objects

```javascript

// Declare an object
var myObj1 = {};
var myObj2 = new Object();
var myObj3 = {
  "prop1": "Hello",
  "prop2": true,
  "prop3": 123
};

// Declare nested objects
var myObj4 = {
  "prop1": "Hello",
  "prop2": true,
  "prop2": 123,
  "prop3": {
    "innerProp": "Inner prop value"
  }
};

// Add properties to the object
myObj.prop1 = "Hello";

// Delete the object property
delete myObj.prop1;

// Read value from the property
var value = myObj.prop1;
var value = myObj["prop1"];
```

## Arrays
Arrays are also objects
```javascript
// Declare array
var myArray = ["Hello", "World"];

// Access array
var value = myArray[3];
var value = myArray["3"];

// Assign value to the array
myArray[3] = "some value"

// Check length of an array. length is the property of all the array
myArray.length

// Methods properties on array object
myArray.push(10);     // Add element at the end of the array
myArray.pop();        // Fetch & Remove element from the end of the array
myArray.shift();      // Fetch & Remove element from the start of the array
myArray.unshift("A"); // Add element at the start of the array
```

## Functions
* Functions are also objects
* No function overloading is possible
* While calling the function, we can pass any number of arguments
* While calling the function,  **arguments** (an object) and **this** are implicitly passed.
* **this** value will depend on the context in which function is called
* By default, every function returns __undefined__

```javascript

// Function Declaration
function myFunc(name){
  console.log('Hello ' + name);
}

// Function Expression
var myFunc = function foo(name){
  return 'Hello ' + name;
}

myFunc()

// Anonymous Function Expression
var myFunc = function(name){
  return 'Hello ' + name;
}

myFunc();

// Immediately Invoke Function Expression (IIFE)
(function(name){
  return 'Hello ' + name;
})();

// Using functions on objects
var person = {
  "firstName": "Hello",
  "lastName": "World",
  "getFullName": function(){
    return this.firstName + " " + this.lastName;
  }
};

// this depends on the context in which it is called. There are various ways to call the function
// Method 1: Global Context
myFunc()  // myFunc will receive this pointing to 'global' context

// Method 2: Object Context
myObj.myFunc()  // myFunc will receive this pointer pointing to the 'object' context

// Method 3: Calling Constructor Function
new Employee();   // new 'this' object will be created and returned.

// Method 4: Object context but passing this of different object
myObj.myFunc.call(myObj2) // myFunc will receive 'this' pointing to 'myObj2' context
```

## Constructors
* Constructor in javascript is just a __function__.
* As per naming convention, function constructors created using new should should start with CAPITAL letter
* Constructors can be created in 2 ways
  1. Manually create constructor object
  2. Create constructor using **new** keyword

```javascript

// Method 1: Manual constructor
function createEmployee(firstName, lastName, gender, designation) {
  // Create a empty object
  var newobj = {};
  
  newObj.firstName = firstName;
  newObj.lastName = lastName;
  newObj.gender = gender;
  newObj.designation = designation;
  
  // return object
  return newObj
}

var emp = createEmployee("Hello", "World", "F", "Staff");

// Method 2: Using new
function Employee(firstName, lastName, gender, designation) {
  // Implicit object creation on using 'new'
  // var this = {};
  
  this.firstName = firstName;
  this.lastName = lastName;
  this.gender = gender;
  this.designation = designation;
  
  // Implicit return object on using 'new'
  // return this;
}

var emp = new Employee("Hello", "World", "F", "Staff");
```

## Type Coercion
Automatic type conversion by javascript interpreter

Use **===** operator for precise check i.e for checking both value and type

```javascript
// number and string will become 'string'
var a = 123 + "4"; //Result: 1234

// string and bool will become 'bool'
// Non empty string will return true
var a = "hello";
if (a){

}

// number and bool will become bool
// Non 0 value returns true
var a = 1234;
if (a){

}
```

## Scopes and Closures
* Variable scopes are defined by using **function**
* Variables outside functions are the part of global scope and can be accessed by anyone in the particular context
* Reading undeclared variable results in error
* Writing undeclared variable results in creating a **new variable** in global scope. __use strict;__ prevents this behavior
* **Closuer** When function is passed as the argument, all the reference to the variables used inside the function are also passed.
Scope information of variables inside the functions are also passed.

```javascript
// Module pattern
// Create 'private' variables in javascript using scopes and closures
function createPerson() {
  
  // Define private variables here
  var firstName = "Hello";
  var lastName = "World";
  
  var returnObj = {
    // Define getters and setters here
    "getFirstName": function() {
      return firstName;
    },
    ...
  };
}

var person = createPerson();
console.log(person.getFirstName());
```

## References

https://javabrains.io/courses/corejs_jsfordev/

https://javabrains.io/courses/corejs_scopesclosures/

https://javabrains.io/courses/corejs_objectsprototypes/

https://www.pentesteracademy.com/course?id=11
