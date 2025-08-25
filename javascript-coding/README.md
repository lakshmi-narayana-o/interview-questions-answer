# JavaScript Interview Questions & Code Challenges

A comprehensive collection of JavaScript questions, code snippets, outputs, and detailed explanations for interview preparation.

## Table of Contents
1. [Array Operations](#array-operations)
2. [String Manipulation](#string-manipulation)
3. [Object Operations](#object-operations)
4. [Closures & Scope](#closures--scope)
5. [Promises & Async](#promises--async)
6. [Type Coercion & Comparisons](#type-coercion--comparisons)
7. [ES6+ Features](#es6-features)
8. [Algorithms](#algorithms)
9. [Event Loop & Timing](#event-loop--timing)
10. [Prototypes & Classes](#prototypes--classes)

***

## Array Operations

### Q1: Array Filtering with Complex Logic
```javascript
const arr1 = [3, 4, 2, 5, 1]
const arr2 = [2, 3, 4, 7, 6]
const arr3 = [1, 5, 8, 9, 10];

const combineArray = [...arr1, ...arr2, ...arr3]
const result = combineArray.filter((item) => {
    const inArr1 = arr1.includes(item)
    const inArr2 = arr2.includes(item)
    const inArr3 = arr3.includes(item)
    return (inArr1 !== inArr2 !== inArr3)
}).sort((a, b) => a - b)
console.log(result)
```

**Output:** ``

**Explanation:** 
- The filter condition `(inArr1 !== inArr2 !== inArr3)` is evaluated left to right
- It finds elements that appear in exactly one or two arrays (not all three)
- Elements like 3, 1, 5 appear in multiple arrays and are filtered out

### Q2: Find Missing Values in Range
```javascript
const findMissedValues = (...numbers) => {
    const maxNum = Math.max(...numbers)
    const minNum = Math.min(...numbers)

    const result = [];
    for (let i = minNum; i < maxNum; i++) {
        if (numbers.indexOf(i) < 0) {
            result.push(i)
        }
    }
    return result;
}

console.log(findMissedValues(1, 2, 4, 7, 13))
```

**Output:** ``

**Explanation:** Finds all missing integers between the minimum (1) and maximum (13) values in the input array.

### Q3: Array Rotation
```javascript
const roatedValues = [1, 2, 3, 4, 5, 6, 7]
const k = 2
console.log([...roatedValues.slice(k, roatedValues.length), ...roatedValues.slice(0, k)])
```

**Output:** ``

**Explanation:** Rotates array left by k positions using slice and spread operator.

### Q4: Remove Duplicates
```javascript
const words = ["hello", "worlds", "this", "worlds", "as", "a", "good", "worlds"]
console.log([...new Set(words)])
console.log(words.filter((word, index, a) => a.indexOf(word) === index))
```

**Output:** 
```
['hello', 'worlds', 'this', 'as', 'a', 'good']
['hello', 'worlds', 'this', 'as', 'a', 'good']
```

**Explanation:** Two methods to remove duplicates - Set and filter with indexOf.

### Q5: Two Sum Problem
```javascript
const twoSum = (arr, target) => {
  let result = []
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] + arr[j] === target) {
        result = [...result, [i, j]];
      }
    }
  }
  return result;
};

console.log(twoSum([2, 7, 11, 15], 9));
console.log(twoSum([3, 2, 4], 6));
```

**Output:** 
```
[[0, 1]]
[[1, 2]]
```

**Explanation:** Finds all pairs of indices where elements sum to target value.

***

## String Manipulation

### Q6: Find Shortest Word
```javascript
const sentence = "narayana is a good person"
const wordInArray = sentence.split(' ')
console.log(wordInArray.sort((a,b) => (a.length - b.length))[0])
```

**Output:** `"a"`

**Explanation:** Splits sentence into words, sorts by length, returns shortest word.

### Q7: Swap Character Pairs
```javascript
const personName = "goutham"
const result = personName.split('').reduce((acc, char, index, arr) => {
    if (index % 2 === 0) {
        acc += arr[index + 1] ? arr[index + 1] + char : char
    }
    return acc
}, "")
console.log(result)
```

**Output:** `"ogtuhma"`

**Explanation:** Swaps every pair of characters. For odd-length strings, last character remains in place.

### Q8: Find Most Repeated Character
```javascript
const findRepeatedLetter = (sentence) => {
    const [letter, count] = Object.entries(
        sentence.split('')
            .reduce((store, item) => ({ ...store, [item]: (store[item] || 0) + 1 }), {})
    ).reduce((store, array) => store[1] > array ? store : array)

    return { [letter]: count }
}

console.log(findRepeatedLetter('narayana'))
```

**Output:** `{ a: 4 }`

**Explanation:** Counts character frequency and returns the most frequent character with its count.

### Q9: Check String Rotation
```javascript
const isRotation = (str1, str2) => {
    if(str1.length !== str2.length){
        return false
    }
    return (str1 + str1).includes(str2)
}

console.log(isRotation("abcde", "cdeab"));
console.log(isRotation("abcde", "abced"));
```

**Output:** 
```
true
false
```

**Explanation:** Concatenating a string with itself contains all possible rotations.

***

## Object Operations

### Q10: Object Deep Comparison
```javascript
let obj1 = { name: 'abc', age: 30 };
let obj2 = { name: 'abc', age: 30 };

if (JSON.stringify(obj1) === JSON.stringify(obj2)) {
    console.log("The objects are equal");
} else {
    console.log("The objects are not equal");
}
```

**Output:** `"The objects are equal"`

**Explanation:** Uses JSON.stringify for deep comparison (note: property order matters).

### Q11: Array to Object Conversion
```javascript
const arr = ['a', 'b', 'c', 'd']
const result = arr.reduce((store, item) => ({...store, [item]: item}), {})
console.log(result)
```

**Output:** `{ a: 'a', b: 'b', c: 'c', d: 'd' }`

**Explanation:** Converts array elements to object keys and values using reduce.

### Q12: Group Objects by Property
```javascript
const people = [
    { name: 'ram', age: 20 },
    { name: 'ram', age: 50 },
    { name: 'sam', age: 22 },
    { name: 'kaly', age: 18 },
]

const result = people.reduce((store, person) => {
    if (!store[person.name]) {
        store[person.name] = []
    }
    store[person.name].push(person)
    return store
}, {})

console.log(result);
```

**Output:** 
```javascript
{
  ram: [{ name: 'ram', age: 20 }, { name: 'ram', age: 50 }],
  sam: [{ name: 'sam', age: 22 }],
  kaly: [{ name: 'kaly', age: 18 }]
}
```

**Explanation:** Groups array of objects by a specific property (name).

***

## Closures & Scope

### Q13: Basic Closure
```javascript
function outerFunction(outerVariable) {
    return function innerFunction(innerVariable) {
        return `${outerVariable} ${innerVariable}`
    };
}

const closureExample = outerFunction('Outer');
console.log(closureExample('Inner'));
```

**Output:** `"Outer Inner"`

**Explanation:** Inner function retains access to outer function's variables even after outer function returns.

### Q14: Memoization with Closure
```javascript
const memorizationArray = (myArray) => {
    let cache = {}
    return (index) => {
        if (index in cache) {
            console.log('fetching stored value')
            return cache[index]
        } else {
            console.log('create new value')
            return cache[index] = myArray[index]
        }
    }
}

const myArray = [10, 20, 30, 40, 50]
const result = memorizationArray(myArray)
console.log(result(0))
console.log(result(1))
console.log(result(0))
```

**Output:** 
```
create new value
10
create new value
20
fetching stored value
10
```

**Explanation:** Closure-based memoization caches previously accessed array values.

### Q15: Nested Closures
```javascript
function outer() {
  let counter = 0;
  function firstInner() {
    counter++;
    function secondInner() {
      counter += 5;
      function thirdInner() {
        counter *= 2;
        return counter;
      }
      return thirdInner;
    }
    return secondInner;
  }
  return firstInner;
}

const result = outer()()()()
console.log(result)
```

**Output:** `12`

**Explanation:** 
- counter starts at 0
- firstInner(): counter = 1
- secondInner(): counter = 6
- thirdInner(): counter = 12

***

## Promises & Async

### Q16: Promise.race
```javascript
const promise1 = new Promise(resolve => setTimeout(() => resolve("Fast promise"), 100));
const promise2 = new Promise(resolve => setTimeout(() => resolve("Slow promise"), 1000));

Promise.race([promise1, promise2])
  .then(result => console.log(result))
  .catch(error => console.error(error));
```

**Output:** `"Fast promise"`

**Explanation:** Promise.race resolves with the first promise that settles.

### Q17: Promise.all
```javascript
const promise1 = new Promise(resolve => setTimeout(() =>resolve("First promise"), 10 ));
const promise2 = new Promise(resolve => setTimeout(() =>resolve("Second promise"), 100 ));
const promise3 = new Promise(resolve => setTimeout(() =>resolve("Third promise"), 1000 ));

Promise.all([promise1, promise2, promise3])
  .then((results) => console.log(results))
  .catch((error) => console.error(error));
```

**Output:** `["First promise", "Second promise", "Third promise"]`

**Explanation:** Promise.all waits for all promises to resolve and returns array of all results.

### Q18: Async/Await Error Handling
```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    let number = 4;
    if (number % 2 !== 0) resolve("The number is even!");
    else reject("The number is odd!");
  });
}

const asyncProccess = async () => {
    try {
      const data = await fetchData()
      console.log('success: ',data)
    } catch (error) {
      console.log('failed: ', error)
    }
}

asyncProccess();
```

**Output:** `"failed: The number is odd!"`

**Explanation:** Since 4 is even, the condition fails and promise rejects.

***

## Type Coercion & Comparisons

### Q19: Equality Comparisons
```javascript
console.log('' === true) //false
console.log(0 === true) //false
console.log('' === false) //false
console.log(0 === false) //false

console.log('' == true) //false
console.log(0 == true) //false
console.log('' == false) //true
console.log(0 == false) //true
```

**Output:** As commented above

**Explanation:** 
- `===` strict equality (no type coercion)
- `==` loose equality (with type coercion)
- Empty string and 0 are falsy values

### Q20: Special Cases
```javascript
console.log(null <= 0) //true
console.log(null >= 0) //true
console.log(NaN === NaN) //false
```

**Output:** As commented above

**Explanation:** 
- `null` converts to 0 in numeric comparisons
- `NaN` is not equal to anything, including itself

### Q21: Array Comparisons
```javascript
console.log([] == ![]) //true
console.log("" == []) //true
console.log([] == 0) //true
console.log( == ) // false
```

**Output:** As commented above

**Explanation:** 
- `![]` is false (empty array is truthy, negation makes it false)
- Arrays convert to strings, then to numbers in comparisons
- Object comparison checks reference, not content

***

## ES6+ Features

### Q22: Destructuring with Default Values
```javascript
const [x, y = 5] = 
console.log(x + y)
```

**Output:** `15`

**Explanation:** x gets 10, y gets default value 5 since array has only one element.

### Q23: Rest Parameters
```javascript
const [one, two, ...other] = [1, 2, 3, 4, 5];
console.log(other)
```

**Output:** ``

**Explanation:** Rest operator collects remaining elements into an array.

### Q24: Spread Operator with Arrays
```javascript
const removeDuplication = (...arr) => {
  const uniqueArr = [];
  arr.forEach((item) => {
    if (!uniqueArr.includes(item)) {
      uniqueArr.push(item);
    }
  });
  return uniqueArr.sort((a,b) => a - b);
};

console.log(removeDuplication(1, 2, 3, 9, 4, 5, 6, 7, 3, 1, 2, 2, 1));
```

**Output:** ``

**Explanation:** Rest parameters collect all arguments, then removes duplicates and sorts.

***

## Algorithms

### Q25: Flatten Nested Array (Recursive)
```javascript
const flattenArray = (array) => {
  return array.reduce((output, item) => {
    if (Array.isArray(item)) {
      return [...output, ...flattenArray(item)];
    } else {
      return [...output, item];
    }
  }, []);
};

console.log(flattenArray([1, [2, 3], [3, 4, [5, 6], 7, 8]]));
```

**Output:** ``

**Explanation:** Recursively flattens nested arrays to any depth.

### Q26: Palindrome Check (Recursive)
```javascript
const isPalindrome = (str, left = 0, right = str.length - 1) => {
  if (left > right) return true
  if (str[left] !== str[right]) return false
  return isPalindrome(str, left + 1, right - 1)
}

console.log(isPalindrome("malayalam"))
```

**Output:** `true`

**Explanation:** Recursively compares characters from both ends moving inward.

### Q27: Create Nested Object from Dot Notation
```javascript
function createNestedObject(path, value) {
  return path.split('.').reverse().reduce((acc, key) => ({ [key]: acc }), value);
}

const result = createNestedObject("a.b.c", "someValue")
console.log(result);
```

**Output:** 
```javascript
{
  a: {
    b: {
      c: "someValue"
    }
  }
}
```

**Explanation:** Splits path by dots, reverses, and reduces to create nested structure.

***

## Event Loop & Timing

### Q28: Event Loop Priority
```javascript
console.log("Start");

setTimeout(() => console.log("setTimeout"), 0);
Promise.resolve().then(() => console.log("Promise resolved"));

console.log("End");
```

**Output:** 
```
Start
End
Promise resolved
setTimeout
```

**Explanation:** 
- Synchronous code runs first
- Microtasks (Promises) have higher priority than macrotasks (setTimeout)

### Q29: Variable Scoping in Loops
```javascript
for (var i = 0; i < 5; i++){
  setTimeout(() => console.log(i), 100)
}

for (let i = 0; i < 5; i++) {
  setTimeout(() => console.log(i), 100);
}
```

**Output:** 
```
5 5 5 5 5
0 1 2 3 4
```

**Explanation:** 
- `var` has function scope - all closures reference same variable
- `let` has block scope - each iteration creates new binding

***

## Prototypes & Classes

### Q30: Array Prototype Modification
```javascript
const dummy = []
const arr = [1, 2, 3]
Array.prototype[3] = 'surprise'
console.log(arr.length)
console.log(arr)
console.log(dummy)
delete Array.prototype
console.log(arr)
```

**Output:** 
```
3
surprise
surprise
undefined
```

**Explanation:** 
- Prototype properties are inherited by all array instances
- Array length is not affected by prototype properties
- Deleting prototype property removes it from all instances

### Q31: Class Methods and Arrow Functions
```javascript
class MyClass {
  value = 10;

  logValueRegular() {
    console.log("Regular method 'this.value':", this.value);
  }

  logValueArrow = () => {
    console.log("Arrow method 'this.value':", this.value);
  };

  setTimeoutExample() {
    setTimeout(() => {
      console.log("Arrow callback 'this.value':", this.value);
    }, 100);
  }
}

const instance = new MyClass();
instance.logValueRegular();
instance.logValueArrow();
instance.setTimeoutExample();
```

**Output:** 
```
Regular method 'this.value': 10
Arrow method 'this.value': 10
Arrow callback 'this.value': 10 (after 100ms)
```

**Explanation:** 
- Arrow functions maintain lexical `this` binding
- Regular methods have dynamic `this` binding
- Arrow functions are useful for preserving context in callbacks

***

## Advanced Concepts

### Q32: Longest Substring Without Repeating Characters
```javascript
const findLargestSubstring = (s) => {
  const seen = {}
  let start = 0
  let maxLength = 0
  let maxSubstring = ""

  for(let end = 0; end < s.length; end++){
    const char = s[end]

    if(seen[char] >= start){
      start = seen[char] + 1
    }

    seen[char] = end

    if(end - start + 1 > maxLength){
      maxLength = end - start + 1
      maxSubstring = s.substring(start, end + 1)
    }
  }
  return `${maxLength} - ${maxSubstring}`
}

console.log(findLargestSubstring("abcabcbb"));
console.log(findLargestSubstring("pwwkew"));
```

**Output:** 
```
3 - abc
3 - wke
```

**Explanation:** Uses sliding window technique to find longest substring without repeating characters.

### Q33: Function Binding Methods
```javascript
function person(data, data2) {
  return this.first + " " + this.second + " " + data + " " + data2;
}

const person1 ={
  first: 'firstname',
  second: 'secondName'
}

console.log(person.call(person1, 'sam', 'john'))
console.log(person.apply(person1, ["sam", "john"]));
const result = person.bind(person1, "sam", "john");
console.log(result());
```

**Output:** 
```
firstname secondName sam john
firstname secondName sam john
firstname secondName sam john
```

**Explanation:** 
- `call()`: invokes immediately with individual arguments
- `apply()`: invokes immediately with array of arguments
- `bind()`: returns new function with bound context

***

## Tips for Interview Success

1. **Understand the fundamentals**: Focus on closures, scope, prototypes, and async behavior
2. **Practice algorithms**: Two Sum, array flattening, string manipulation are common
3. **Know ES6+ features**: Destructuring, spread/rest, arrow functions, promises
4. **Understand event loop**: Microtasks vs macrotasks, execution order
5. **Master array/object methods**: map, filter, reduce, Object.keys, etc.
6. **Be comfortable with type coercion**: == vs ===, truthy/falsy values
7. **Practice explaining your code**: Walk through step by step

# JavaScript Interview Questions & Code Challenges (Continued)

## Advanced Mock Q&A

### Q34: Hoisting and Temporal Dead Zone
```javascript
console.log(a); // ?
console.log(b); // ?
console.log(c); // ?

var a = 1;
let b = 2;
const c = 3;

function test() {
    console.log(x); // ?
    var x = 10;
    console.log(x); // ?
}

test();
```

**Output:**
```
undefined
ReferenceError: Cannot access 'b' before initialization
ReferenceError: Cannot access 'c' before initialization
undefined
10
```

**Explanation:**
- `var` declarations are hoisted and initialized with `undefined`
- `let` and `const` are hoisted but remain in temporal dead zone until declaration
- Function-scoped `var` follows same hoisting rules within functions

### Q35: Complex Array Methods Chaining
```javascript
const students = [
    { name: 'Alice', score: 85, subject: 'Math' },
    { name: 'Bob', score: 92, subject: 'Science' },
    { name: 'Charlie', score: 78, subject: 'Math' },
    { name: 'Diana', score: 96, subject: 'Science' },
    { name: 'Eve', score: 88, subject: 'Math' }
];

const result = students
    .filter(student => student.score > 80)
    .map(student => ({ ...student, grade: student.score >= 90 ? 'A' : 'B' }))
    .reduce((acc, student) => {
        if (!acc[student.subject]) {
            acc[student.subject] = [];
        }
        acc[student.subject].push(student);
        return acc;
    }, {});

console.log(result);
```

**Output:**
```javascript
{
  Math: [
    { name: 'Alice', score: 85, subject: 'Math', grade: 'B' },
    { name: 'Eve', score: 88, subject: 'Math', grade: 'B' }
  ],
  Science: [
    { name: 'Bob', score: 92, subject: 'Science', grade: 'A' },
    { name: 'Diana', score: 96, subject: 'Science', grade: 'A' }
  ]
}
```

**Explanation:** Chains filter (score > 80), map (adds grade), and reduce (groups by subject) operations.

### Q36: Generator Functions
```javascript
function* numberGenerator() {
    let num = 1;
    while (true) {
        const reset = yield num;
        if (reset) {
            num = 1;
        } else {
            num++;
        }
    }
}

const gen = numberGenerator();
console.log(gen.next().value);      // ?
console.log(gen.next().value);      // ?
console.log(gen.next().value);      // ?
console.log(gen.next(true).value);  // ?
console.log(gen.next().value);      // ?
```

**Output:**
```
1
2
3
1
2
```

**Explanation:** Generator functions can pause execution and resume. Passing `true` to `next()` resets the counter.

### Q37: WeakMap vs Map
```javascript
let obj1 = { name: 'Alice' };
let obj2 = { name: 'Bob' };

// Map
const map = new Map();
map.set(obj1, 'userData1');
map.set(obj2, 'userData2');

// WeakMap
const weakMap = new WeakMap();
weakMap.set(obj1, 'userData1');
weakMap.set(obj2, 'userData2');

console.log(map.size);        // ?
console.log(weakMap.has(obj1)); // ?

obj1 = null; // Remove reference

console.log(map.size);        // ?
console.log(weakMap.has(obj2)); // ?

// Try to iterate
console.log([...map.keys()]);  // ?
// console.log([...weakMap.keys()]); // This would throw error
```

**Output:**
```
2
true
2
true
[{ name: 'Bob' }, null]
```

**Explanation:** 
- Map maintains strong references, preventing garbage collection
- WeakMap allows garbage collection when no other references exist
- WeakMap is not enumerable

### Q38: Prototype Chain Manipulation
```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    return `${this.name} makes a sound`;
};

function Dog(name, breed) {
    Animal.call(this, name);
    this.breed = breed;
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.speak = function() {
    return `${this.name} barks`;
};

Dog.prototype.getInfo = function() {
    return `${this.name} is a ${this.breed}`;
};

const dog = new Dog('Rex', 'German Shepherd');

console.log(dog.speak());        // ?
console.log(dog.getInfo());      // ?
console.log(dog instanceof Dog); // ?
console.log(dog instanceof Animal); // ?
```

**Output:**
```
Rex barks
Rex is a German Shepherd
true
true
```

**Explanation:** Demonstrates classical inheritance pattern with prototype chain setup and method overriding.

### Q39: Advanced Async Patterns
```javascript
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

async function processItems(items) {
    console.log('Sequential processing:');
    for (const item of items) {
        await delay(100);
        console.log(`Processed ${item}`);
    }
    
    console.log('\nParallel processing:');
    const promises = items.map(async (item) => {
        await delay(100);
        return `Processed ${item}`;
    });
    
    const results = await Promise.all(promises);
    results.forEach(result => console.log(result));
}

// This will show timing difference
const start = Date.now();
processItems(['A', 'B', 'C']).then(() => {
    console.log(`Total time: ${Date.now() - start}ms`);
});
```

**Output:**
```
Sequential processing:
Processed A
Processed B
Processed C

Parallel processing:
Processed A
Processed B
Processed C
Total time: ~600ms
```

**Explanation:** Sequential processing takes ~300ms (3 × 100ms), parallel takes ~200ms (max of concurrent 100ms operations).

### Q40: Currying and Partial Application
```javascript
// Currying
const multiply = (a) => (b) => (c) => a * b * c;
console.log(multiply(2)(3)(4)); // ?

// Partial application
const add = (a, b, c) => a + b + c;
const partialAdd = add.bind(null, 5, 10);
console.log(partialAdd(15)); // ?

// Generic curry function
const curry = (fn) => {
    return function curried(...args) {
        if (args.length >= fn.length) {
            return fn.apply(this, args);
        }
        return function(...nextArgs) {
            return curried.apply(this, args.concat(nextArgs));
        };
    };
};

const curriedAdd = curry(add);
console.log(curriedAdd(1)(2)(3)); // ?
console.log(curriedAdd(1, 2)(3)); // ?
console.log(curriedAdd(1)(2, 3)); // ?
```

**Output:**
```
24
30
6
6
6
```

**Explanation:** 
- Currying transforms function calls from `f(a,b,c)` to `f(a)(b)(c)`
- Partial application pre-fills some arguments
- Generic curry function handles various argument combinations

### Q41: Symbol and Iterators
```javascript
const myIterable = {
    data: [1, 2, 3, 4, 5],
    [Symbol.iterator]() {
        let index = 0;
        const data = this.data;
        
        return {
            next() {
                if (index < data.length) {
                    return { value: data[index++] * 2, done: false };
                }
                return { done: true };
            }
        };
    }
};

console.log([...myIterable]); // ?

for (const value of myIterable) {
    console.log(value); // ?
}

// Symbol properties
const sym1 = Symbol('description');
const sym2 = Symbol('description');

console.log(sym1 === sym2); // ?

const obj = {
    [sym1]: 'value1',
    normalProp: 'value2'
};

console.log(Object.keys(obj)); // ?
console.log(Object.getOwnPropertySymbols(obj)); // ?
```

**Output:**
```
[2, 4, 6, 8, 10]
2
4
6
8
10
false
['normalProp']
[Symbol(description)]
```

**Explanation:** 
- Custom iterator doubles each value
- Symbols are unique even with same description
- Symbol properties don't appear in `Object.keys()`

### Q42: Proxy and Reflection
```javascript
const target = {
    name: 'John',
    age: 30
};

const handler = {
    get(target, property, receiver) {
        console.log(`Getting ${property}`);
        return Reflect.get(target, property, receiver);
    },
    
    set(target, property, value, receiver) {
        console.log(`Setting ${property} to ${value}`);
        if (property === 'age' && value < 0) {
            throw new Error('Age cannot be negative');
        }
        return Reflect.set(target, property, value, receiver);
    },
    
    has(target, property) {
        console.log(`Checking if ${property} exists`);
        return Reflect.has(target, property);
    }
};

const proxy = new Proxy(target, handler);

console.log(proxy.name); // ?
proxy.age = 25;          // ?
console.log('name' in proxy); // ?

try {
    proxy.age = -5;      // ?
} catch (e) {
    console.log(e.message);
}
```

**Output:**
```
Getting name
John
Setting age to 25
Checking if name exists
true
Setting age to -5
Age cannot be negative
```

**Explanation:** Proxy intercepts and customizes operations (get, set, has, etc.) performed on objects.

### Q43: Memory Leaks and Garbage Collection
```javascript
// Memory leak example
function createLeak() {
    const largeArray = new Array(1000000).fill('data');
    
    return function() {
        // This closure holds reference to largeArray
        console.log('Function called');
        // Even though we don't use largeArray, it can't be garbage collected
    };
}

// Better approach
function createNoLeak() {
    const largeArray = new Array(1000000).fill('data');
    const result = largeArray.length; // Extract what we need
    
    return function() {
        console.log(`Array had ${result} elements`);
        // largeArray can now be garbage collected
    };
}

// Event listener leak
const button = document.createElement('button');
const handler = function() {
    console.log('Button clicked');
};

button.addEventListener('click', handler);

// To prevent leak:
// button.removeEventListener('click', handler);

// WeakRef example (newer feature)
let obj = { data: 'important' };
const weakRef = new WeakRef(obj);

console.log(weakRef.deref()); // ?

obj = null; // Remove strong reference

// At some point after garbage collection:
// console.log(weakRef.deref()); // might return undefined
```

**Output:**
```javascript
{ data: 'important' }
```

**Explanation:** 
- Closures can prevent garbage collection of large objects
- Extract only needed values from large objects in closures
- Remove event listeners to prevent memory leaks
- WeakRef allows referencing objects without preventing GC

### Q44: Advanced Promise Patterns
```javascript
// Promise timeout utility
const withTimeout = (promise, ms) => {
    return Promise.race([
        promise,
        new Promise((_, reject) => 
            setTimeout(() => reject(new Error('Timeout')), ms)
        )
    ]);
};

// Retry utility
const retry = async (fn, maxAttempts = 3) => {
    for (let attempt = 1; attempt <= maxAttempts; attempt++) {
        try {
            return await fn();
        } catch (error) {
            if (attempt === maxAttempts) throw error;
            console.log(`Attempt ${attempt} failed, retrying...`);
            await new Promise(resolve => setTimeout(resolve, 1000 * attempt));
        }
    }
};

// Batch processing
const processBatch = async (items, batchSize = 3) => {
    const results = [];
    
    for (let i = 0; i < items.length; i += batchSize) {
        const batch = items.slice(i, i + batchSize);
        const batchPromises = batch.map(item => 
            new Promise(resolve => {
                setTimeout(() => resolve(`Processed ${item}`), 100);
            })
        );
        
        const batchResults = await Promise.all(batchPromises);
        results.push(...batchResults);
        console.log(`Batch ${Math.floor(i/batchSize) + 1} completed`);
    }
    
    return results;
};

// Usage example
const failingFunction = () => {
    if (Math.random() < 0.7) throw new Error('Random failure');
    return Promise.resolve('Success!');
};

retry(failingFunction).then(console.log).catch(console.error);

processBatch([1, 2, 3, 4, 5, 6, 7]).then(results => {
    console.log('All processed:', results);
});
```

**Expected Output:**
```
Attempt 1 failed, retrying...
Attempt 2 failed, retrying...
Success!
Batch 1 completed
Batch 2 completed
Batch 3 completed
All processed: ["Processed 1", "Processed 2", "Processed 3", "Processed 4", "Processed 5", "Processed 6", "Processed 7"]
```

**Explanation:** Advanced patterns for handling timeouts, retries, and batch processing with Promises.

### Q45: Module Pattern and Namespacing
```javascript
// IIFE Module Pattern
const Calculator = (function() {
    // Private variables
    let history = [];
    let currentValue = 0;
    
    // Private methods
    function log(operation, result) {
        history.push({ operation, result, timestamp: Date.now() });
    }
    
    // Public API
    return {
        add(x) {
            currentValue += x;
            log(`+${x}`, currentValue);
            return this;
        },
        
        multiply(x) {
            currentValue *= x;
            log(`*${x}`, currentValue);
            return this;
        },
        
        getValue() {
            return currentValue;
        },
        
        getHistory() {
            return [...history]; // Return copy to prevent mutation
        },
        
        clear() {
            currentValue = 0;
            history = [];
            return this;
        }
    };
})();

console.log(Calculator.add(5).multiply(3).add(2).getValue()); // ?
console.log(Calculator.getHistory().length); // ?

// ES6 Modules simulation
const createCounter = () => {
    let count = 0;
    
    return {
        increment: () => ++count,
        decrement: () => --count,
        get value() { return count; },
        reset: () => { count = 0; }
    };
};

const counter1 = createCounter();
const counter2 = createCounter();

counter1.increment();
counter1.increment();
counter2.increment();

console.log(counter1.value); // ?
console.log(counter2.value); // ?
```

**Output:**
```
17
3
2
1
```

**Explanation:** 
- IIFE creates private scope with encapsulated state
- Method chaining returns `this` for fluent interface
- Each counter instance maintains separate state

### Q46: Performance and Optimization
```javascript
// Debounce function
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

// Throttle function
function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

// Memoization for expensive calculations
function memoize(fn) {
    const cache = new Map();
    
    return function(...args) {
        const key = JSON.stringify(args);
        
        if (cache.has(key)) {
            console.log('Cache hit');
            return cache.get(key);
        }
        
        console.log('Computing...');
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
}

// Expensive fibonacci function
const fibonacci = memoize(function(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
});

console.log(fibonacci(10)); // ?
console.log(fibonacci(10)); // ?

// Virtual scrolling simulation
function createVirtualList(items, viewportHeight, itemHeight) {
    let scrollTop = 0;
    
    return {
        getVisibleItems() {
            const startIndex = Math.floor(scrollTop / itemHeight);
            const endIndex = Math.min(
                startIndex + Math.ceil(viewportHeight / itemHeight),
                items.length - 1
            );
            
            return {
                items: items.slice(startIndex, endIndex + 1),
                startIndex,
                endIndex,
                totalHeight: items.length * itemHeight,
                offsetY: startIndex * itemHeight
            };
        },
        
        setScrollTop(newScrollTop) {
            scrollTop = Math.max(0, Math.min(newScrollTop, items.length * itemHeight - viewportHeight));
        }
    };
}

const virtualList = createVirtualList(
    Array.from({length: 10000}, (_, i) => `Item ${i}`),
    400, // viewport height
    50   // item height
);

console.log(virtualList.getVisibleItems().items.length); // ?
```

**Output:**
```
Computing...
Computing...
Computing...
... (multiple computing logs)
55
Cache hit
55
8
```

**Explanation:** 
- Memoization caches function results to avoid recomputation
- Virtual scrolling renders only visible items for performance
- Debounce/throttle control function execution frequency

# JavaScript Interview Preparation: Questions, Code Snippets, Outputs & Explanations

---

## 1. Find Missing Values in a Range

### Problem
Given a list of numbers, find all numbers missing in the sequence between the minimum and maximum value.

### Code Snippet

```js
const findMissedValues = (...numbers) => {
    const maxNum = Math.max(...numbers)
    const minNum = Math.min(...numbers)

    const result = [];
    for (let i = minNum; i < maxNum; i++) {
        if (numbers.indexOf(i) < 0) {
            result.push(i)
        }
    }
    return result;
}

console.log(findMissedValues(1, 2, 4, 7, 13));
```

### Output

```
[3, 5, 6, 8, 9, 10, 11, 12]
```

### Explanation

- `Math.max(...numbers)` and `Math.min(...numbers)` find the range.
- The loop checks each number between min and max.
- If a number is not present in the input array, it's pushed to `result`.
- The function returns all missing numbers in the sequence.

---

## 2. Sort Alphabets in a String

### Problem
Given a string, return its alphabets sorted in ascending and descending order.

### Code Snippet

```js
function sortAlphabets(str) {
  const asc = str.split("").sort((a, b) => a.localeCompare(b)).join("");
  const desc = str.split("").sort((a, b) => b.localeCompare(a)).join("");

  return {
    ascending: asc,
    descending: desc,
  };
}

// Example usage
const input = "fbacged";
const result = sortAlphabets(input);
console.log(`Input: ${input}`);
console.log(`Ascending: ${result.ascending}`);
console.log(`Descending: ${result.descending}`);
```

### Output

```
Input: fbacged
Ascending: abcdefg
Descending: gfedcba
```

### Explanation

- `str.split("")` turns the string into an array of characters.
- `.sort((a, b) => a.localeCompare(b))` sorts alphabetically (ascending).
- `.sort((a, b) => b.localeCompare(a))` sorts in reverse (descending).
- `.join("")` converts the array back to a string.

---



# JavaScript Interview: Arrays & Objects – Deduplication, Intersection, Merging

---

## 1. Remove Duplicates from an Array and Sort

### **Problem**
Given an array with duplicate numbers, return a sorted array with unique values.

### **Code Snippet**
```js
const duplicates = [3, 4, 3, 1, 2, 3, 5];
const results = duplicates
    .filter((item, index) => duplicates.indexOf(item) === index)
    .sort((a, b) => a - b);
console.log(results); // [1, 2, 3, 4, 5]
```

**Alternative:**  
You can also use a `Set`:
```js
const results = [...new Set(duplicates)].sort((a, b) => a - b);
```

### **Explanation**
- `.filter((item, index) => duplicates.indexOf(item) === index)` keeps only the first occurrence of each item.
- `.sort((a, b) => a - b)` sorts the array in ascending order.

---

## 2. Find Common Key-Value Pairs in Two Objects

### **Problem**
Given two objects, find key-value pairs that have the same key and value in both.

### **Code Snippet**
```js
const obj1 = { a: 1, b: 2, c: 3, d: 4, e: 5 };
const obj2 = { a: 4, c: 5, d: 4, e: 5, f: 6 };

const result = {};
for (let obj in obj2) {
    if (obj1[obj] === obj2[obj]) {
        result[obj] = obj1[obj];
    }
}
console.log(result); // { d: 4, e: 5 }
```

### **Explanation**
- Loop through keys in `obj2`.
- If `obj1` has the same value for the key, add it to `result`.

---

## 3. Merge Two Objects

### **Problem**
Merge two objects; values from the second object override those from the first in case of conflict.

### **Code Snippet**
```js
const obj1 = { a: 1, b: 2, c: 3, d: 4, e: 5 };
const obj2 = { a: 4, c: 5, d: 4, e: 5, f: 6 };

// Using for...in
const result = {};
for (let key in obj1) {
  result[key] = obj1[key];
}
for (let key in obj2) {
  result[key] = obj2[key];
}
console.log(result);
// { a: 4, b: 2, c: 5, d: 4, e: 5, f: 6 }

// Using Object.assign
const result2 = Object.assign({}, obj1, obj2);
console.log(result2);

// Using spread operator
const result3 = { ...obj1, ...obj2 };
console.log(result3);
```

### **Explanation**
- All methods merge keys. If a key exists in both, the value from `obj2` is used.
- `Object.assign()` and spread syntax `...` are concise, modern approaches.

---

# JavaScript Interview Preparation: Questions, Code Snippets, Outputs & Explanations

---

## 4. Separate Numbers and Alphabets in a String (Recursive Approach)

### **Problem**
Given a string containing both alphabets and digits, write a function to separate the digits and alphabets, preserving their order.

---

### **Code Snippet**

```js
const seperateNumAlpha = (s, num = "", alpha = "") => {
  if (s.length <= 0) return [num, alpha]
  
  const ch = s[0]
  if (ch >= '0' && ch <= '9') {
    return seperateNumAlpha(s.slice(1), num + ch, alpha)
  } else {
    return seperateNumAlpha(s.slice(1), num, alpha + ch)
  }
}

console.log(seperateNumAlpha('a1b2c3d4'))
```

---

### **Output**

```
[ '1234', 'abcd' ]
```

---

### **Explanation**

- The function uses recursion and accumulators for numbers (`num`) and alphabets (`alpha`).
- It checks the first character of the string (`ch`):
  - If `ch` is between `'0'` and `'9'`, it’s a digit, so it’s appended to `num`.
  - Otherwise, it’s assumed to be an alphabetic character and appended to `alpha`.
- The function recurses on the remainder of the string (`s.slice(1)`), carrying forward the accumulators.
- The base case: when the input string is empty (`s.length <= 0`), it returns an array `[num, alpha]` containing the separated numbers and alphabets in order of appearance.

---

# JavaScript Interview Preparation: Separate Numbers, Alphabets, and Special Characters from a String (Recursive)

---

## 5. Separate Numbers, Alphabets, and Special Characters from a String

### **Problem**
Write a function that takes a string and returns three strings:  
1. All numbers (in order)  
2. All alphabets (in order)  
3. All special characters (in order)  

---

### **Code Snippet**

```js
const separateAll = (s, num = "", alpha = "", special = "") => {
  if (s.length === 0) return [num, alpha, special];

  const ch = s[0];
  if (ch >= "0" && ch <= "9") {
    return separateAll(s.slice(1), num + ch, alpha, special);
  } else if ((ch >= "a" && ch <= "z") || (ch >= "A" && ch <= "Z")) {
    return separateAll(s.slice(1), num, alpha + ch, special);
  } else {
    return separateAll(s.slice(1), num, alpha, special + ch);
  }
};

console.log(separateAll("a%1b2c3@d*4"));
// Output: ["1234", "abcd", "%@*"]
```

---

### **Output**

```
[ '1234', 'abcd', '%@*' ]
```

---

### **Explanation**

- The function uses recursion and three accumulators: `num` for numbers, `alpha` for alphabets, and `special` for special characters.
- For each character in the input string:
  - If the character is a digit (`'0'` to `'9'`), it's appended to `num`.
  - If it's an alphabet (either lowercase or uppercase), it's appended to `alpha`.
  - Otherwise, it's considered a special character and appended to `special`.
- The function processes one character at a time, recursing on the remainder of the string.
- The base case is when the string is empty; the function returns the three accumulated strings as an array.

---

### **Interview Tips**
- Be ready to discuss how to modify this approach for Unicode letters, whitespace, or other categories.
- Consider edge cases: empty string, single character, only special characters, etc.
- Iterative approaches or using regex are alternatives for the same task.

---





**Example with Special Characters:**

```js
console.log(seperateNumAlpha('A1!b2@c3#')); // [ '123', 'Abc!@#' ]
```
*(In the current implementation, non-digit characters are grouped with alphabets.)*

---

# JavaScript Equality, Type Coercion, and Comparison Operators

Understanding the difference between `==` (loose equality) and `===` (strict equality), as well as how JavaScript coerces types during comparison, is crucial for interviews.

---

## 1. Strict Equality (`===`)

- Compares both **type** and **value**.
- No type conversion is done.

```js
console.log('' === true);     // false   (string vs boolean)
console.log(0 === true);      // false   (number vs boolean)
console.log('' === false);    // false   (string vs boolean)
console.log(0 === false);     // false   (number vs boolean)
```

---

## 2. Loose Equality (`==`)

- Performs **type coercion** if necessary before comparing values.

```js
console.log('' == true);      // false   ('' coerces to 0, true to 1, 0 != 1)
console.log(0 == true);       // false   (0 != 1)
console.log('' == false);     // true    ('' coerces to 0, false to 0, 0 == 0)
console.log(0 == false);      // true    (0 == 0)
```

---

## 3. Comparison Operators with `null`

```js
console.log(null <= 0);       // true
console.log(null >= 0);       // true
```

- With comparison operators (`<=`, `>=`), `null` is coerced to `0`:
    - `null <= 0` → `0 <= 0` → `true`
    - `null >= 0` → `0 >= 0` → `true`

---

## 4. Comparison Operators with Numbers

```js
console.log(0 <= 0);          // true
console.log(0 >= 0);          // true
```

- Obvious numeric comparison.

---

## 5. `NaN` Equality

```js
console.log(NaN === NaN);     // false
```
- `NaN` (Not-a-Number) is **not** equal to anything, including itself.

---

### **Summary Table**

| Expression         | Result | Explanation                                      |
|--------------------|--------|--------------------------------------------------|
| `'' === true`      | false  | Type mismatch (string vs boolean)                |
| `0 === true`       | false  | Type mismatch (number vs boolean)                |
| `'' === false`     | false  | Type mismatch                                    |
| `0 === false`      | false  | Type mismatch                                    |
| `'' == true`       | false  | `'' → 0`, `true → 1`, `0 != 1`                   |
| `0 == true`        | false  | `0 != 1`                                         |
| `'' == false`      | true   | `'' → 0`, `false → 0`, `0 == 0`                  |
| `0 == false`       | true   | `0 == 0`                                         |
| `null <= 0`        | true   | `null → 0`, `0 <= 0`                             |
| `null >= 0`        | true   | `null → 0`, `0 >= 0`                             |
| `NaN === NaN`      | false  | `NaN` is never equal to itself                   |

---

**Tip:**  
Always use `===` for comparisons unless you explicitly want type coercion, and never compare anything to `NaN` using equality operators. Use `Number.isNaN(value)` instead.

```js
console.log(Number.isNaN(NaN)); // true
```

# JavaScript Tricky Questions: Type Coercion, Primitives vs Objects, and Operators

---

## 1. Type Coercion in Arithmetic and Concatenation

```js
console.log(true + 1);     // 2
console.log(true + "1");   // "true1"
```

- `true + 1`: `true` is coerced to `1`, so `1 + 1 = 2`.
- `true + "1"`: `true` is coerced to `"true"`, `"true" + "1" = "true1"` (string concatenation).

---

## 2. Primitives Cannot Have Properties

```js
const str = "hello";
str.data = "val";
console.log(str.data); // undefined
```

- Primitive strings are **not objects**; you cannot set arbitrary properties on them.

---

## 3. String Primitives vs String Objects

```js
const s1 = "hello";
const s2 = new String("hello");
console.log(s1 == s2);      // true
console.log(s1 === s2);     // false
console.log(s1, s2);        // "hello" [String: 'hello']
```

- `==` does type coercion: compares primitive and object by value.
- `===` checks type and value: string primitive is not equal to String object.

---

## 4. Boolean Casting of Objects/Arrays

```js
console.log(Boolean({}));                 // true
console.log(Boolean([]));                 // true
console.log(Boolean(""));                 // false
console.log(Boolean(new Boolean(false))); // true
console.log(Boolean(new Boolean(true)));  // true
```

- All objects (including empty ones) are truthy.
- `new Boolean(false)` is an object, so it's truthy.

---

## 5. Weird Equality Cases

```js
console.log([] == ![]);   // true
console.log("" == []);    // true
console.log("" == ![]);   // true
console.log(!{} == []);   // true
console.log([] == 0);     // true
console.log([0] == [0]);  // false
```

**Explanations:**
- `[] == ![]`  
  - `![]` is `false`; `[] == false`; `[]` coerces to `""`; `"" == false` → `0 == 0` → `true`.
- `"" == []`  
  - `[]` coerces to `""`; `"" == ""` → `true`.
- `"" == ![]`  
  - `![]` is `false`; `"" == false` → `0 == 0` → `true`.
- `!{} == []`  
  - `!{}` is `false`; `false == []` → `0 == ""` → `0 == 0` → `true`.
- `[] == 0`  
  - `[]` coerces to `""`, then `"" == 0` → `0 == 0` → `true`.
- `[0] == [0]`  
  - Different object references; always `false`.

---

## 6. Arithmetic with Strings and Booleans

```js
let x = "5";
let y = true;
console.log(x - y); // 4
```
- `"5" - true` → `5 - 1` → `4` (subtraction triggers number coercion).

---

## 7. Logical AND (`&&`) and OR (`||`)

```js
console.log(5 && 1); // 1
console.log(5 || 1); // 5
```
- `&&` returns the last truthy value if all are truthy, otherwise the first falsy one.
- `||` returns the first truthy value.

---

## 8. `typeof` Operator

```js
console.log(typeof null);           // "object"
console.log(typeof undefined);      // "undefined"
console.log(typeof NaN);            // "number"
console.log(typeof function(){});   // "function"
```
- `typeof null` is a long-standing JS quirk.
- `typeof NaN` is `"number"` because NaN is a special numeric value.

---

## 9. Number Primitives vs Number Objects

```js
let x = 123;
let y = new Number(123);
console.log(x === y); // false
console.log(x == y);  // true
```
- `===` compares type: primitive vs object.
- `==` coerces object to primitive, then compares.

---

## 10. Unary Plus (`+`) for Type Conversion

```js
console.log(+true);    // 1
console.log(+false);   // 0
console.log(+"123");   // 123
console.log(+null);    // 0
```
- The unary plus converts its operand to a number.

---

