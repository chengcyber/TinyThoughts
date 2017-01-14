# Arrow Function

```javascript
var foo = function () {
    return bar;
}

var foo = () => bar;
```

## this in arrow function


```javascript
/*
 * ES 5: this lost when the second depth function called
 */
var deliveryBoy = {
    name: "John",
    
    handleMessage: function(message, handler) {
        handler(message);
    },
    
    receive: function() {
        var self = this;
        
        this.handleMessage("hello" , function(message) {
            conosle.log(message + self.name);
        }
    }
}

// ES 6 
var deliveryBoy = {
    ...
    
    receive: () => {
        this.handleMessage("hello", (message) => console.log(message + this.name);
    }
} 
```

# The let keyword

ES5:
```javascript
var message = 'hi';
{
    message = 'bye';
}

console.log(message); // bye
```


```javascript
var fs = [];

for (var i = 0; i < 4; i++) {
    fs.push(function() {
        console.log(i);
    }
}

fs.forEach(function(f) {
    f();
});

// 4 4 4 4
```

The ES6 `let` have a block scope.


```javascript
let message = 'hi';

{
    message = 'bye';
}

console.log(message); // hi
```


```javascript
var fs = [];

// use let in for-loop statement
for (let i = 0; i < 4; i++) {
    fs.push(function() {
        console.log(i);
    }
}

fs.forEach(function(f) {
    f();
});

// 0 1 2 3 
```

# Default values for Function Parameters


```javascript
function greet(greeting, name = 'John') {
    console.log(greeting + ', ' + name);
}

greeting('Hello');
// Hello, John
```

# const Declaration


```javascript
const VALUE = {};
VALUE = 'bar';
// error : VALUE is ready-only

const VALUE = {};
VALUE.foo = 'bar';
console.log('value: ' + VALUE);
// value: { foo : 'bar' }

// Usage: 
XXX_API_KEY
XXX_SECRET
XXX_PORT

// Block Scope
if (true) {
    const foo = 'bar';
    console.log(foo); // bar
}
console.log(foo); // foo is not defined
```

# Shorthand Properties


```javascript
let firstName = 'John';
let lastName = 'Lindquist';

let person = {firstName, lastName};

console.log(person); 
// { firstName: 'John', lastName: 'Lindquist' }

let admin = 'Admin';
let team = {person, admin};

console.log(team);
// { person: { firstName: 'John', lastName: 'Lindquist' }, admin: 'Admin'}

 
```

# Object Enhancements


```javascript
var color = 'red';
var speed = 10;

var car = {color, speed};
// same as 
// var car = {color: color, speed: speed};

console.log(car.color);
console.log(car.speed);
// red
// 10

var car = {
    color,
    speed,
    go() {
        console.log('go called');
    }

car.go();
// go called

var car = {
    color,
    speed,
    ["go"]: function() {
        console.log('go called');
    }
    
car.go();
// go called

var drive = 'go';
var car = {
    color,
    speed,
    [drive]: function() {
        console.log('go called');
    }

car.go();
// go called

```

# Spread Operator


```javascript
// Spread Operator ...

console.log(...[1, 2, 3]);
// 1 2 3

```

```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

/*
arr1.push(arr2);

console.log(arr1);
// [1, 2, 3, [4, 5, 6]]
*/
arr1.push(...arr2);

console.log(arr1);
// [1, 2, 3, 4, 5, 6]
```

```javascript
arr = [1, 2, 3]

var sumThree = (a, b, c) => console.log(a + b + c);

sumThree(...arr);
// 6
```


# String Template


```javascript
/*
 * ES5 concatenate string
 */
 var prefix = 'hello ';
 var str = prefix + 'world';
 console.log(str); // hello world
 
 /*
  * ES6 String template
  */
var prefix = 'hello ';
var str = `${prefix}, world`;
console.log(str); // hello, world


var str = `

${prefix}
    .
        /
            world
`;
console.log(str);
//
//hello
//  .
//      /
//          world
//
 
```

## Parse function


```javascript
var parse = (strings, ...values) => {
    console.log(strings);
    console.log(values);
}

var message = parse`It's ${new Date.getHours()} sleepy`;
console.log(message);

//[ 'It\'s ', ' sleepy']
//[15]
//undefined
```


```javascript
var parse = (strings, values) => {
    if(values[0] < 20) {
        values[1] = 'awake';
    }
    
    return `${strings[0]}${values[0]}${strings[1]}${values[1]}`;
}

var message = parse`It's ${new Date.getHours()} ${""}`;
console.log(message);

// It's 15 awake
```

# Destructing Assignment


```javascript
/*
 * ES5 
 */
var obj = {
    color: 'blue'
}

console.log(obj.color);
//blue
```

```javascript
// ES6
var {color} = {
    color: 'blue'
}
console.log(color);
// blue
```

```javascript
// bigger demo
function generateObj() {
    return {
        firstName: 'John',
        lastName: 'Lindquist',
        id: 123.
        email: 'foo@example.com'
    }
}
var {firstName, lastName} = generateObj();
console.log(firstName);
console.log(lastName);
// John
// Lindquist

/* Alias using colon sign */
var {firstName: firstAlias, lastName: secondAlias} = generateObj();
console.log(firstAlias);
console.log(secondAlias);
// John
// Lindquist

