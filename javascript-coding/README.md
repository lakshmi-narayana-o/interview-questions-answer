# JavaScript Coding Interview Questions & Answers (Senior Developer)

Below is a mock interview set for a senior JavaScript developer role, covering fundamental and advanced topics. Each question includes an answer, example code, and an explanation.

---

## 1. **Explain Closures in JavaScript with an Example.**

**Answer:**  
A closure is a function that remembers its outer variables and can access them. Closures allow functions to have "private" variables.

**Example:**
```javascript
function makeCounter() {
  let count = 0;
  return function() {
    return ++count;
  };
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

**Explanation:**  
The inner function returned by `makeCounter` forms a closure that captures the `count` variable, allowing it to persist across calls.

---

## 2. **What are Promises? How do you use async/await?**

**Answer:**  
Promises represent the result of an asynchronous operation. `async/await` syntax allows writing asynchronous code that looks synchronous.

**Example:**
```javascript
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => resolve('data'), 1000);
  });
}

async function getData() {
  const result = await fetchData();
  console.log(result);
}

getData(); // 'data' after 1 second
```

**Explanation:**  
`fetchData` returns a Promise. `getData` uses `await` to pause until the Promise resolves.

---

## 3. **Deep vs. Shallow Copy — How do you deep clone an object?**

**Answer:**  
A shallow copy copies property references, while a deep copy duplicates all nested objects.

**Example:**
```javascript
const obj = { a: 1, b: { c: 2 } };
const shallow = { ...obj };
const deep = JSON.parse(JSON.stringify(obj));

shallow.b.c = 3;
console.log(obj.b.c); // 3 (because shallow is a reference)
console.log(deep.b.c); // 2 (deep copy is independent)
```

**Explanation:**  
Spread syntax creates a shallow copy. `JSON.parse(JSON.stringify(obj))` creates a deep copy (with limitations).

---

## 4. **What is Event Delegation?**

**Answer:**  
Event delegation is handling events at a higher-level element rather than individual child elements.

**Example:**
```javascript
document.getElementById('list').addEventListener('click', function(event) {
  if (event.target.tagName === 'LI') {
    alert(event.target.textContent);
  }
});
```
HTML:
```html
<ul id="list">
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```

**Explanation:**  
Instead of adding listeners to each `<li>`, a single listener on `<ul>` manages all clicks using event bubbling.

---

## 5. **Explain the 'this' Keyword and Arrow Functions.**

**Answer:**  
`this` refers to the invocation context. Arrow functions don't have their own `this`; they inherit from their enclosing scope.

**Example:**
```javascript
const obj = {
  value: 42,
  arrow: () => this.value,
  regular: function() { return this.value; }
};
console.log(obj.arrow());    // undefined (arrow's 'this' is global or undefined)
console.log(obj.regular());  // 42
```

**Explanation:**  
Arrow functions don't bind their own `this`, regular functions do.

---

## 6. **How do you debounce and throttle a function?**

**Answer:**  
**Debounce:** Ensures a function is only called after a certain time has elapsed since the last call.  
**Throttle:** Ensures a function is called at most once in a specified period.

**Debounce Example:**
```javascript
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

**Throttle Example:**
```javascript
function throttle(fn, limit) {
  let inThrottle;
  return (...args) => {
    if (!inThrottle) {
      fn(...args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}
```

**Explanation:**  
Debounce waits for inactivity; throttle limits execution rate.

---

## 7. **How does prototypal inheritance work?**

**Answer:**  
Objects can inherit properties from other objects via their prototype chain.

**Example:**
```javascript
function Animal(name) {
  this.name = name;
}
Animal.prototype.speak = function() {
  return `${this.name} makes a noise.`;
};

const dog = new Animal('Rex');
console.log(dog.speak()); // Rex makes a noise.
```

**Explanation:**  
`dog` inherits `speak` from `Animal.prototype`.

---

## 8. **Explain the Module Pattern in JavaScript.**

**Answer:**  
The module pattern allows encapsulation of private and public variables/methods.

**Example:**
```javascript
const Counter = (function() {
  let count = 0;
  return {
    increment() { count++; return count; },
    reset() { count = 0; }
  };
})();
```

**Explanation:**  
`count` is private; only accessible via `increment` or `reset`.

---

## 9. **What is the difference between `==` and `===`?**

**Answer:**  
`==` checks for value equality with type coercion. `===` checks for value and type equality (strict).

**Example:**
```javascript
console.log(0 == false);  // true
console.log(0 === false); // false
```

**Explanation:**  
Use `===` to avoid unexpected type coercion.

---

## 10. **How would you implement a custom `bind` function?**

**Answer:**
```javascript
Function.prototype.myBind = function(context, ...args) {
  const fn = this;
  return function(...newArgs) {
    return fn.apply(context, [...args, ...newArgs]);
  };
};

function greet(greeting, name) {
  return `${greeting}, ${name}`;
}
const sayHelloToJohn = greet.myBind(null, 'Hello', 'John');
console.log(sayHelloToJohn()); // Hello, John
```

