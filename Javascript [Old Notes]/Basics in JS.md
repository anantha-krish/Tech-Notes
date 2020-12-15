Basics in JS

## Introducing Script in HTML

- Inline Way
```html
    <script>
        console.log('Hello World !')
    </script>
```

- Using External File

step 1: Create a new file named  "script.js"
```js
console.log('Hello World !')
```

step 2: Import the file in HTML (Preferably at end )
```html
<script src="script.js"></script>
```


## DataTypes & Variables

JavaScript has dynamic typing.
i.e dataTypes are automatically assigned.

There are basically 5 datatypes:

- Number: Floating point numbers
- String
- Boolean
- Undefined: datatype is not assigned to variable
- Null

```js
//Number
var age=28;
console.log(age);

//String
var firstName ='John';
console.log(firstName);

//Boolean
var fullAge = false;
console.log(fullAge);

//undefined
var teacher;
console.log(teacher);

```

## Type Coercion & Variable Mutation

 - Type Coercion -> The number & boolean is converted to string automatically
 
 - Variable Mutation -> contents/datatyes of variable can be changed anytime

```js
 /* 
Type Coercion & Variable Mutation 
************/

var age=28;
var firstName ='John';

// Type Coercion <- the number & boolean is converted to string
console.log( firstName + ' '+ age);


var job, isMarried;
job='Teacher';
isMarried = false;

console.log(firstName +' is a '+ age +' years old ' + job +
'. Is he Married ?'+isMarried
)

//Variable Mutation <- contents/datatyes can be changed in JS
age ='twenty eight';
job = 'driver';

// alert window Example
alert( firstName +' is a '+ age +' years old ' + job +
'. Is he Married ?'+isMarried )

//prompt window example
var lastName = prompt('What is his Last Name ?');
console.log( firstName +' '+ lastName);

//typeof operator

console.log(typeof age)  //number
console.log(typeof isMarried)  //boolean
```

## Truthy, Falsy values and equality operators

 * falsy values : undefined , null , 0, '' , NaN
 * truthy values : NOT falsy values
 *  '==' does type coercion on operands
 *  '===' is better to use in javascript as its more strict

```js

/******
 * Truthy,Falsy values and equality operators
 * 
 * falsy values : undefined , null, 0, '',NaN
 * truthy values : NOT falsy values
 */

var height =23;
if( height || height === 0 )
console.log('Variable is defined')

else
console.log ('variable is not defined')

//equality operator
if( height =='23')
console.log('The == operator does type coercion, operands can be different type')

if( height === 23)
console.log('The === operator is stricter operands should have same type')

```

## Function Statements & Expressions

- Statements are those which doesnot return any immediate value
 Ex: If else, Switch , variable declaration etc.
 
- Expression are those which return an immediate value.
Ex: call(), x > 2 , x === y , typeof 2 etc..


```js
// function statement
function sum (a,b)
{
  return a + b;
}

// function expression 
var sum =  function(a,b)
{
return a + b ;
}
```
## Arrays
- index starts with 0
-  array.length -> will give size of array
-  array.push(element) ->  inserts element at the end of array
-  array.unshift(element) -> inserts element at the starting of array
-  array.pop()-> deletion from the end
-  array.shift()-> deletion from the start
-  array.indexOf(element)-> will return the index if found else will return -1 (not found);

```js
// Initialize new array
var names = ['John', 'Mark', 'Jane'];
var years = new Array(1990, 1969, 1948);

console.log(names[2]);
console.log(names.length);

// Mutate array data
names[1] = 'Ben';
names[names.length] = 'Mary';
console.log(names);

// Different data types
var john = ['John', 'Smith', 1990, 'designer', false];

john.push('blue');
john.unshift('Mr.');
console.log(john);

john.pop();
john.pop();
john.shift();
console.log(john);

console.log(john.indexOf(23));

var isDesigner = john.indexOf('designer') === -1 ? 'John is NOT a designer' : 'John IS a designer';
console.log(isDesigner);
```

## Objects & properties

in objects we do key-value pairing. To access a value, you have two ways:

- Dot operator : object.firstName
- Indexing : object['firstName']    <- Important key should be passed as string.

```js
// Object literal
var john = {
    firstName: 'John',
    lastName: 'Smith',
    birthYear: 1990,
    family: ['Jane', 'Mark', 'Bob', 'Emily'],
    job: 'teacher',
    isMarried: false
};

console.log(john.firstName);
console.log(john['lastName']);
var x = 'birthYear';
console.log(john[x]);

john.job = 'designer';
john['isMarried'] = true;
console.log(john);

// new Object syntax
var jane = new Object();
jane.firstName = 'Jane';
jane.birthYear = 1969;
jane['lastName'] = 'Smith';
console.log(jane);
```
## Objects & Methods


A function is a piece of code that is called by name. It can be passed data to operate on (i.e. the parameters) and can optionally return data (the return value). All data that is passed to a function is explicitly passed.

A method is a piece of code that is called by a name that is associated with an object. i.e implicitily passed with object & scope is limited to data inside the class.

```js
var john = {
    firstName: 'John',
    lastName: 'Smith',
    birthYear: 1992,
    family: ['Jane', 'Mark', 'Bob', 'Emily'],
    job: 'teacher',
    isMarried: false,
    //function expression
    calcAge: function() {
        this.age = 2018 - this.birthYear;
    }
};

john.calcAge();
console.log(john);

```
## Looping & iteration

```js
var john = ['John', 'Smith', 1990, 'designer', false, 'blue'];

for (var i = 0; i < john.length; i++) {
    console.log(john[i]);
}

// Looping backwards
for (var i = john.length - 1; i >= 0; i--) {
    console.log(john[i]);
}

```


## How JS works behind the scenes

objects defined directly are stored in global context or also known as window objects

functions executions are provided different execution contexts.


### Hoisting
- functions are already available after hoisting
- variables or function expressions are undefined after hoist

 Example 1 : functions -> statement vs expressions
```js
console.log(age(1995)) *//<- works

// functions are already available after hoisting
 function age(year)
{
    return 2020 - year
}

console.log(retirement (2020)) // error

// variables or function expressions are undefined after hoist
// only available during execution time
var retirement = function (year)
{
    return 65 - (2020 -year)
}
console.log(retirement (1990)) // <-works
```


- Example 2: Variables
```js
console.log(age) //undefined
var age = 23
function test ()
{  //Note: the varaible name of global & local is same 
    console.log(age) // in current scope its undefined
    var age = 34
    console.log(age) // 34
}
test()
console.log(age) //24
```

### Scoping
Inner functions can access the variables in outer functions
```js
 var a = 'Hello!';
first();
function first() {
    var b = 'Hi!';
    second();

    function second() {
        var c = 'Hey!';
        console.log(a + b + c); // can access b & c
    }
} 
```

### Difference bw execution stack & scope chain
``Scope chain ->`` Order in which functions are written lexically

``Execution stack->`` Order in which function is called
```js
var a = 'Hello!';
first();

function first() {
    var b = 'Hi!';
    second();

    function second() {
        var c = 'Hey!';
        third()
    }
}

function third() {
    var d = 'John';
    console.log(a +  d); // b & c cannot be accessed.
}
```


This pointer pending...


type conversion

parseInt ,parseFloat