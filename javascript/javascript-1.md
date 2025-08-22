
# this: this keyword is refers to show browser window object and nodejs compiter it show global object. it used for current object as a constructor to assign value of object properies
***
null: Null is used to represent an intentional absence of any object value
undefined: indicates a variable is declared but not assigned a value
call, app, bind are methods in javascript that allow to manipulate the THIS keyword within function
call() method is used to call a function with specified THIS value and arguments in individually
apply() method is used to similar like call but we need pass arguments in array
bind() method create new function that want to called for execution we can pass argument type or array
hoisting: default behavior of moving declarations to the top of the current scope (script or function) during the compilation phase.
var: var is global scope
let and const: is function scope in let variable assigned new value but we cannot reassigned same name variable and const is fixed value we cannot reassign value or same variable name but we can modify with non-primitive types like object and array
function declaration: a function defines a function using the function keyword followed by function\_name, parentheses and curly braces
function expression: a function defines a function and assigns it to variable is
closure: A closure is created when a function is defined inside another function. The inner function retains access to the variables in the outer function's scope. the outer function has completed execution, as long as the inner function still exists
Currying: this functional programming concept that takes multiple arguments is transformed into sequence of functions that each take a single argument
Event Bubbling: The event starts from the target element and propagates upward to its ancestors it is a default behaviour.
Event Capturing: The event propagates downward from the root to the target when true is passed in addEventListener().
Event Delegation: A single event listener on a parent element handles events from its children using event.target.
////////////////////////
Shallow copy: shallow copy creates a new object or array, but only one level deep.
Deep copy: A deep copy creates a completely new object or array, and it recursively copies all nested objects or arrays.
map(): map transforms each element via callback function and return a new array with the tranformed values
forEach(): Iterates over an array and executes a provided callback function for each element, primarily for performing side effects. It does not return a value
filter(): filter creates a new array containing only the elements that satisfy the condition specified in the callback function
reduce(): is a higher-order array method that accumulates into a single output value using callback function.
find(): Use when you need to find the first match.
findIndex(): Use when you need the index of the first match.
some(): Use to check if at least one element matches a condition.
every(): Use to check if all elements match a condition.
sort(): Use to reorder elements.
includes(): Use to check if an element exists.
concat(): Use to combine arrays.
flat(): Use to flatten nested arrays.
join(): Use to join elements into a string.
slice(): Extracts a portion of an array (or string) and returns a new array without changing the original. Use: Get a subset of data non-destructively.
splice(): Modifies an array by adding, removing, or replacing elements at a specified index; returns the removed elements. Use: Alter the original array dynamically.
split(): Divides a string into an array of substrings based on a delimiter; doesn’t affect the original string.
Use: Parse or break down strings into manageable parts.
Rest → The rest operator collects all remaining arguments passed to a function into an array. rest operator must be the last parameter.
Spread → Expands elements from an array/object into individual items.
Array Destructuring: Extract values from an array into variables
Object Destructuring: Extract values from an object into variables.
///////////////////////////
Promises: Handling asynchronous operations providing a clear and more manageable approach to traditional callback based methods. promise states are pending, fulfilled, rejected
Promises.all(): Waits for all promises to resolve and returns their results as an array. If any promise is rejected, it immediately rejects all promises.
let p1 = new Promise((resolve) => setTimeout(() => resolve("One"), 1000));
let p2 = new Promise((resolve) => setTimeout(() => resolve("Two"), 2000));
let p3 = new Promise((resolve, rej) => setTimeout(() => rej("Three"), 3000));
Promise.all([p1, p2, p3]).then(console.log).catch(console.log);
Promise.allSettled(): Waits for all promises to settle (fulfilled or rejected) Method and returns an array of their outcomes.
Promise.allSettled([Promise.resolve(1), Promise.reject("Error")])
.then(console.log);
Promise.race(): Resolves/rejects with the first completed promise.
Promise.race([new Promise((res) => setTimeout(res, 100, "Fast")),
new Promise((res) => setTimeout(res, 500, "Slow"))])
.then(console.log); // Output: Fast
Promise.any():  Resolves with the first fulfilled promise (ignores rejections).
Promise.any([Promise.reject("Error"), Promise.resolve("Success")])
.then(console.log); // Output: Success
async/await: async/await is a modern way to handle asynchronous code, making it look synchronous and more readable. async makes a function return a Promise, and await pauses execution until the Promise resolves.
//Object
Object.freeze: Makes an object completely immutable. Cannot add, delete, or modify properties. All properties become non-writable
Object.seal: Makes an object modification of existing property values. Cannot add or delete properties.
/////////////////////
OOPS: object oriented programming structure to organizes code into objects that properties(data) and methods(functions)
encapsulation: is the building of properties and methods into a single object. it restricts direct access to some of the object components
inheritance: it allows a class (child) to inherit properties and method from another class using extends keywords
polymorphism: it allows methods in different classes to share the same name but behave differently based context
abstraction: it hides complex implementation details and exposes only the necessary functionality. JavaScript doesn’t have built-in abstract classes but can achieve abstraction using classes and methods.
Classes: A blueprint for creating objects. It defines properties and methods
Object: An instance of a class.
/////////////////////////
***
generate code make like questions, answers, output, and explanation.

