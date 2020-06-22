# Creating and using constructors
`Blue printer === constructor`

Constructors are like regular functions, but we use them with the `new` keyword. There are two types of constructors: built-in constructors such as `Array` and `Object`, which are available automatically in the execution environment at runtime; and custom constructors, which define properties and methods for your own type of object.
- `new` method
- `Object.defineProperty()` method
- Object literal notations

```js
function Book(name,year) {
  this.name=name
  this.year=year
  this.print=function(){
    console.log('The '+this.name+" book was publish in "+this.year)
  }
  return 1
} 
newBook=new Book('Master Java',1971)
newBook.print()

// object literals vs constructors

var obj = new String("text");
obj.slice(0,2);     // "te"

// same as above
var string = "text";
string.slice(0,2);  // "te"


```
Here Book is a constructor(aka function). 
# Determining the type of an instance
- instanceof
- [instance].constructor

<span style="color: red;">**Note that if the right side of the instanceof operator isn’t a function, it will throw an error**.</span> Every object in JavaScript inherits a constructor property from its prototype, which points to the constructor function that has created the object. Note, however, that using the constructor property to check the type of an instance is generally considered bad practice because it can be overwritten.

# Scope-safe constructors
```js
function Book(name, year) { 
  if (!(this instanceof Book)) { 
    return new Book(name, year);
  }
  this.name = name;
  this.year = year;
}

var person1 = new Book("js book", 2014);
var person2 = Book("js book", 2014);

console.log(person1 instanceof Book);    // true
console.log(person2 instanceof Book);    // true
```



```js
//prototypal inheritance
var human = {
 species: "human",
 create: function(values) {
  var instance = Object.create(this);
  Object.keys(values).forEach(function(key) {
    instance[key] = values[key];
  });
  return instance;
 },
 saySpecies: function() {
  console.log(this.species);
 },
 sayName: function() {
  console.log(this.name);
 }
};

var musician = human.create({
  species: "musician",
  playInstrument: function() {
    console.log("plays " + this.instrument);
  }
});

var will = musician.create({
  name: "Will",
  instrument: "drums"
});

will.playInstrument(); // plays drums
will.sayName(); //will
```

## Primitive Data Type

| Tables    |                  Are                  |
| --------- | :-----------------------------------: |
| boolean   |             true or false             |
| null      |               no value                |
| undefined | a variable declared, but has no value |
| number    |    integers, decimals, floats, etc    |
| string    |     a series(array) of characters     |

## Object Data Types

| Tables       |                   Are                   |
| ------------ | :-------------------------------------: |
| new Array    |         A collection of values          |
| new Error    |  Contains a name and an error message   |
| new Function |             A block of code             |
| new Object   |        A Wrapper around any type        |
| new RegExp   |          A regular expression           |
| new Boolean  |  An object that contains true or false  |
| new Number   | An object that contains a numeric value |
| new String   | An object that contains a character(s)  |

## Object Data Type / Constructor

- All object data types inherit from Object (not primitives)
- Object has constructor property
- Returns a reference to the object itself

```js
function constructorSample() {
  let introDate = new Date();
  let strValue = new String();
  let isActive = false;

  console.log("introDate = " + introDate.constructor.toString());
  console.log("strValue = " + strValue.constructor.toString());
  console.log("isActive = " + isActive.constructor.toString());
  console.log(
    "constructorSample = " + constructorSample.constructor.toString()
  );
}
```



# difference between call, apply and bind
## Object: 
```js
var obj = {num:2};
```
## function or method:
```js
var addToThis = function(a) {
	return this.num + a;
};
```
suppose we want to add this functionality to our object(obj) then we can use `call`,`apply` or `bind`:
```js
console.log(addToThis.call(obj, 3));
```
here we follow `functionName.call(obj,functionarguments)` which executed and give the result.
```js
var arr=[3,5];
var addToThis2 = function(a,b) {
	return this.num + a + b;
};
console.log(addToThis2.apply(obj, arr)); 
```
here we follow `functionName.call(obj,array)` which executed and give the result.
```js
var bound = addToThis.bind(obj);
console.log(bound(3))
```
here we follow `functionName.bind(obj)` which return a function. **A[pply] for array and C[all] for comma**

# Memoization 
caching data for re-using which optimizated a lot. 
```js
const prevValues = []

function square(n) {
	if(prevValues[n] != null) {
  	return prevValues[n]
  }
  let result = 0
  for (let i=1; i<=n; i++) {
  	for(let j=1; j<=n; j++) {
    	result += 1
    }
  }
  prevValues[n] = result
  return result
}
console.log(square(30000))
```
In dynamic programming:
```js
function fib(n,prevValues = []) {
  if(prevValues[n] != null) {
  	return prevValues[n]
  }
  let result
  if(n <= 2) {
  	result = 1
  } else {
  	result = fib(n -1, prevValues) + fib(n -2, prevValues)
  }
  prevValues[n] = result
  return result
}
console.log(fib(40))
```
