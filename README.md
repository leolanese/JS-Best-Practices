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

## Variable creation
- Variable creation, Scope and Lookups
```javascript
// longhand
foo = 1; //Stores 1 as global variable (windows is the father of all scopes)

// shorthand
var foo = 1; //assigns number 1 to the variable foo global to our scope, avoid scope lookup
```

## Comparison operators
- Always use === instead of == and !== instead of !=

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


### prototypes
In JS everything is an Object (...) function, array, objects are considered as objects. All objects in JS inherit, properties and methods from the prototype. Prototype style is much faster to create (each closure creates a function object)
```javascript
function person(name, age) {
  name = name;
  age  = age;
}

person.prototype.color = 'White';
```