```


```javascript
// destructing with array
var [one,,,,five] = ['red', 'yellow', 'blue', 'orange', 'green'];

console.log(one);
console.log(five);
// red
// green
```

# ES6 Module - import and export

[tutorial on babeljs.io](https://babeljs.io/learn-es2015/#ecmascript-2015-features-modules)


```javascript
// lib/math.js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```

```javascript
// app.js
import * as math from "lib/math";
console.log("2π = " + math.sum(math.pi, math.pi));
```

```javascript
// otherApp.js
import {sum, pi} from "lib/math";
console.log("2π = " + sum(pi, pi));
```

# Convert array-like Object - Array.from()

```javascript
/*
 * ES5, slice() to convert array-like Object.
 */ 
function list() {
  return Array.prototype.slice.call(arguments);
}

var list1 = list(1, 2, 3); // [1, 2, 3]
```


```html
<ul>
    <li class="product">15.99</li>
    <li class="product">7.99</li>
    <li class="product">24.99</li>
</ul>
```

```javascript
const products = document.querySelectAll('.product');

console.log(products)
// Array-like object - NodeList
```

Use Array.from() to transform to array
```javascript
const products = Array.from(document.querySelectAll('.product'));

products
    .filter(product => parsFloat(product.innerHTML) < 10)
    .forEach(product => product.style.color = 'red');
// highlight every <li> elements less than 10

```

# Promises with ES6

new Promise() accpets a single function specified resolve and reject

then() contains the real business logic

catch() catches every error during the promise processing

```javascript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

# Generators

function* means make a generator

function launch to the next yield position when next() called.


```javascript
function* greet() {
    console.log(`Generators are "Lazy"`);
    yield "How";
    console.log(`I'm not called until the second next`);
    yield "are";
    console.log(`Call me before "you?"`);
    yield "you?";
    console.log(`Called  when "done"`);
}

var greeter = greet();
console.log(greeter().next());
// Generator are "Lazy"
// { value: 'How', done: false }

console.log(greeter().next());
// I'm not called until the second next
// { value: 'are', done: false }

console.log(greeter().next());
// Call me before "you?"
// { value: 'you?', done: false }

console.log(greeter().next());
// Called when "done"
// { value: undefined, done: true }

```

```javascript
/* other output way */

console.log(greeter().next().value);
console.log(greeter().next().value);
console.log(greeter().next().value);
console.log(greeter().next().value);


/* other output way */
for (let word of greeter) {
    console.log(word);
}

/* Same output */
    
// Generator are "Lazy"
// How
// I'm not called until the second next
// are
// Call me before "you?"
// you?
// Called when "done"
```


# Maps and WeakMaps

## Map

a data structure to store key-value pairs

```javascript
var m = new Map();

/*
 * - APIs -
 * set()
 * get()
 * size()
 * clear()
 * has()
 * --------
 */
m.set('hello', 'world');
m.set('foo', 'bar');

console.log(m.get('qwerty'));
// undefined

console.log(m.get('qwerty'));
// false

console.log(m.size());
// 2

console.log(m.get('foo'));
// bar
```

```javascript
/*
 * Iterator on Map
 * keys()
 * values()
 * entries()
 */
for (var key of m.keys()) {}

for (var value of m.values()) {}

for (var [key, value] of m.entries()) {} 
```

## WeakMap

diff from Map
- key must be a object
- weak reference to key, means if key object is no longer used, the key-value stored in WeakMap will be reclaimed by GC (Garbage Collection)
- no method about size and iterators

If the object used as key will be removed in business logic, using WeakMap will not cause Memory-Leak.

```javascript
var wm = new WeakMap();

var obj = {};

wm.set(obj, 'bar');
wm.set('string', 2);
// Invalid value used as weak map key

// no iterators now

```

# ES6 Destructing with Required Values


```javascript
function ajax({
    type = 'get',
    // url = '',
    url = requiredParameter("url"),
    data = {},
    // success = () => {},
    success = requiredParameter("success"),
    error = () => {},
    isAsync = true} = {}) {
    console.log(JSON.stringify({type, url, data, succes, error, isAsync}, null, 2));
}

function requiredParameter(name) {
    throw new Error(`Missing parameter "${name}"`);
}

try {
    ajax({
        url: 'http://my.api.io',
        success: () => {}
    });
} catch (e) {
    console.warn(e.message);
}

// console output when url is not given when ajax called
// "Missing parameter 'url'"
```

# Rest Paramters

... in function parameter

goal: deprecated arguments


```javascript
function Store() {
  var aisle = {
    fruit: [],
    vegetable: []
  }
  return {
    //Store().add('category', 'item1', 'item2');
    add: function(category) {
      var items = [].splice.call(arguments, 1);
      console.log(items);
      
      items.forEach(function(value, index, array) {
        aisle[category].push(value);
      });
    },
    aisle: aisle
  }
}

// using arguments and slice() to get [item1, item2]

/* using rest parameter to replace arguments solution */

    ...
    add: function(category, ...items) {
        console.log(items);
        ...
    }

// implement same function without call build-in arguments

```


# Resources

- [Learn ES2015](https://babeljs.io/learn-es2015/)
- [ES2015 course in Egghead](https://egghead.io/courses/learn-es6-ecmascript-2015)


