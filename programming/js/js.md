# 🔞 JavaScript

A lightweight, interpreted, or just-in-time compiled programming language

#### Links

- Styleguide - [https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)
- ESLint statically analyzes your code - [https://eslint.org/](https://eslint.org)
- Online IDE - [https://replit.com/](https://replit.com/)

## Basics

### Comments

Example

```javascript
// Coment on new line
console.log("Nice to see you!"); // This code outputs the message to the console

/*  
  The following code outputs the message to the console
  The console will display a line with the text "Hello, JS!"
*/
```

### Variables

The name of a variable can contain only letters, numbers or symbols `$` and `_` and it _cannot_ begin with a number.

#### keywords

* `let` - defines a mutable variable the value of which can be changed as many times as needed
* `const` - declares a variable whose binding (reference) cannot be reassigned . It does not make the value itself immutable
* `var` - an outdated way of declaring a variable

```javascript
let letters, &ampersand, _underscore; // variable examples

let 1number; // SyntaxError: Invalid or unexpected token
```

#### Best Practices

- Use `const` by default
- Use `let` when reassignment is necessary

#### Destructuring Assignment

```js
let [first, ...rest] = "Hello"; // first == "H"; rest == ["e","l","l","o"]

let transparent = {r: 0.0, g: 0.0, b: 0.0, a: 1.0};
let {r, g, b} = transparent; // r == 0.0; g == 0.0; b == 0.0

let points = [{x: 1, y: 2}, {x: 3, y: 4}]; // An array of  two point objects
let [{x: x1, y: y1}, {x: x2, y: y2}] = points; // destructured  into 4 variables.  
(x1 === 1 && y1 === 2 && x2 === 3 && y2 === 4) // => true
```

### Primitive types

Any JavaScript value that is not a number, a string, a boolean, a symbol, null, or undefined is an object. Primitive types are immutable, object types are mutable.

Technically, it is only JavaScript objects that have methods. But numbers, strings, boolean, and  symbol values behave as if they have methods.

#### Numb

```js
x = 1; // Numbers.
x = 0.01; // Numbers can be integers or reals.
```

#### bool

```js
// false:
false
undefined  
null  
0  
-0  
NaN  
"" // the empty string

// all other - true
true
{}              // non-empty object
[]              // empty array
42              // any non-zero number
-42             // negative numbers are truthy
3.14            // floats
"In Code We Trust" // non-empty string
" "             // even a string with just a space
new Date()      // date object
Infinity        // positive or negative Infinity
-Infinity
function() {}   // any function
Symbol("id")    // all Symbols are truthy
```

#### undefined, null

null is a language keyword that evaluates to a special value that is  usually used to indicate the absence of a value

undefined value is also the return value of functions that do not  explicitly return a value and the value of function parameters for which no  argument is passed

```js
x = undefined; // Undefined is another special value like null.

// more undefined examples
let count; 
console.log(count); // undefined

let person = {
  age: 27
}; 
console.log(person.name); // undefined

function getDetails(a) {
  console.log(a);
}
getDetails(); // undefined value returned when a function has a missing parameter:

x = null; // Null is a special value that means "no value."
```

#### Strings

String is an immutable ordered sequence of 16-bit values, each of which typically represents a Unicode character. 

Strings are immutable in JavaScript. Methods like replace() and toUpperCase() return new strings: they do not modify the string on which they are invoked.

```javascript
let s = "hello"; // Start with some lowercase text 
s.toUpperCase(); // Returns "HELLO" , but doesn't alter s 
s // => "hello": the original string has not  changed

// string examples
"" // The empty string: it has zero characters  
'testing'  
"3.14"  
'name="myform"'  
"Wouldn't you prefer O'Reilly's book?"  
"τ is the ratio of a circle's circumference to its radius"  
`"She said 'hi'", he said.`

// A string representing 2 lines written on one line:  
'two\nlines'  
// A one-line string written on 3 lines:  
"one\
long\
line"  
// A two-line string written on two lines:  
`the newline character at the end of this line  
is included literally in this string`
```

unicode

```js
let euro = "€";
let love = "❤️";
euro.length // => 1: this character has one 16-bit element
love.length // => 2: UTF-16 encoding of ❤ is "\ud83d\udc99"
```

Strings can also be treated like read-only arrays

```js
// string as array
let s = "hello, world";
s[0] // => "h"
s[s.length-1] // => "d"
```

##### Template Literals

In ES6 and later, string literals can be delimited with backticks.

Everything between the ${ and the matching } is interpreted as a  JavaScript expression

```javascript
let name = "Bill";
let greeting = `Hello ${name}.`; // greeting == "Hello Bill."

let errorMessage = `\  
\u2718 Test failure at ${filename}:${linenumber}:  
${exception.message}  
Stack trace:  
${exception.stack}  
`;
```

### Object types

A  non-primitive data type that represents an unordered collection of properties. A property is a part of the object that imitates a variable, consists of a key and a value separated by a colon. A key can only be a string, but the value may be of any data type.

JavaScript’s object types are **mutable** and its primitive types are **immutable**.  
JavaScript program can change the values of object properties and array elements.

```javascript
// An object is a collection of name/value pairs, or a string to value map.
let book = {
topic: "JavaScript", 
edition: 7 
}; 

// Access the properties of an object with . or []:
book.topic
book["edition"]

book.author = "Flanagan"; // Create new properties 
book.contents = {}; // {} is an empty object with no properties.

// delete prop.
delete book.author;

// Conditionally access properties with ?. (ES2020): 
book.contents?.ch01?.sect1 // => undefined: book.contents has no ch01 property.
```

#### Array

Object. Ordered collection of numbered values

```javascript
const companies = ["Apple", "Google", "Amazon"];
companies.push("Yandex"); // add new:
companies.pop(); // delete last:
const firstPlace = companies[0] // get by index
companies.length // check length
```

#### Set

represents a set of values

```js
const mySet = new Set([1, 2, 3, 3]);
console.log(mySet); // Set(3) {1, 2, 3}
```

#### Map

represents a mapping from keys to values

```js
const myMap = new Map();
myMap.set('a', 1);
console.log(myMap.get('a')); // 1
```

#### Typed Array

types facilitate operations on arrays of bytes and other binary data.

```js
const buffer = new Uint8Array([10, 20, 30]);
console.log(buffer[1]); // 20
```

#### RegExp

represents textual patterns and enables sophisticated matching, searching, and replacing operations on strings.

```js
const regex = /hello/i;
console.log(regex.test('Hello world')); // true

let text = "testing: 1, 2, 3"; // Sample text  
let pattern = /\d+/g; // Matches all instances of one or more digits
pattern.test(text) // => true: a match exists
text.search(pattern) // => 9: position of first match
text.match(pattern) // => ["1", "2", "3"]: array of all matches  
text.replace(pattern, "#") // => "testing: #, #, #"  
text.split(/\D+/) // => ["","1","2","3"]: split on nondigits
```

#### Date

represents dates and times and supports rudimentary date arithmetic.

```js
const now = new Date();
console.log(now.toISOString()); // e.g., 2025-02-19T13:00:00.000Z
```

#### Error

and its subtypes represent errors that can arise when executing JavaScript code.

```js
try {
  throw new Error('Something went wrong');
} catch (err) {
  console.log(err.message); // Something went wrong
}
```

#### Object creation

Creates a new object and invokes a function (constructor) to initialize the properties.

```js
new Object()  
new Point(2,3)

new Object  
new Date
```

#### Property Access Expressions

JavaScript defines two syntaxes for property access:  
`expression . identifier`  
`expression [ expression ]`

```js
// dot.notation
const user = { name: "Alice", age: 30 };
console.log(user.name); // "Alice"
user.age = 31;

// bracket["notation]
const user = { name: "Alice" };
const key = "name";
console.log(user[key]); // "Alice"

// complex example
const data = {
  items: [
    {
      sku: "bundle_123",
      price: {
        amount: "16.00",
        currency: "USD"
      },
      loyalty_rewards: [
        {
          name: "Pirate Coins",
          amount: 160
        }
      ]
    }
  ]
};

// Dot notation for nested properties
console.log(data.items[0].price.amount);             // "16.00"
console.log(data.items[0].loyalty_rewards[0].amount); // 160

// Bracket notation with string keys
console.log(data["items"][0]["price"]["currency"]);  // "USD"

// Mixed notation with dynamic key
const rewardKey = "loyalty_rewards";
console.log(data.items[0][rewardKey][0].name);       // "Pirate Coins"
```

### Global object

JavaScript stores there  built-in methods, properties, global variables, and functions.

- **Browser Environment**: The global object is window.

```js
console.log(window === this);        // true (in global scope)
console.log(window.Math === Math);   // true
console.log(window.parseInt === parseInt); // true

// Defining global variables
window.myVar = 'Hello, world!';
console.log(myVar); // 'Hello, world!'
```

- **Node.js Environment**: The global object is global.

```js
console.log(global === this); // false (in modules), true (in REPL)
console.log(global.setTimeout === setTimeout); // true

// Defining global variables
global.myVar = 'Hello from Node!';
console.log(global.myVar); // 'Hello from Node!'
```

```js
// Works in both browser and Node.js
console.log(globalThis);
globalThis.myGlobalVariable = 'Universal!';
console.log(myGlobalVariable); // 'Universal!'
```

### Operators

Operators act on values (the operands) to produce a new value.

#### _typeof_ operator

```javascript
console.log(typeof(9)); // number
console.log(typeof 9); // number
```

#### comparison operators

greater than - *gt*

```js
a > b
```

greater than or equal to

```js
a >= b
``` 

*lt* and *lte* accordingly

#### Arithmetic operators

The JavaScript programming language provides operators to perform arithmetic operations. They are called **binary** because they apply to two **operands** (objects over which the operation is performed).

```javascript
3+2 // => 5: addition
3-2 // => 1: subtraction
3*2 // => 6: multiplication
3/2 // => 1.5: division
points[1].x - points[0].x // => 1: more complicated operands also work
"3" + "2" // => "32": + adds numbers, concatenates strings

// JavaScript defines some shorthand arithmetic operators
let count = 0; 
count++; // Increment the variable
count--; // Decrement the variable
count += 2; // Add 2: same as count = count + 2;
count *= 3; // Multiply by 3: same as count = count * 3;
count // => 6: variable names are expressions, too.

//  % for - modulo (remainder after  division)
console.log(10 % 3); // 1
console.log(12 % 4); // 0

// exponentiation **
console.log(2 ** 3); // 8, because (2 * 2 * 2) is 8

// rounding
let x = .3 - .2; // thirty cents minus 20 cents  
let y = .2 - .1; // twenty cents minus 10 cents  
x === y // => false: the two values are not the same!  
x === .1 // => false: .3-.2 is not equal to .1  
y === .1 // => true: .2-.1 is equal to .1

```

The list below is sorted from the highest to the lowest precedence level:

* parentheses
* unary plus/minus
* multiplication, division
* addition and subtraction

#### Logical operators

[https://hyperskill.org/learn/step/8580](https://hyperskill.org/learn/step/8580)

```javascript
console.log(true && true);   // true
console.log(true && false);  // false
console.log(false && true);  // false
console.log(false && false); // false
// || returns false if both operands are false and true in all other cases:
console.log(true || true);   // true
console.log(true || false);  // true
console.log(false || true);  // true
console.log(false || false); // false
// ! returns false to true and true to false: 
console.log(!false); // true
console.log(!true);  // false
```

Among the numerical values, `0` is considered `false`, and all other numbers are true. Strings are considered true.

Expression is always calculated from left to right. `&&` returns \*\*\*\* _false_ as soon as it finds the first occurring \*\*\*\* _false_, and the operator `||` returns \*\*\*\* _true_ as soon as it sees the first \*\*\*\* _true_:

```javascript
console.log(true || 0);      // true
console.log(false && "sun"); // false
console.log(1 || 0);         // 1
```

The priority of `!` is higher than that of `&&`, and the priority of `&&` is higher than that of `||`. If you need to change the priority, use parentheses:

```javascript
console.log(!false && !true);   // false
console.log(!(false && !true)); // true
```

#### in

expects: 
- left-side operand that is a string, symbol, or  value that can be converted to a string
- right-side operand - an object

```js
let point = {x: 1, y: 1}; // Define an object  
"x" in point // => true: object has property named "x"  
"z" in point // => false: object has no "z" property.
"toString" in point // => true: object inherits toString method
let data = [7,8,9]; // An array with elements (indices) 0, 1, and 2  
"0" in data // => true: array has an element "0"  
1 in data // => true: numbers are converted to  strings
3 in data // => false: no element 3
```

#### instanceof

expects:
- a left-side operand that is an object 
- a right-side operand that identifies a class of objects

object in JavaScript **inherits from Object.prototype** unless explicitly created without a prototype

```js
let d = new Date(); // Create a new object with the Date() constructor
d instanceof Date // => true: d was created with Date()
d instanceof Object // => true: all objects are instances of Object
d instanceof Number // => false: d is not a Number object

let a = [1, 2, 3]; // Create an array with array literal syntax
a instanceof Array // => true: a is an array
a instanceof Object // => true: all arrays are objects
a instanceof RegExp // => false: arrays are not regulal expressions
```

### Type conversion

`==` operator with a flexible definition  of equality

```js
null == undefined // => true: These two values are treated as equal.
"0" == 0 // => true: String converts to a number
0 == false // => true: Boolean converts to number
"0" == false // => true: Both operands convert to 0
```

##### String conversion

In JS, an _implicit conversion_ will be called by the binary `+` operator when one of the operands is a string:

```javascript
"3" + 4                        // "34"
4 + ""                         // "4"
true + "detective"             // "truedetective"
"You are " + 25 + " years old" // "You are 25 years old"
```

The automatic conversion will take place regardless of the location of the operand string on the right or left side of the expression.

Remember the order of arithmetic operations. If there are several numbers before the string, these numbers will be added before the conversion:

```javascript
3 + 10 + "1" // "131", not "3101"
```

#### Numeric conversion

When converting a string to a number, spaces and characters `\n,`  at the beginning and the end of the string are cut off. If the string turns out to be empty, the result will be `0`. The boolean type behaves as expected: `false` turns into `0`, `true` turns into `1`.

If the values ​​cannot be cast to a number, the result of the conversion will be `NaN`. For example, `Number("apple")` will return a `NaN` value, which means **Not-a-Number**. Usually, this value is returned when an operation with numbers is performed incorrectly.

The _implicit_ conversion is a little more confusing. It occurs in almost all mathematical functions and expressions:

```javascript
true + 43 // 44
3 - false // 3
10 / "5"  // 2
-true     // -1
+"85"     // 85
```

### Boolean conversion

The rules for using this function are simple: values that imply "empty", like `0` or an empty string `""` turn into `false`. All other values turn into `true`.

An _implicit_ conversion occurs when using logical operators (`||&&` `!`):

```javascript
!!3                      // true
0 || "hello"             // "hello"
"Master" && "Margarita"  // "Margarita"
```

#### Explicit Conversions

```js
Number("3") // => 3
String(false) // => "false": Or use false.toString()  
Boolean([]) // => true

x + "" // => String(x)  
+x // => Number(x)  
x-0 // => Number(x)  
!!x // => Boolean(x): Note double !
```

## built-in Functions

### Console.log()

print some text

```javascript
console.log("Learning JavaScript is easy and enjoyable!");
```

### String()

```javascript
String(123);   // "123"
String(false); // "false"
```

### Number()

```javascript
Number("1");    // 1
Number(" 37 "); // 37
Number("");     // 0
Number("\n3");  // 3
Number("\n");   // 0
Number("\t");   // 0
Number(true);   // 1
Number(false);  // 0
```

### Boolean()

```javascript
Boolean(1);            // true
Boolean(0);            // false
Boolean("Am I nice?"); // true
Boolean("");           // false
```

### fetch()

The global **`fetch()`** method starts the process of fetching a resource from the network, returning a promise which is fulfilled once the response is available.

Syntax

```javascript
fetch(resource)
fetch(resource, options) // response - Promise with Response object

// example
fetch('https://domain.com/api/v1/purchases?limit=50&offset=50', 
      {
          headers: 
          {
              'Authorization': 'Bearer blalax'
          }
});
	.then((response) => response.json());
	.then((data) => data);
	.catch((err) => { ... });
```

## user-defined Functions

### default style

```javascript
function plus1(x) {
    return x + 1;
};

plus1(y);

let square = function (x) { // Functions are values and can be assigned to vars
    return x * x;
};

square (plus1(y)) // => 16: invoke two functions in one line
```

### arrow functions style

Arrow functions are most commonly used when you want to pass an unnamed function as an argument to another function.

```javascript
const name = (a, b) => {
  const result = a + b
  return result;
};

two = (a) => b = 2 + a; // it could be one-line function
```

* Arrow functions don't have their own bindings to [`this`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this), [`arguments`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments) or [`super`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super), and should not be used as [methods](https://developer.mozilla.org/en-US/docs/Glossary/Method).
* Arrow functions don't have access to the [`new.target`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target) keyword.
* Arrow functions aren't suitable for [`call`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Function/call), [`apply`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Function/apply) and [`bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Function/bind) methods, which generally rely on establishing a [scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope).
* Arrow functions cannot be used as [constructors](https://developer.mozilla.org/en-US/docs/Glossary/Constructor).
* Arrow functions cannot use [`yield`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield), within its body.

**Формальными параметрами** функции называются имена переменных в _определении функции_. Например у функции `const f = (a, b) => a - b;` формальные параметры — это `a` и `b`.

**Фактические параметры** — это то, что было _передано в функцию_ в момент вызова. Например если предыдущую функцию вызвать так `f(5, z)`, где `const z = 8`, то фактическими параметрами являются `5` и `z`. Результатом этого вызова будет число `-3`, а внутри функции на момент конкретного вызова параметр `a` становится равным `5`, а `b` становится равным `8`.

### recursion

```javascript
const factorial = (n) => {
  if (n === 0) {
    return 1;
  }
  else {
    return n * factorial(n - 1);
  }
}

const answer = factorial(3);
```

### Callback Functions

the way to create a callback function is to pass it as a parameter to another function, and then to call it back right after something has happened or some task is completed

```javascript
const message = function() {  
    console.log("This message is shown after 3 seconds");
}
setTimeout(message, 3000);

//Anonymous Function
setTimeout(function() {  
    console.log("This message is shown after 3 seconds");
}, 3000);
//Arrow Function
setTimeout(() => { 
    console.log("This message is shown after 3 seconds");
}, 3000);
```

### async-await

The [`async`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) keyword gives you a simpler way to work with asynchronous promise-based code. Adding `async` at the start of a function makes it an async function.

Inside an async function, you can use the `await` keyword before a call to a function that returns a promise. This makes the code wait at that point until the promise is settled, at which point the fulfilled value of the promise is treated as a return value, or the rejected value is thrown.

```js
// modern cool async function
async function foo() {
  await 1;
}

// old and ugly, but same function
function foo() {
  return Promise.resolve(1).then(() => undefined);
}
```

```javascript
async function fetchProducts() {
  try {
    // after this line, our function will wait for the `fetch()` call to be settled
    // the `fetch()` call will either return a Response or throw an error
    const response = await fetch('https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json');
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    // after this line, our function will wait for the `response.json()` call to be settled
    // the `response.json()` call will either return the parsed JSON object or throw an error
    const data = await response.json();
    console.log(data[0].name);
  }
  catch (error) {
    console.error(`Could not get products: ${error}`);
  }
}

fetchProducts();
```

```javascript
async function getStarWarsMovie(id) {
  const response = await fetch(`https://swapi.dev/api/films/${id}/`)
  console.log("ответ получен", response) // *1
  return response.json()
}

const movies = getStarWarsMovie(1).then((movie) => {
  console.log(movie.title)
}) // *2
console.log("результат вызова функции", movies) // *3
```

#### parallel execution

- `await Promise.all()` - parallel execution of several independen functions (expects success)
- `Promise.allSettled()` - parallel exec. expects both err/success

```js
async function getUser(){..} // return userinfo

async function getNews(){..} // return newsfeed

// one by one
const user = await getUser() // wait for execution
const news = await getNews() // run after getUser()

// parallel
const [user, news] = await Promise.all([
  getUser(),
  getNews()
])
```


## Methods

When functions are assigned to the properties of an object, we call them "methods."

```javascript
// All JavaScript objects (including arrays) have methods:
let a = []; // Create an empty array
a.push(1,2,3); // The push() method adds elements to an array
a.reverse(); // Another method: reverse the order of elements 

// We can define our own methods, too. The "this" keyword refers to the object
// on which the method is defined: in this case, the points array from earlier.
points.dist = function() { // Define a method to compute distance between points
    let p1 = this[0]; // First element of array we're invoked on
    let p2 = this[1]; // Second element of the "this"object
    let a = p2.x-p1.x; // Difference in x coordinates
    let b = p2.y-p1.y; // Difference in y coordinates
    return Math.sqrt(a*a + b*b); // Math.sqrt() computes the square root
};

points.dist() // => Math.sqrt(2): distance between our 2 points
```

### array.sort()

default behavior of `sort()` is to sort values alphabetically, which doesn't work correctly for numbers.

When sort() is called without any arguments, it sorts the elements of the array in lexicographic order, which means that it sorts the elements as strings.

However, when sort() is called with a comparison function, it sorts the elements of the array based on the result of the comparison function. In this case, the comparison function (a, b) => a - b subtracts the second number from the first number, which produces a negative number if a is less than b, zero if a equals b, or a positive number if a is greater than b. This comparison function ensures that the resulting array will be sorted in ascending order.

```javascript
let numbers = [1, 30, 4, 21, 100000];
console.log(numbers.sort()); // => [ 1, 100000, 21, 30, 4 ]
console.log(numbers.sort((a, b) => a - b));// => [ 1, 100000, 21, 30, 4 ]
```

## XMLHttpRequest (XHR)

Objects are used to interact with servers. You can retrieve data from a URL without having to do a full page refresh.

`XMLHttpRequest` is used heavily in [AJAX](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX) programming.

Despite its name, `XMLHttpRequest` can be used to retrieve any type of data, not just XML.

```javascript
function reqListener() {
  console.log(this.responseText);
}

const req = new XMLHttpRequest();
req.addEventListener("load", reqListener);
req.open("GET", "http://www.example.org/example.txt");
req.send();
```

## Promises

- [doka(ru)](https://doka.guide/js/promise/)

Promise is an object with 3 states

* **Pending:** Initial State, before the Promise succeeds or fails
* **Resolved:** Completed Promise
* **Rejected:** Failed Promise

![](../../aaa-assets/js-1.png)

It takes two parameters, one for success (resolve) and one for fail (reject):

Finally, there will be a condition. If the condition is met, the Promise will be resolved, otherwise it will be rejected:

```javascript
const myPromise = new Promise((resolve, reject) => {  
    let condition;  
    
    if(condition is met) {    
        resolve('Promise is resolved successfully.');  
    } else {    
        reject('Promise is rejected');  
    }
});
```

## Classes

```javascript
class Human { // class name with Capital Letter
    constructor(name, gender) { // Constructor function to initialize new instances.
        this.name = name;
        this.gender = gender; 
    }                         // No return is necessary in constructor functions.
    sayMyName() {             // add method to class
        console.log("My name is " + this.name);
    } 
}

let idealHuman = new Human("John", "investigating"); // create new instance

idealHuman.sayMyName(); // call method 
```

## Conditional operators

#### equality

the `==` equality operator is deprecated in favor of the strict equality operator `===`, which does no type conversions

### if

```javascript
let condition = null; 
 
if (condition === null) {
    console.log("True!");
} else if (condition === 10) {
    console.log("Wow its 10!")
}
else {
    console.log("Not True");   
}
```

### ternary operator "? :"

short version of the `if...else` block

```javascript
const getColour = colour => colour === 'white' ? 'white' : 'black';
```

## Loops

### for

```javascript
const names = ["JD", "DJ", "Dr. D"]
for (let i = 0; i < names.length; i++) {
    console.log(names[i]);
}
// another example
let arr = [1, 2, 5, -3, 15, 20, 13, -3, -5, -10, 22, 14]
// Задача — найти все отрицательные элементы
let found = []
for (let i = 0; i < arr.length; i++) {
  if (arr[i] < 0) {
    found.push(arr[i])
  }
}
console.log(found)
// reverse for
for (let i = Name.length-1; i >= 0; i--)   
```

### while

```javascript
let answer = "sun";
let guess = "";

while (guess != answer) {
    guess = prompt("What do you see?");
}

alert("Yep!");
```

### do while

```javascript
let factorial = 1;
let number = 5;
let original = number;

do {
    factorial = factorial * number;
    number--
} while (number > 0);

console.log(original + " factorial is " + factorial);
```

## XHR vs fetch

![](../../aaa-assets/js-2.png)

Instead of having to write code like:

```javascript
function reqListener() {
    var data = JSON.parse(this.responseText);
}

function reqError(err) { ... }

var oReq = new XMLHttpRequest();
oReq.onload = reqListener;
oReq.onerror = reqError;
oReq.open('get', './api/some.json', true);
oReq.send();
```

we can clean things up and write with promises and modern syntax

```javascript
fetch('./api/some.json')
    .then((response) => {
        response.json().then((data) => { 
            ... 
        });
    })
    .catch((err) => { ... });
```

## Math

Arithmetic in JavaScript does not raise errors in cases of overflow, underflow, or division by zero. When the result of a numeric operation is larger than the largest representable number (overflow), the result is a special infinity value, Infinity.

Division by zero is not an error in JavaScript: it simply returns infinity or negative infinity. There is one exception, however: zero divided by zero does not have a well-defined value, and the result of this operation is the special not-a-number value, NaN.

```javascript
Math.pow(2,53) // => 9007199254740992: 2 to the power 53 
Math.round(.6) // => 1.0: round to the nearest integer
Math.ceil(.6) // => 1.0: round up to an integer
Math.floor(.6) // => 0.0: round down to an integer
Math.abs(-5) // => 5: absolute value
Math.max(x,y,z) // Return the largest argument
Math.min(x,y,z) // Return the smallest argument
Math.random() // Pseudo-random number x where 0 <= x < 1.0
Math.PI // π: circumference of a circle diameter
Math.E // e: The base of the natural logarithm
Math.sqrt(3) // => 3**0.5: the square root of 3
Math.pow(3, 1/3) // => 3**(1/3): the cube root of 3
Math.sin(0) // Trigonometry: also Math.cos Math.atan, etc.
Math.log(10) // Natural logarithm of 10
Math.log(100)/Math.LN10 // Base 10 logarithm of 100 
Math.log(512)/Math.LN2 // Base 2 logarithm of 512 Math.exp(3)

// ES6
Math.cbrt(27) // => 3: cube root
Math.hypot(3, 4) // => 5: square root of sum of squares of all arguments
Math.log10(100) // => 2: Base-10 logarithm
Math.log2(1024) // => 10: Base-2 logarithm
Math.log1p(X) // Natural log of (1+x); accurate for very small X
Math.expm1(x) // Math.exp(x)-1; the inverse of Math. logip ()
Math.sign(X) // -1, 0, or 1 for arguments <, ==, or > 0
Math.imul(2,3) // => 6: optimized multiplication of 32-bit integers
Math.c1z32(0xf) // => 28: number of leading zero bits in a 32-bit integer
Math.trunc(3.9) // => 3: convert to an integer by truncating fractional part
Math.fround(x) // Round to nearest 32-bit float number
Math.sinh(x) // Hyperbolic sine. Also Math.cosh(),Math.tanh()
Math.asinh(x) // Hyperbolic arcsine. Also Math.acosh (),Math.atanh()
```

## Dates and Times

```javascript
let timestamp = Date.now(); // The current time as a timestamp (a number).
let now = new Date(); // The current time as a Date object.
let ms = now.getTime(); // Convert to a millisecond timestamp.
let iso = now.toISOString(); // Convert to a string in standard format.
```
