# JS-Best-Practices
A place to debate about the best way to code good JS

## Table of contents

## arrays
- Array creation using literal syntax

```javascript
// longhand
var a1 = New Array();

// shorthand
var a2 = []; //array literal
```

## objects
- Object creation using literal syntax

```javascript
// longhand
var o1 = new Object();

// shorthand
var o2 = {}; //empty object literal
```

## strings
- Using single quote '' for strings
```javascript
// longhand
var s1 = "Hello world";
 
// shorthand
var s2 = 'Hello world';
```

## Variable Creation
- Variable creation, Scope and Lookups
```javascript
// longhand
foo = 1; //Stores 1 as global variable (windows is the father of all scopes)

// shorthand
var foo = 1; //assigns number 1 to the variable foo global to our scope, avoid scope lookup
```

## Comparison operators
- Always use === instead of == and !== instead of !=
```javascript
if (values != null){           // AVOID
     …
}

if (values instanceof Array){  // Preferred instanceof ‘operator’
     …
}

or make it complete:
if (values){                  // even better: !== 0, !== null and !== undefined
     …
} 
```

## loops
- Evaluate the length of the conditional
```javascript
for (initialisation; conditional; incremental) {
 ...
}
```

+ Use the array.length out of the array iteration:
Cache length during loops.
Caching the length can be anywhere up to 190 times faster than repeatedly accessing it.

