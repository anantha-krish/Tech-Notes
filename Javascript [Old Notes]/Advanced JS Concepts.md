Advanced JS Concepts


In JS everything is object, except the primitive data types

## Inheritance

Prototype & prototype chain

the js first search  a method in its own object, otherwise it will search on prototype.


Ever js object has prototype.

Object can define some properties & methods in prototype, so that other objects can also inherit them.

```js
//Function constructor
var Person = function(name,lastName,year){
  this.name = name
  this.lastName = lastName
  this.year = year
}

//Prototype
Person.prototype.calculateAge = function()
{
    console.log( 2020 - this.year)
}

var john = new Person('John','Smith',1988)
var mark = new Person('Mark','Smith',1995)
john.calculateAge()
mark.calculateAge()
```

__proto__ in instance

```js
Person {name: "John", job: "teacher", year: 1988}
job: "teacher"
name: "John"
year: 1988
__proto__:
calculateAge: ƒ ()
lastName: "Smith"
constructor: ƒ (name,year,job)
__proto__: Object   \\ prototype chain

```
Example:
Array is also an object
```js
var x = [10,20,30]
console.info(x)

0: 10
1: 20
2: 30
length: 3
__proto__: Array(0)
concat: ƒ concat()
constructor: ƒ Array()
copyWithin: ƒ copyWithin()
find: ƒ find()
forEach: ƒ forEach()
includes: ƒ includes()
indexOf: ƒ indexOf()
join: ƒ join()
keys: ƒ keys()
lastIndexOf: ƒ lastIndexOf()
length: 0
map: ƒ map()
pop: ƒ pop()
push: ƒ push()

```

## Object Creation (Another method)

 Object.create() is used when you have to implement complex prototypes
 
```js
var personProto = {
  calculateAge: function () {
    console.log(2020 - this.year);
  }
};

var john = Object.create(personProto);
john.name ='John';
john.year = 1995;
john.calculateAge();

//output
{name: "John", year: 1995}

name: "John"
year: 1995
__proto__:
    calculateAge: ƒ ()
    __proto__: Object
```
## Primitive vs Object

- Primitive: While copying or passing to function, it creates a new copy of right hand side (a new memory Location).
- Object: While copying or passing to function, it creates a new reference to same memory location.

```js
// Primitives
var a = 23;
var b = a;  //<- New memory location with same value of a
a = 46;
console.log(a);  // 46
console.log(b);  // 23 (updated value is not reflected)



// Objects
var obj1 = {
    name: 'John',
    age: 26
};
var obj2 = obj1;  // <- A new reference to same memory location
obj1.age = 30;
console.log(obj1.age);  //30
console.log(obj2.age);  //30 (updated value is reflected here)

// Functions
var age = 27;
var obj = {
    name: 'Jonas',
    city: 'Lisbon'
};

function change(a, b) {
    a = 30;
    b.city = 'San Francisco';
}

change(age, obj);

console.log(age);  //27
console.log(obj.city); //San francisco  (as it was a object)

```


## Passing Functions as an argument
dynamically processs data differently functionality based on argument

```js
var years = [1990,1995, 2005,1988]

function calcArray(arr, fn){
   var newArr = []
   for (var i = 0; i < arr.length ; i++)
   {
     newArr.push(fn(arr[i]));
   } 
   return newArr;
}

function calcAge(el)
{
   return 2020 - el
}

function heartRate(el)
{
  if(el >= 18 && el < 60)
  return Math.round(206.9 - (.67 * el))
  else
  return -1
}

var ages = calcArray(years,calcAge)
console.log(ages)

var heartRates = calcArray(ages,heartRate)
console.log(heartRates)
```
## Functions returning function
dynamically return different functionality based on argument
```js
function interviewQn(job)
{
  if(job === 'teacher')
  return function(name){
  console.log('Hi '+name+', what subjects do you teach ? ');
  }

  else if(job ==='designer')
  return function(name){
    console.log('Hi '+name+', what technology you are comfortable ? ');
  }
} 

var teacherInterview = interviewQn('teacher')
teacherInterview('John')

var designerInterview = interviewQn('designer')
designerInterview('Mark')
```

## Immediately Invoked Function Expression (IIFE)

Sometimes for data privacy we may need to another create a function for new local variable inside function scope and call it immediately 

This approach can be replaced by using IIFE approach
Syntax: 

``(function(args){ \\ logic })(args)``
'()' parantheses lets interpreter know that its not a function declaration
but an expression.

Example:

```js
(function () {
  var score = Math.random() * 10;
  console.log(score >= 5);
})();

console.log(score);  // <- not acessible undefined error


(function (goodLuck) {
  var score = Math.random() * 10;
  console.log(score >= 5 - goodLuck);
})(5);

```

## Closures

An inner function has always access to variables and outer function,
even after the outer function has been returned.

```js
function retirement(retirementAge)
{
  var a =" years left until retirement"
   return function(year)
   {      // function has still access
       //to outer variable (a,retirementAge) due to closure
        var age = (2020) - year

        if(age <retirementAge)
        {
          console.log (age + a)  
        }
        else
        {
          console.log('retired..')
        }
   }

}

var retirementUS = retirement(65)
retirementUS(1990)
var retirementGermany = retirement(67)
retirementGermany(1990)
```


## Bind, Apply, Call

- call, apply is used when we need to use method of another object

- The first argument is `this`, where we pass the object to be replaced.

- Only difference is in apply we need to pass arguments in an array
- Bind is used when you need to use a copy of method , with a default argument.


```js
var john = {
  name : 'John',
  age:20,
  job: 'teacher',
  presentation : function(style,timesOfDay)
  {
    if(style === 'formal')
    {
      console.log('Good '+timesOfDay+' , Ladies & Gentlemen ! I\'m ' + this.name +' & I\'m a '+this.job)
    }
    else if(style === 'friendly')
    {
      console.log('Good '+timesOfDay+' , Guys ! I\'m ' + this.name +' & I\'m a '+this.job)
    }
  }
}

john.presentation('formal','morning')

var mary = {
  name : 'mary',
  age:20,
  job: 'designer',
}

john.presentation.call(mary,'friendly','morning')

john.presentation.apply(mary,['friendly','morning'])


var maryPresentation = john.presentation.bind(mary,'friendly')
maryPresentation('morning')

```

Example 2

```js
var years = [1990,1995, 2005,1988]

function calcArray(arr, fn){
   var newArr = []
   for (var i = 0; i < arr.length ; i++)
   {
     newArr.push(fn(arr[i]));
   } 
   return newArr;
}
function getAge (el)
{
  return 2020-el;
}

function isAdult(limit,el)
{
   return  limit < el;
}

var ages = calcArray(years,getAge)
console.log(ages);
                    // bind was use to set an limit for adult age
var checkAdultInJapan = calcArray(ages,isAdult.bind(this,20));
console.log(checkAdultInJapan);
```
