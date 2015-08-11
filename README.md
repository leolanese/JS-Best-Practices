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