**Explanation:**  
Custom `bind` returns a function with preset context and arguments.

---

## 11. **How do you implement memoization?**

**Answer:**
```javascript
function memoize(fn) {
  const cache = {};
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache[key]) return cache[key];
    return cache[key] = fn(...args);
  };
}

const fib = memoize(function(n) {
  if (n < 2) return n;
  return fib(n - 1) + fib(n - 2);
});
```

**Explanation:**  
Memoization caches function results to avoid repeated calculations.

---

## 12. **Explain the event loop and call stack in JavaScript.**

**Answer:**  
The call stack executes synchronous code. The event loop manages the execution of callbacks from the task queue after the stack is empty.

**Example:**
```javascript
console.log('A');
setTimeout(() => console.log('B'), 0);
console.log('C');
```
**Output:**
```
A
C
B
```

**Explanation:**  
`setTimeout` places its callback in the queue; it's executed after the synchronous code.

---

## 13. **What is Currying?**

**Answer:**  
Currying transforms a function with multiple arguments into a sequence of functions each with a single argument.

**Example:**
```javascript
function add(a) {
  return function(b) {
    return a + b;
  };
}
const add5 = add(5);
console.log(add5(3)); // 8
```

**Explanation:**  
`add(5)` returns a function waiting for the next argument.

---

## 14. **How do you detect memory leaks in JavaScript?**

**Answer:**  
- Use browser DevTools (Memory tab) to take heap snapshots.
- Look for detached DOM trees, unintentional global variables, or listeners that aren’t removed.
- Tools: Chrome DevTools, Node.js `--inspect`, `heapdump` package.

**Example (common source):**
```javascript
const elements = [];
document.getElementById('myBtn').addEventListener('click', function() {
  elements.push(new Array(1000000));  // Memory leak!
});
```

**Explanation:**  
Unused objects stuck in arrays or closures cause leaks.

---

## 15. **How do you implement Array.prototype.flat()?**

**Answer:**
```javascript
function flat(arr, depth = 1) {
  return arr.reduce((acc, val) => 
    Array.isArray(val) && depth > 0
      ? acc.concat(flat(val, depth - 1))
      : acc.concat(val), []);
}

console.log(flat([1, [2, [3, [4]]]], 2)); // [1, 2, 3, [4]]
```

**Explanation:**  
Recursively flatten arrays up to the specified depth.

---

## 16. **How does the `map` function work internally?**

**Answer:**
```javascript
Array.prototype.myMap = function(callback, thisArg) {
  const result = [];
  for (let i = 0; i < this.length; i++) {
    if (i in this) {
      result[i] = callback.call(thisArg, this[i], i, this);
    }
  }
  return result;
};
```

**Explanation:**  
Iterates over each array element and applies the callback.

---

## 17. **How do you handle errors in async/await code?**

**Answer:**
```javascript
async function fetchData() {
  try {
    const res = await fetch('url');
    const data = await res.json();
    return data;
  } catch (error) {
    console.error(error);
    // handle error
  }
}
```

**Explanation:**  
Use `try/catch` around `await` calls to handle errors.

---

## 18. **What are WeakMap and WeakSet?**

**Answer:**  
- `WeakMap` and `WeakSet` hold "weak" references to objects, allowing garbage collection if no other references exist.
- Cannot be iterated.

**Example:**
```javascript
let obj = {};
const wm = new WeakMap();
wm.set(obj, 'value');
obj = null; // 'obj' can now be garbage collected
```

**Explanation:**  
Useful for private data in classes or tracking objects without preventing GC.

---

## 19. **What is the difference between `Object.freeze()` and `const`?**

**Answer:**  
- `const`: Variable binding can't be reassigned, but object properties are still mutable.
- `Object.freeze()`: Makes object properties immutable.

**Example:**
```javascript
const obj = { a: 1 };
obj.a = 2; // allowed
Object.freeze(obj);
obj.a = 3; // ignored in strict mode
```

---

## 20. **Explain Microtasks and Macrotasks.**

**Answer:**  
- **Microtasks:** `Promise.then`, `MutationObserver` — executed after the current stack, before rendering and macrotasks.
- **Macrotasks:** `setTimeout`, `setInterval`, UI events.

**Example:**
```javascript
console.log('start');
setTimeout(() => console.log('timeout'), 0);
Promise.resolve().then(() => console.log('promise'));
console.log('end');
// Output: start, end, promise, timeout
```

**Explanation:**  
`Promise.then` (microtask) runs before `setTimeout` (macrotask) even if both are set for "immediate" execution.


## 21. **What is the difference between `call`, `apply`, and `bind`?**

**Answer:**  
- `call`: Invokes a function with a given `this` value and arguments, passed individually.
- `apply`: Invokes a function with a given `this` value and arguments, passed as an array.
- `bind`: Returns a new function with a specified `this` value and (optionally) preset arguments.