```javascript
//count outside
var count=a.length;
for(var i=0; i<count; i++){
    //do nothing as we are measuring time based on count var only 
}

//calculating count on each iteration
for(var i=0; i< a.length; i++){
    //do nothing as we are measuring time based on count var only 
}

//count inside: fastest
for(var i=0,count=a.length; i<count; i++){
    //do nothing as we are measuring time based on count var only 
}
```
[TEST](http://jsperf.com/for-count-inside-and-outside)


### functions
```javascript
foo = function(arg1, arg2) {};     // not good
function foo2(arg1, arg2) {};       // OK...
var foo3 = function(arg1, arg2) {}; // recommend
```
### Why
Is syntactically consistent with the rest of your variables, and doesn't throw the function into global scope.

### prototypes: Prototypal inheritance simples concept
```javascript
function Person(name, age) {
  name = name;
  age  = age;
}

Person.prototype.color = 'White';
```
### Why: 
In JS everything is an Object (...) function, array, objects are considered as objects. 
All objects in JS inherit, properties and methods from the prototype. So objects just "point" to their parent via prototypes.
Prototype style is much faster to create (each closure creates a function object). 
We simple make an object and then create new objects from them pointing at other objects as the prototype. If you want to define a new object you create a new object, that become the bases for all others and then we simple overwrite method and properties on those created objects.

Creating object on JS is just sintaxis sugar. There is only one way of inheritance: prototype. If we want to define a new object, we create a new object base. 

The others function Constructor or Object.create() in the end they are doing the same and it all about prototype

### prototypes: Properties INSIDE object Constructor. Method OUTSIDE, on the prototype.
```javascript
function Person(name, age) {
  name = name;
  age  = age;
}

Person.prototype.color = 'White';
```
### Why 
Because only need one copy to be use and we save memory space.

Constructor.prototype:
We can add features all at ones to all of those object created by the object Constructor (new).

Objects con be set inside object constructor, but this function takes memory space. If I added to the function constructor, every object will have it, if I have 10000 object every object will have their own copy, but If I added to the prototype of the function constructor (.prototype) it will only be ones because the constructor will prototype chain it, so performance PoV: save memory space and put your methods on the prototype and use prototype chain to find it

So then the unique Methods still can be access because of the prototype chain and it is using only one space in memory, but properties are going to be different for every new object. 


---
### Clean the garbage:

We need to destroy every object we are not using any more. 
In order to our memory usage, we set to null, so the garbage collector will destroy will destroy it:

```javascript
// do some thing with the object
var oObje = new Object;

// set to null = there are no longer any references to the object.
oObj = null;
```

---
### Avoid Null Comparisons(value != null)

Values should be checked for what they are “expected to be”, not for what they “aren't expected to be”: is expected to be an array, so you should be checking to see if it is an array.

```javascript
if (values != null){             // AVOID
     …
}

if (values instanceof Array){  // Preferred instanceof ‘operator’
     …
}

or make it complete:
if (values){             // even better: !== 0, !== null and !== undefined
     …
}
```javascript

---
### new() vs Object Literals:

```javascript
var lunch = new Array();
lunch[0]='Dosa';
lunch[1]='Roti';
lunch[2]='Rice';
lunch[3]='what the heck is this?';

= 

var lunch = [
   'Dosa',
   'Roti',
   'Rice',
   'what the heck is this?'
];
```javascript

-=-
### Use function factories

```javascript
function hello(lang) {
  
  return function(firstname,lastname){
    if(lang === 'en'){
      console.log('Hello ' + firstname + lastname);
    }
    if(lang === 'es'){
      console.log('Hola ' + firstname + lastname);
    }
  }
  
}

var en = hello('en');
var es = hello('es');

en('Leo','Lanese'); // Hello LeoLanese
es('Leo','Lanese'); // Hola LeoLanese
```

---
### Obscure js

Accessor setters and getters: 
Obscure the function and provide encapsulation

```javascript
function Field(val){
    this.value = val;
}
Field.prototype = {
    getValue: function(){
        return this._value;
    },
    setValue: function(val){
        this._value = val;
    }
};

var field = new Field("test");

field.setValue("test2")
field.getValue()
```

---
### Avoid scope-lookUps 
Specially if some slow JS process is involved, loops, intervals. 

```javascript
var foo = 42;
(function() {
  (function() {
    alert(foo); // two scope lookups
  }());
}());
```


---
### Use event delegation
Something needs to happen when each child element is clicked.
You could add a separate event listener to each individual LI element, but what if 
'LI' elements are frequently added and removed from the list?
The better solution is to add an event listener to the UL parent element.  

```javascript
<ul id="parent-list">
   <li id="post-1">Item 1</li>
   <li id="post-2">Item 2</li>
   <li id="post-3">Item 3</li>
   <li id="post-4">Item 4</li>
   <li id="post-5">Item 5</li>
   <li id="post-6">Item 6</li>
</ul>
```

But if you add the event listener to the parent, how will you know which element was clicked?
Simple:  when the event bubbles up to the UL element, you check the event object's 
target property to gain a reference to the actual clicked node:

```javascript
// using $
$('parent-list').on('click',function(){
  if(e.target && e.target.nodeName == "LI") {
  // List item found!  Output the ID!
     console.log("List item ",e.target.id.replace("post-")," was clicked!");
  }	
})

// using JS Get the parent DIV, add click listener...
document.getElementById("parent-list").addEventListener("click",function(e) {
    // e.target was the clicked element
    console.log( e.target)
});
```


---
### Be careful using: parseInt()

```javascript
  parseInt('10'); //10
  parseInt('010'); //8

  We need to specified the base to use:
  parseInt('010',10); //10

  Or we can use: 
  Number('010'); //10
```

---
### Beware Loose Types
When doing mathematical operations, JavaScript can convert numbers to stings:
```javascript
var x = 5 + 7;       // x.valueOf() is 12,  typeof x is a number
var x = 5 + "7";     // x.valueOf() is 57,  typeof x is a string
```

---
### Use coercion to verify lack of existence

```javascript
if (a || a !== 0) { // + !==0
   console.log("something is there"); // a is something: !== undefined, !== null and !==""
}
```

---
### Use the array.length out of the array iteration
Cache length during loops.
Caching the length can be anywhere up to 190 times faster than repeatedly accessing it.

```javascript
//count outside
var count=a.length;
for(var i=0; i<count; i++){
    //do nothing as we are measuring time based on count var only 
}
```

```javascript
//calculating count on each iteration
for(var i=0; i< a.length; i++){
    //do nothing as we are measuring time based on count var only 
}
```

```javascript
//count inside: fastest
for(var i=0,count=a.length; i<count; i++){
    //do nothing as we are measuring time based on count var only 
}
```







