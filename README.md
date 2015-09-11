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
```

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
```

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

--- 
### Be careful using indexOf: (IE9+, Mobile=?)
```javascript
['apple','lemon','banana'].indexOf('lemon'); //returns 1
['apple','lemon'].indexOf('banana'); //returns -1
```

---
### "Don't modify objects I don't own"
Be careful extending Native Object like Object.prototype


### Why: Simply put: if everyone on your team modified objects that they didn’t own, you’d quickly run into naming collisions, incompatible implementations, and maintenance nightmares.

If for some reason you do end up extending the object prototype, ensure that the method doesn't already exist, document it 
so that the rest of the team is aware why it's necessary. 

```javascript
	if(typeof Object.prototype.myMethod != 'function'){
	    Object.prototype.myMethod = function(){
	         ...
	    };
	}
```

---

### Return shorthand
```javascript
if(a == b){
 return true;
} else {
 return false;
}
```

```javascript
// same as
return (a == b);
```

---
### DRY (Don't Repeat Yourself) 
If you are repeating yourself, you're doing it wrong (typical case is when you are C&Pasting and change just the values). 
Avoid repetition specially where JS is slow.
```javascript
if (1 === 'activate'){
  sound.play();
}
if (2 === 'activate'){
  sound.play();
}
if (3 === 'activate'){
  sound.play();
}

// put all the properties within array
var obj = ['active','active','active']

// or create an Object Literal
var obj = {
  "1": "active",
  "2": "active",
  "3": "active"
};

$.each( obj, function( key, value ) {
  console.log( key + ": " + value );
});
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
```
---
### Avoid use eval() or with()
Use them Only for deserialisation (e.g. evaluating RPC responses)

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
### Avoid Global variables
Global are slow don’t go window

---
Avoid != null comparison
```javascript
//AVOID
var n !== nul

//USE
use instanceof instead
```
---
### Beware Loose Type
When doing mathematical operations, JavaScript can convert numbers to stings:
```javascript
var x = 5 + 7;       // x.valueOf() is 12,  typeof x is a number
var x = 5 + "7";     // x.valueOf() is 57,  typeof x is a string
```
---
### Use the array.length out of the array iteration
```javascript
Cache length during loops.
Caching the length can be anywhere up to 190 times faster than repeatedly accessing it.

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

// TEST: http://jsperf.com/for-count-inside-and-outside
```
---
### "Don't modify objects I don't own"
Be careful extending Native Objects, like Object.prototype
My point is that you should treat the already-existing JavaScript objects as a library of utilities. 
Don't override methods, don't add new methods, don't remove existing methods.
When you are the only one working on a project, it is easy to get away with these types of modifications because you 
Know them and expect them. When working with a team on a large project, making changes like this cause mass confusion and a lot of lost time.

This is quite different from extending your own custom object properties. 
It's generally considered bad practice, and modifying it should only be a last resort.
It looks like properly extending native objects, unlike host ones, is actually not all that bad. 
This is of course considering that we are talking about standard objects and methods. 
Extending native built-ins with custom methods immediately makes "collision" problem apparent. It violates "don't 
modify objects you don't own" principle, and makes code not future-proof.
Simply put: if everyone on your team modified objects that they didn’t own, you’d quickly run into naming collisions, incompatible implementations, and maintenance nightmares.

If for some reason you do end up extending the object prototype, ensure that the method doesn't already exist, document it 
so that the rest of the team is aware why it's necessary. 

```javascript
if(typeof Object.prototype.myMethod != 'function'){
    Object.prototype.myMethod = function(){
         ...
    };
}
```
---
### Extra consideration
- Use a JavaScript quality Validator like www.jslint.com (Douglas Crockford's JavaScript Lint) or JSHint
- Use Profiling Tools
- Bundle the js: Pack all JS and CSS
- Remove comments
- Remove duplication
- Minify JS and CSS
- Obfuscate: Technique to reduce the name of the all variables in the code: JSON, JS, css, etc
- GZIP compression: Enable file compression using mod_gzip Apache on server side
- deflate compression': Enable file compression using mod_deflate Apache on server side
- server caching: avoiding to generate this big process on every request on server
- Client-side caching 