I'll create comprehensive interview-style questions with code examples based on your JavaScript concepts. Here's a structured set of questions, answers, outputs, and explanations:

## **JavaScript Fundamentals**

### **Question 1: `this` Keyword Behavior**

```javascript
// Code Example
const obj = {
    name: 'Alice',
    greet: function() {
        console.log(`Hello, ${this.name}`);
    },
    greetArrow: () => {
        console.log(`Hello, ${this.name}`);
    }
};

obj.greet();
obj.greetArrow();

const greetFunc = obj.greet;
greetFunc();
```

**Answer \& Output:**

```
Hello, Alice
Hello, undefined
Hello, undefined (or error in strict mode)
```

**Explanation:**

- Regular functions: `this` refers to the object that calls the method
- Arrow functions: `this` is lexically bound to the enclosing scope
- Standalone function calls: `this` refers to global object (window/global) or undefined in strict mode

***

### **Question 2: Call, Apply, Bind Methods**

```javascript
const person = {
    name: 'John',
    age: 30
};

function introduce(city, country) {
    console.log(`Hi, I'm ${this.name}, ${this.age} years old from ${city}, ${country}`);
}

// Using call, apply, bind
introduce.call(person, 'New York', 'USA');
introduce.apply(person, ['London', 'UK']);
const boundIntroduce = introduce.bind(person, 'Tokyo');
boundIntroduce('Japan');
```

**Answer \& Output:**

```
Hi, I'm John, 30 years old from New York, USA
Hi, I'm John, 30 years old from London, UK
Hi, I'm John, 30 years old from Tokyo, Japan
```

**Explanation:**

- `call()`: Immediately invokes function with specified `this` and individual arguments
- `apply()`: Same as call but arguments passed as array
- `bind()`: Returns new function with bound `this`, can be called later

***

## **Scoping and Hoisting**

### **Question 3: Hoisting Behavior**

```javascript
console.log(a); // What will this print?
console.log(b); // What will this print?
console.log(c); // What will this print?

var a = 5;
let b = 10;
const c = 15;

function hoistTest() {
    console.log(x); // What will this print?
    var x = 'inside function';
    console.log(x); // What will this print?
}

hoistTest();
```

**Answer \& Output:**

```
undefined
ReferenceError: Cannot access 'b' before initialization
ReferenceError: Cannot access 'c' before initialization
undefined
inside function
```

**Explanation:**

- `var` declarations are hoisted and initialized with `undefined`
- `let` and `const` are hoisted but not initialized (temporal dead zone)
- Function-scoped `var` follows same hoisting rules within functions

***

## **Functions and Closures**

### **Question 4: Closure Example**

```javascript
function createCounter() {
    let count = 0;
    
    return function() {
        count++;
        return count;
    };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1()); // ?
console.log(counter1()); // ?
console.log(counter2()); // ?
console.log(counter1()); // ?
```

**Answer \& Output:**

```
1
2
1
3
```

**Explanation:**
Each call to `createCounter()` creates a new closure with its own `count` variable. The inner function retains access to its outer function's scope even after the outer function returns.

***

### **Question 5: Currying Implementation**

```javascript
// Regular function
function add(a, b, c) {
    return a + b + c;
}

// Curried version
function curriedAdd(a) {
    return function(b) {
        return function(c) {
            return a + b + c;
        };
    };
}

// Usage
console.log(add(1, 2, 3)); // ?
console.log(curriedAdd(1)(2)(3)); // ?

// Partial application
const addOne = curriedAdd(1);
const addOneAndTwo = addOne(2);
console.log(addOneAndTwo(3)); // ?
```

**Answer \& Output:**

```
6
6
6
```

**Explanation:**
Currying transforms a function that takes multiple arguments into a sequence of functions that each take a single argument, enabling partial application and function composition.

***

## **Array Methods**

### **Question 6: Array Methods Comparison**

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

// map vs forEach
const doubled = numbers.map(n => n * 2);
console.log('Doubled:', doubled); // ?

const sideEffect = [];
numbers.forEach(n => sideEffect.push(n * 2));
console.log('Side effect:', sideEffect); // ?

// filter vs find
const evens = numbers.filter(n => n % 2 === 0);
console.log('Evens:', evens); // ?

const firstEven = numbers.find(n => n % 2 === 0);
console.log('First even:', firstEven); // ?

// reduce example
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log('Sum:', sum); // ?
```

**Answer \& Output:**

```
Doubled: [2, 4, 6, 8, 10, 12]
Side effect: [2, 4, 6, 8, 10, 12]
Evens: [2, 4, 6]
First even: 2
Sum: 21
```