**Example:**
```javascript
function greet(greeting, name) {
  return `${greeting}, ${name}`;
}
console.log(greet.call(null, 'Hello', 'Alice'));      // Hello, Alice
console.log(greet.apply(null, ['Hi', 'Bob']));        // Hi, Bob

const sayHeyToCarol = greet.bind(null, 'Hey', 'Carol');
console.log(sayHeyToCarol());                         // Hey, Carol
```

---

## 22. **How do you polyfill `Object.assign()`?**

**Answer:**
```javascript
if (!Object.assign) {
  Object.assign = function(target, ...sources) {
    if (target == null) throw new TypeError('Cannot convert undefined or null to object');
    let to = Object(target);
    for (let i = 0; i < sources.length; i++) {
      let nextSource = sources[i];
      if (nextSource != null) {
        for (let key in nextSource) {
          if (Object.prototype.hasOwnProperty.call(nextSource, key)) {
            to[key] = nextSource[key];
          }
        }
      }
    }
    return to;
  };
}
```

**Explanation:**  
Copies enumerable own properties from source objects to a target object.

---

## 23. **Describe a scenario where arrow functions are not suitable.**

**Answer:**  
Arrow functions are not suitable when a function needs its own `this`, such as object methods or constructors.

**Example:**
```javascript
const obj = {
  count: 10,
  inc: () => { this.count++; }
};
obj.inc();
console.log(obj.count); // 10, 'this' is not obj

function Person(name) {
  this.name = name;
}
// const p = new Person => {}; // SyntaxError: arrow functions can't be used as constructors
```

---

## 24. **How can you make an object iterable?**

**Answer:**
```javascript
const range = {
  from: 1,
  to: 5,
  [Symbol.iterator]() {
    let current = this.from;
    let last = this.to;
    return {
      next() {
        if (current <= last) {
          return { done: false, value: current++ };
        }
        return { done: true };
      }
    };
  }
};

for (const num of range) {
  console.log(num); // 1 2 3 4 5
}
```

**Explanation:**  
Implement `[Symbol.iterator]` to make objects iterable using `for...of`.

---

## 25. **How do you implement a Singleton pattern in JavaScript?**

**Answer:**
```javascript
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    Singleton.instance = this;
    this.data = 42;
  }
}
const a = new Singleton();
const b = new Singleton();
console.log(a === b); // true
```

**Explanation:**  
The constructor checks if an instance already exists and returns it, ensuring only one instance.

---

## 26. **What are generators and how do you use them?**

**Answer:**  
Generators are functions that can be paused and resumed, using `function*` and `yield`.

**Example:**
```javascript
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

const g = gen();
console.log(g.next()); // { value: 1, done: false }
console.log(g.next()); // { value: 2, done: false }
console.log(g.next()); // { value: 3, done: false }
console.log(g.next()); // { value: undefined, done: true }
```

**Explanation:**  
Each `yield` pauses the generator, `next()` resumes until the next yield.

---

## 27. **How do you convert an array-like object to an array?**

**Answer:**
```javascript
function example() {
  // 'arguments' is array-like
  return Array.from(arguments); // or [...arguments]
}

console.log(example(1, 2, 3)); // [1, 2, 3]
```

**Explanation:**  
Use `Array.from()` or spread `...` to convert array-like objects to real arrays.

---

## 28. **What is the Temporal Dead Zone (TDZ)?**

**Answer:**  
TDZ is the phase between entering a block and variable declaration where `let` and `const` variables cannot be accessed.

**Example:**
```javascript
{
  // console.log(a); // ReferenceError: Cannot access 'a' before initialization
  let a = 5;
}
```

**Explanation:**  
Accessing `a` before the `let` declaration throws a ReferenceError due to TDZ.

---

## 29. **How do you use destructuring and default parameters?**

**Answer:**
```javascript
function config({host = 'localhost', port = 80} = {}) {
  return `${host}:${port}`;
}
console.log(config({host: 'example.com'})); // example.com:80
console.log(config()); // localhost:80
```

**Explanation:**  
Destructuring with default values and default parameters make function arguments flexible.

---

## 30. **How would you implement a simple LRU cache?**

**Answer:**
```javascript
class LRUCache {
  constructor(limit = 5) {
    this.cache = new Map();
    this.limit = limit;
  }
  get(key) {
    if (!this.cache.has(key)) return undefined;
    const val = this.cache.get(key);
    this.cache.delete(key);
    this.cache.set(key, val);
    return val;
  }
  set(key, val) {
    if (this.cache.has(key)) this.cache.delete(key);
    else if (this.cache.size === this.limit) this.cache.delete(this.cache.keys().next().value);
    this.cache.set(key, val);
  }
}

const lru = new LRUCache(2);
lru.set('a', 1);
lru.set('b', 2);
lru.get('a');   // Access 'a', now 'b' is LRU
lru.set('c', 3);// 'b' evicted
console.log([...lru.cache.keys()]); // ['a', 'c']
```

**Explanation:**  
Use a `Map` to keep insertion order; on access, re-insert the key; evict oldest if over limit.

---