**Explanation:**

- `map()`: Returns new array with transformed elements
- `forEach()`: Executes function for each element, no return value
- `filter()`: Returns new array with elements that pass test
- `find()`: Returns first element that passes test
- `reduce()`: Accumulates array elements into single value

***

## **Modern JavaScript Features**

### **Question 7: Destructuring and Spread/Rest**

```javascript
// Array destructuring with rest
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first, second, rest); // ?

// Object destructuring
const user = { name: 'Alice', age: 25, city: 'Boston' };
const { name, ...otherProps } = user;
console.log(name, otherProps); // ?

// Spread operator
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined); // ?

// Function with rest parameters
function sum(...numbers) {
    return numbers.reduce((acc, num) => acc + num, 0);
}
console.log(sum(1, 2, 3, 4)); // ?
```

**Answer \& Output:**

```
1 2 [3, 4, 5]
Alice { age: 25, city: 'Boston' }
[1, 2, 3, 4, 5, 6]
10
```

**Explanation:**

- Rest operator (`...`) collects remaining elements into array
- Spread operator (`...`) expands array/object elements
- Destructuring extracts values from arrays/objects into variables

***

## **Asynchronous JavaScript**

### **Question 8: Promise Methods**

```javascript
const promise1 = Promise.resolve(1);
const promise2 = Promise.reject('Error');
const promise3 = new Promise(resolve => setTimeout(() => resolve(3), 1000));

// Promise.all
Promise.all([promise1, promise3])
    .then(result => console.log('All resolved:', result))
    .catch(error => console.log('All error:', error));

// Promise.allSettled
Promise.allSettled([promise1, promise2, promise3])
    .then(results => console.log('All settled:', results));

// Promise.race
Promise.race([promise1, promise3])
    .then(result => console.log('Race winner:', result));
```

**Answer \& Output:**

```
Race winner: 1
All resolved: [1, 3]
All settled: [
  { status: 'fulfilled', value: 1 },
  { status: 'rejected', reason: 'Error' },
  { status: 'fulfilled', value: 3 }
]
```

**Explanation:**

- `Promise.all()`: Resolves when all promises resolve, rejects if any reject
- `Promise.allSettled()`: Waits for all promises to settle regardless of outcome
- `Promise.race()`: Resolves/rejects with first completed promise

***

### **Question 9: Async/Await**

```javascript
async function fetchData() {
    try {
        const data1 = await Promise.resolve('First data');
        console.log(data1);
        
        const data2 = await Promise.resolve('Second data');
        console.log(data2);
        
        return 'All data fetched';
    } catch (error) {
        console.log('Error:', error);
    }
}

fetchData().then(result => console.log(result));
console.log('This runs first');
```

**Answer \& Output:**

```
This runs first
First data
Second data
All data fetched
```

**Explanation:**
`async/await` makes asynchronous code look synchronous. The function returns a Promise, and `await` pauses execution until the Promise resolves.

***

## **Object-Oriented Programming**

### **Question 10: Class Inheritance and Polymorphism**

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        console.log(`${this.name} makes a sound`);
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);
        this.breed = breed;
    }
    
    speak() {
        console.log(`${this.name} barks`);
    }
    
    getInfo() {
        return `${this.name} is a ${this.breed}`;
    }
}

const animal = new Animal('Generic');
const dog = new Dog('Buddy', 'Golden Retriever');

animal.speak(); // ?
dog.speak(); // ?
console.log(dog.getInfo()); // ?
```

**Answer \& Output:**

```
Generic makes a sound
Buddy barks
Buddy is a Golden Retriever
```

**Explanation:**

- **Inheritance**: `Dog` extends `Animal` using `extends` keyword
- **Polymorphism**: `speak()` method behaves differently in child class
- **Encapsulation**: Properties and methods are bundled within classes

***

## **Advanced Object Methods**

### **Question 11: Object.freeze vs Object.seal**

```javascript
const obj1 = { name: 'Alice', age: 25 };
const obj2 = { name: 'Bob', age: 30 };

Object.freeze(obj1);
Object.seal(obj2);

// Try to modify
obj1.name = 'Changed Alice'; // Won't work
obj1.city = 'New York'; // Won't work
delete obj1.age; // Won't work

obj2.name = 'Changed Bob'; // Will work
obj2.city = 'Boston'; // Won't work
delete obj2.age; // Won't work

console.log(obj1); // ?
console.log(obj2); // ?
```

**Answer \& Output:**

```
{ name: 'Alice', age: 25 }
{ name: 'Changed Bob', age: 30 }
```

**Explanation:**

- `Object.freeze()`: Makes object completely immutable
- `Object.seal()`: Allows modification of existing properties but prevents addition/deletion

These examples cover all the core JavaScript concepts you mentioned and provide practical interview-style questions that test understanding of fundamental and advanced JavaScript features.

