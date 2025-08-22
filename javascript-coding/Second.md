# JavaScript Coding Interview Questions & Answers

---

## 1️⃣ Call, Apply, and Bind – Difference + Polyfill

**Explanation:**  
- `call` invokes a function with a given `this` and arguments list.
- `apply` is like call, but takes arguments as an array.
- `bind` returns a new function with a bound `this` and (optionally) preset arguments.

```js
function greet(place) { return `Hello ${this.name} from ${place}`; }
const obj = { name: "Alice" };

greet.call(obj, "NY");     // "Hello Alice from NY"
greet.apply(obj, ["LA"]);  // "Hello Alice from LA"
const bound = greet.bind(obj, "SF");
bound(); // "Hello Alice from SF"
```

**Polyfills:**
```js
Function.prototype.myCall = function(context, ...args) {
  context = context || globalThis;
  const fn = Symbol('fn');
  context[fn] = this;
  const result = context[fn](...args);
  delete context[fn];
  return result;
};

Function.prototype.myApply = function(context, args) {
  context = context || globalThis;
  const fn = Symbol('fn');
  context[fn] = this;
  const result = context[fn](...(args || []));
  delete context[fn];
  return result;
};

Function.prototype.myBind = function(context, ...args1) {
  const fn = this;
  return function(...args2) {
    return fn.apply(context, [...args1, ...args2]);
  };
};
```

---

## 2️⃣ Flatten Array (No Array.flat)

**Input:** `[1,2,3,[4,5,6,[7,8,[10,11]]],9]`  
**Output:** `[1,2,3,4,5,6,7,8,10,11,9]`

```js
function flatten(arr) {
  return arr.reduce((acc, val) =>
    acc.concat(Array.isArray(val) ? flatten(val) : val), []);
}
console.log(flatten([1,2,3,[4,5,6,[7,8,[10,11]]],9]));
// [1,2,3,4,5,6,7,8,10,11,9]
```

---

## 3️⃣ Inline 5 divs in a row without flex/margin/padding

**Solution:**  
```html
<div style="display:inline-block;">1</div>
<div style="display:inline-block;">2</div>
<div style="display:inline-block;">3</div>
<div style="display:inline-block;">4</div>
<div style="display:inline-block;">5</div>
```

---

## 4️⃣ Find sum of numbers without for loop (reduce or recursion)

**Solution (reduce):**
```js
[1, 2, 3, 4].reduce((a,b) => a+b, 0); // 10
```
**Solution (recursion):**
```js
function sum(arr) {
  if (!arr.length) return 0;
  return arr[0] + sum(arr.slice(1));
}
sum([1,2,3,4]); // 10
```

---

## 5️⃣ Deep Copy vs Shallow Copy Example

**Shallow Copy:**
```js
const obj1 = { a: 1, b: { c: 2 } };
const obj2 = {...obj1}; // Shallow
obj2.b.c = 3;
console.log(obj1.b.c); // 3 (affected)
```

**Deep Copy:**
```js
const obj1 = { a: 1, b: { c: 2 } };
const obj2 = JSON.parse(JSON.stringify(obj1));
obj2.b.c = 3;
console.log(obj1.b.c); // 2 (not affected)
```

---

## 6️⃣ Promise & Async/Await Output

```js
async function test() {
  console.log(1);
  await Promise.resolve();
  console.log(2);
}
test(); 
console.log(3);
// Output: 1, 3, 2
```

---

## 7️⃣ Find first repeating character in a string

```js
function firstRepeatingChar(str) {
  const set = new Set();
  for (let ch of str) {
    if (set.has(ch)) return ch;
    set.add(ch);
  }
  return null;
}
firstRepeatingChar("acbbac"); // "b"
```

---

## 8️⃣ Implement a Stopwatch (Start, Stop, Reset)

```js
class Stopwatch {
  constructor() { this.startTime = 0; this.elapsed = 0; this.running = false; }
  start() {
    if (!this.running) { this.running = true; this.startTime = Date.now() - this.elapsed; }
  }
  stop() {
    if (this.running) { this.running = false; this.elapsed = Date.now() - this.startTime; }
  }
  reset() {
    this.startTime = 0; this.elapsed = 0; this.running = false;
  }
  getTime() {
    return this.running ? Date.now() - this.startTime : this.elapsed;
  }
}
```

---

## 9️⃣ Build a To-Do List (Vanilla JS Example)

```html
<input id="todo-input"><button onclick="addTodo()">Add</button>
<ul id="todo-list"></ul>
<script>
function addTodo() {
  const val = document.getElementById('todo-input').value;
  if (!val) return;
  const li = document.createElement('li');
  li.textContent = val;
  document.getElementById('todo-list').appendChild(li);
  document.getElementById('todo-input').value = '';
}
</script>
```

---

## 🔟 Currying Function for Infinite Sum – sum(10)(20)(30)()

```js
function sum(a) {
  return function(b) {
    if (b === undefined) return a;
    return sum(a + b);
  }
}
sum(10)(20)(30)(); // 60
```
Or with valueOf (for implicit conversion):

```js
function currySum(a) {
  let total = a;
  function inner(b) {
    if (b === undefined) return total;
    total += b;
    return inner;
  }
  inner.valueOf = () => total;
  inner.toString = () => String(total);
  return inner;
}
currySum(1)(2)(3)(); // 6
```

---

## 1️⃣1️⃣ Event Loop & Microtasks Output Question

```js
console.log(1);
setTimeout(()=>console.log(2));
Promise.resolve().then(()=>console.log(3));
console.log(4);
// Output: 1,4,3,2
```

---

## 1️⃣2️⃣ Polyfill for Array.map

```js
Array.prototype.myMap = function(callback, thisArg) {
  let arr = [];
  for(let i=0;i<this.length;i++) {
    arr.push(callback.call(thisArg, this[i], i, this));
  }
  return arr;
};
[1,2,3].myMap(x=>x*2); // [2,4,6]
```

---

## 1️⃣3️⃣ Unique Values from Array

```js
[1,2,2,3,4,4].filter((v,i,a)=>a.indexOf(v)===i); // [1,2,3,4]
// Or
[...new Set([1,2,2,3,4,4])]; // [1,2,3,4]
```

---

## 1️⃣4️⃣ Debounce Function + Difference with Throttle

**Debounce:** Wait for inactivity, then run.
**Throttle:** Run at most every X ms.

```js
function debounce(fn, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(()=>fn.apply(this, args), delay);
  }
}
```
**Throttle:**
```js
function throttle(fn, delay) {
  let last = 0;
  return function(...args) {
    const now = Date.now();
    if (now - last >= delay) {
      last = now;
      fn.apply(this, args);
    }
  }
}
```

---

## 1️⃣5️⃣ Implement Promise.all Polyfill

```js
Promise.myAll = function(proms) {
  return new Promise((res, rej) => {
    let out = [], count = 0;
    proms.forEach((p, i) => {
      Promise.resolve(p).then(val => {
        out[i] = val; count++;
        if (count === proms.length) res(out);
      }, rej);
    });
  });
};
```

---

## 1️⃣6️⃣ Hoisting Var vs Let Example

```js
console.log(a); // undefined
var a = 10;
console.log(b); // ReferenceError
let b = 10;
```

---

## 1️⃣7️⃣ Check Palindrome String

```js
function isPalindrome(s) {
  return s === s.split('').reverse().join('');
}
isPalindrome('madam'); // true
```

---

## 1️⃣8️⃣ Memoization Example

```js
function memoize(fn) {
  const cache = {};
  return function(...args) {
    const key = JSON.stringify(args);
    if (!(key in cache)) cache[key] = fn.apply(this, args);
    return cache[key];
  };
}
const fib = memoize(n => n<2 ? n : fib(n-1)+fib(n-2));
fib(40); // Fast
```

---

## 1️⃣9️⃣ Implement call/apply/bind Polyfills
See #1 above.

---

## 2️⃣0️⃣ Convert Object to Query String

```js
function toQuery(obj) {
  return Object.entries(obj)
    .map(([k,v])=>`${encodeURIComponent(k)}=${encodeURIComponent(v)}`)
    .join('&');
}
toQuery({a:1, b:'hi'}); // "a=1&b=hi"
```

---

## 2️⃣1️⃣ Array Chunking

```js
function chunk(arr, size) {
  let out = [];
  for (let i=0;i<arr.length;i+=size)
    out.push(arr.slice(i, i+size));
  return out;
}
chunk([1,2,3,4,5], 2); // [[1,2],[3,4],[5]]
```

---

## 2️⃣2️⃣ Flatten Object Keys (dot notation)

```js
function flatten(obj, prefix='') {
  return Object.keys(obj).reduce((acc,k)=>{
    const pre = prefix.length ? prefix+'.'+k : k;
    if (typeof obj[k]=='object' && obj[k]!==null && !Array.isArray(obj[k])) 
      Object.assign(acc, flatten(obj[k], pre));
    else acc[pre]=obj[k];
    return acc;
  },{});
}
flatten({a:1, b:{c:2,d:3}}); // {a:1, 'b.c':2, 'b.d':3}
```

---

## 2️⃣3️⃣ Difference Between == vs ===

- `==` checks value after type coercion (loose equality).
- `===` checks value and type (strict equality).

```js
0 == '0'    // true
0 === '0'   // false
```

---

## 2️⃣4️⃣ Implement once Function

```js
function once(fn) {
  let called = false, res;
  return function(...args) {
    if (!called) { called=true; res=fn.apply(this,args); }
    return res;
  }
}
```

---

## 2️⃣5️⃣ Reverse Words in Sentence

```js
function reverseWords(str) {
  return str.split(' ').reverse().join(' ');
}
reverseWords('hello world now'); // 'now world hello'
```

---

## 2️⃣6️⃣ Deep Freeze Object

```js
function deepFreeze(obj) {
  Object.getOwnPropertyNames(obj).forEach(prop=>{
    if(typeof obj[prop]=='object' && obj[prop]!==null)
      deepFreeze(obj[prop]);
  });
  return Object.freeze(obj);
}
```

---

## 2️⃣7️⃣ Event Delegation Example

```html
<ul id="list">
  <li>1</li><li>2</li>
</ul>
<script>
list.onclick = e => {
  if (e.target.tagName === 'LI') alert(e.target.textContent);
};
</script>
```

---

## 2️⃣8️⃣ Closure Output Question

```js
for(var i=0;i<3;i++){
  setTimeout(()=>console.log(i),0);
}
// 3,3,3
for(let i=0;i<3;i++){
  setTimeout(()=>console.log(i),0);
}
// 0,1,2
```

---

## 2️⃣9️⃣ Convert Array to Object

```js
['a','b','c'].reduce((acc,v,i)=>{acc[i]=v;return acc;},{});
// {0:'a',1:'b',2:'c'}
```

---

## 3️⃣0️⃣ Polyfill for Array.filter

```js
Array.prototype.myFilter = function(cb, thisArg) {
  let arr = [];
  for(let i=0;i<this.length;i++)
    if(cb.call(thisArg, this[i], i, this)) arr.push(this[i]);
  return arr;
};
[1,2,3].myFilter(x=>x>1); // [2,3]
```

---

## 3️⃣1️⃣ Output Question (Hoisting + TDZ)

```js
{
  console.log(a); // undefined
  var a = 1;
  //console.log(b); // ReferenceError
  let b = 2;
}
```

---

## 3️⃣2️⃣ Polyfill for Array.reduce

```js
Array.prototype.myReduce = function(cb, init) {
  let acc = init===undefined ? this[0] : init;
  let i = init===undefined ? 1 : 0;
  for(;i<this.length;i++) acc = cb(acc, this[i], i, this);
  return acc;
}
[1,2,3].myReduce((a,b)=>a+b); // 6
```

---

## 3️⃣3️⃣ this Binding in setTimeout

```js
const obj = {
  val: 42,
  method: function() {
    setTimeout(function() {
      console.log(this.val); // undefined (window/global)
    }, 0);
    setTimeout(() => {
      console.log(this.val); // 42 (arrow binds lexical this)
    }, 0);
  }
};
obj.method();
```

---

## 3️⃣4️⃣ Implement groupBy Function

```js
function groupBy(arr, fn) {
  return arr.reduce((acc, v) => {
    const key = fn(v);
    (acc[key]=acc[key]||[]).push(v);
    return acc;
  }, {});
}
groupBy([1,2,3,4], x=>x%2); // {0:[2,4],1:[1,3]}
```

---

## 3️⃣5️⃣ Closure in Loop Output

```js
for(var i=1;i<=3;i++){
  setTimeout(()=>console.log(i),i*100); // 4,4,4
}
for(let i=1;i<=3;i++){
  setTimeout(()=>console.log(i),i*100); // 1,2,3
}
```

---

## 3️⃣6️⃣ Shuffle Array

```js
function shuffle(arr) {
  for(let i=arr.length-1;i>0;i--){
    let j = Math.floor(Math.random()*(i+1));
    [arr[i],arr[j]]=[arr[j],arr[i]];
  }
  return arr;
}
```

---

## 3️⃣7️⃣ Polyfill for Array.from

```js
Array.myFrom = function(iterable, mapFn, thisArg) {
  let arr = [];
  for (let i=0;i<iterable.length;i++)
    arr.push(mapFn ? mapFn.call(thisArg, iterable[i], i) : iterable[i]);
  return arr;
}
```

---

## 3️⃣8️⃣ Object Keys with non-primitive keys

**Explanation:**  
Only strings/symbols can be object keys. Objects used as keys are coerced to `"[object Object]"`.

```js
const a = {}, b = {};
const obj = {};
obj[a] = 1;
obj[b] = 2;
console.log(obj); // { '[object Object]': 2 }
```

---

## 3️⃣9️⃣ Throttle Function + Difference with Debounce

See #14 above.

---

## 4️⃣0️⃣ Convert Query String to Object

```js
function toObj(str) {
  return str.split('&').reduce((acc, pair) => {
    let [k, v] = pair.split('=');
    acc[decodeURIComponent(k)] = decodeURIComponent(v);
    return acc;
  }, {});
}
toObj('a=1&b=hi'); // {a:'1',b:'hi'}
```

---

## 4️⃣1️⃣ Async/Await vs Promise Execution Order

```js
console.log(1);
async function f() {
  console.log(2);
  await null;
  console.log(3);
}
f();
Promise.resolve().then(()=>console.log(4));
console.log(5);
// Output: 1,2,5,4,3
```

---

## 4️⃣2️⃣ Polyfill for Promise.race

```js
Promise.myRace = function(iter){
  return new Promise((res,rej)=>{
    for(let p of iter)
      Promise.resolve(p).then(res, rej);
  });
}
```

---

## 4️⃣3️⃣ Default Parameters TDZ Question

```js
// console.log(x); // ReferenceError
let x = 1;
function foo(a=x) { return a; }
foo(); // 1
```

---

## 4️⃣4️⃣ Infinite Currying with Multiplication

```js
function mul(a) {
  return function(b) {
    if (b === undefined) return a;
    return mul(a * b);
  }
}
mul(2)(3)(4)(); // 24
```

---

## 4️⃣5️⃣ Implement deepEqual Function

```js
function deepEqual(a, b) {
  if (a === b) return true;
  if (typeof a != 'object' || typeof b != 'object' || a==null || b==null) return false;
  let keysA = Object.keys(a), keysB = Object.keys(b);
  if (keysA.length !== keysB.length) return false;
  for (let k of keysA)
    if (!deepEqual(a[k], b[k])) return false;
  return true;
}
```

---

## 4️⃣6️⃣ Set with NaN/undefined/null size question

```js
const s = new Set([NaN, NaN, undefined, null, null]);
console.log(s.size); // 3 (NaN, undefined, null)
```

---

## 4️⃣7️⃣ Event Emitter (Pub/Sub)

```js
class Emitter {
  constructor() { this.events = {}; }
  on(e, fn) { (this.events[e]=this.events[e]||[]).push(fn); }
  off(e, fn) { this.events[e] = (this.events[e]||[]).filter(f=>f!==fn); }
  emit(e, ...args) { (this.events[e]||[]).forEach(fn=>fn(...args)); }
}
```

---

## 4️⃣8️⃣ Object.freeze vs seal

- **freeze**: No add/delete/change properties.
- **seal**: Can't add/delete, but can change existing values.

---

## 4️⃣9️⃣ Compose vs Pipe Functions

```js
const compose = (...fns) => x => fns.reduceRight((v,f)=>f(v), x);
const pipe = (...fns) => x => fns.reduce((v,f)=>f(v), x);
```

---

## 5️⃣0️⃣ Arguments Object Mutation

```js
function foo(a) { arguments[0]=99; return a; }
foo(1); // 99 (non-strict); 1 (strict mode)
```

---

## 5️⃣1️⃣ IIFE & Closure Output

```js
var x = 1;
(function() {
  var x = 2;
  (function() {
    console.log(x); // 2
  })();
})();
```

---

## 5️⃣2️⃣ Polyfill for Array.flatMap

```js
Array.prototype.myFlatMap = function(cb, thisArg) {
  return this.reduce((a, v, i) => a.concat(cb.call(thisArg, v, i, this)), []);
};
```

---

## 5️⃣3️⃣ Object Destructuring Defaults

```js
const {a=1, b=2} = {a:3};
console.log(a, b); // 3 2
```

---

## 5️⃣4️⃣ Clone DOM Node with Events

```js
function cloneWithEvents(node) {
  const clone = node.cloneNode(true);
  // Events are not cloned by default; need to reattach manually
  // Can wrap addEventListener to record, then re-apply on clone
  return clone;
}
```

---

## 5️⃣5️⃣ Function length property with defaults

```js
function f(a,b=1,c) {}
console.log(f.length); // 1 (only before first default)
```

---

## 5️⃣6️⃣ Polyfill for Promise.allSettled

```js
Promise.myAllSettled = function(proms) {
  return Promise.all(proms.map(p =>
    Promise.resolve(p).then(
      v => ({status:'fulfilled', value:v}),
      e => ({status:'rejected', reason:e})
    )
  ));
};
```

---

## 5️⃣7️⃣ Implicit Coercion Outputs ({}+[] etc)

```js
console.log({} + []); // "[object Object]"
console.log([] + {}); // "[object Object]"
console.log([] + []); // ""
console.log({} + {}); // "[object Object][object Object]"
```

---

## 5️⃣8️⃣ Retry Function Implementation

```js
function retry(fn, times) {
  return (...args) => new Promise((res, rej) => {
    function attempt(n) {
      fn(...args).then(res, err => n>1 ? attempt(n-1) : rej(err));
    }
    attempt(times);
  });
}
```

---

## 5️⃣9️⃣ Delete Operator Behavior

```js
const obj = { a: 1 };
delete obj.a; // true; obj = {}
const arr = [1,2];
delete arr[0]; // true; arr=[,2]
```

---

## 6️⃣0️⃣ Lazy Function with Currying

```js
function lazyAdd(a) {
  return function(b) {
    if (b === undefined) return a;
    return lazyAdd(a + b);
  }
}
lazyAdd(1)(2)(3)(); // 6
```

---

## 6️⃣1️⃣ Async/Await with setTimeout Execution Order

```js
async function f() {
  await 1;
  setTimeout(()=>console.log(2),0);
  console.log(3);
}
f();
console.log(4);
// Output: 4,3,2
```

---

## 6️⃣2️⃣ Polyfill Function.prototype.defer

```js
Function.prototype.defer = function(ms) {
  setTimeout(this, ms);
};
function f() { alert("hi"); }
f.defer(1000); // alerts after 1s
```

---

## 6️⃣3️⃣ Spread vs Object.assign Shallow Copy

```js
let o = {a:1}, o2={...o}, o3=Object.assign({},o);
```

---

## 6️⃣4️⃣ Polyfill Array.some

```js
Array.prototype.mySome = function(cb, thisArg) {
  for(let i=0;i<this.length;i++)
    if(cb.call(thisArg, this[i], i, this)) return true;
  return false;
};
```

---

## 6️⃣5️⃣ Optional chaining + Nullish Coalescing Example

```js
const obj = { a: { b: 2 } };
console.log(obj?.a?.b ?? 42); // 2
console.log(obj?.x?.y ?? 42); // 42
```

---

## 6️⃣6️⃣ LRU Cache Implementation

```js
class LRUCache {
  constructor(cap) { this.cap=cap; this.map=new Map(); }
  get(k) {
    if(!this.map.has(k)) return -1;
    let v=this.map.get(k);
    this.map.delete(k); this.map.set(k,v);
    return v;
  }
  put(k,v) {
    if(this.map.has(k)) this.map.delete(k);
    this.map.set(k,v);
    if(this.map.size>this.cap) this.map.delete(this.map.keys().next().value);
  }
}
```

---

## 6️⃣7️⃣ Array Holes Behavior

```js
[,,,].length // 3
[1,,3].map(x=>1) // [1, , 1]
```

---

## 6️⃣8️⃣ Debounce with Immediate Option

```js
function debounce(fn, wait, immediate) {
  let timeout;
  return function(...args) {
    const callNow = immediate && !timeout;
    clearTimeout(timeout);
    timeout = setTimeout(()=>{timeout=null; if(!immediate) fn.apply(this,args);}, wait);
    if(callNow) fn.apply(this,args);
  }
}
```

---

## 6️⃣9️⃣ Class Field vs Prototype property

```js
class A {
  x = 1; // Class field, per-instance
  method() {} // Prototype
}
```

---

## 7️⃣0️⃣ Polyfill for new operator

```js
function myNew(fn, ...args) {
  const obj = Object.create(fn.prototype);
  const res = fn.apply(obj, args);
  return res && typeof res=="object" ? res : obj;
}
```

---

## 7️⃣1️⃣ Promise Chain with Error Handling Output

```js
Promise.resolve().then(()=>{throw 'err'}).catch(e=>console.log(e)).then(()=>console.log('done'));
// Output: 'err', 'done'
```

---

## 7️⃣2️⃣ Polyfill Array.every

```js
Array.prototype.myEvery = function(cb, thisArg) {
  for(let i=0;i<this.length;i++)
    if(!cb.call(thisArg, this[i], i, this)) return false;
  return true;
}
```

---

## 7️⃣3️⃣ Function Overloading Simulation

```js
function fn() {
  if (arguments.length === 1) { /* ... */ }
  if (arguments.length === 2) { /* ... */ }
}
```

---

## 7️⃣4️⃣ JSON.stringify (basic) Implementation

```js
function stringify(obj) {
  if(obj===null) return 'null';
  if(typeof obj==='string') return `"${obj}"`;
  if(typeof obj==='number'||typeof obj==='boolean') return String(obj);
  if(Array.isArray(obj)) return `[${obj.map(stringify).join(',')}]`;
  if(typeof obj==='object') return `{${Object.keys(obj).map(k=>`"${k}":${stringify(obj[k])}`).join(',')}}`;
}
```

---

## 7️⃣5️⃣ Prototype vs Instance property Output

```js
function A() {}
A.prototype.x = 1;
const a = new A();
a.x = 2;
console.log(a.x); // 2
console.log(a.__proto__.x); // 1
```

---

## 7️⃣6️⃣ Polyfill for instanceof

```js
function myInstanceof(obj, fn) {
  let proto = Object.getPrototypeOf(obj);
  while(proto) {
    if(proto === fn.prototype) return true;
    proto = Object.getPrototypeOf(proto);
  }
  return false;
}
```

---

## 7️⃣7️⃣ Symbol Keys in Objects

```js
const s = Symbol('a');
const o = {[s]:1};
console.log(Object.keys(o)); // []
console.log(Object.getOwnPropertySymbols(o)); // [Symbol(a)]
```

---

## 7️⃣8️⃣ Scheduler Implementation

```js
class Scheduler {
  constructor(limit=2) { this.limit=limit; this.running=0; this.queue=[]; }
  add(fn) {
    return new Promise(res=>{
      const task = ()=>fn().then(res).finally(()=>{this.running--;this.next();});
      this.queue.push(task); this.next();
    });
  }
  next() {
    if(this.running>=this.limit||!this.queue.length) return;
    this.running++; this.queue.shift()();
  }
}
```

---

## 7️⃣9️⃣ Tagged Template Literals Behavior

```js
function tag(strings, ...values) {
  return strings[0] + values.map((v,i)=>v+strings[i+1]).join('');
}
tag`a${1}b${2}c` // "a1b2c"
```

---

## 8️⃣0️⃣ Polyfill Object.create

```js
function myCreate(proto) {
  function F() {}
  F.prototype = proto;
  return new F();
}
```

---

## 8️⃣1️⃣ Async/Await with Multiple Awaits Order

```js
async function f() {
  console.log(1);
  await null;
  console.log(2);
  await null;
  console.log(3);
}
f();
console.log(4);
// Output: 1,4,2,3
```

---

## 8️⃣2️⃣ Polyfill setInterval using setTimeout

```js
function mySetInterval(fn, delay) {
  let timer;
  function tick() {
    fn();
    timer = setTimeout(tick, delay);
  }
  tick();
  return { clear: () => clearTimeout(timer) };
}
```

---

## 8️⃣3️⃣ Array sort lexicographic behavior

```js
[1, 2, 10, 5].sort(); // [1,10,2,5]
```

---

## 8️⃣4️⃣ Async Pipe Function

```js
const asyncPipe = (...fns) => x => fns.reduce((p, f) => p.then(f), Promise.resolve(x));
```

---

## 8️⃣5️⃣ typeof null vs instanceof Object

```js
typeof null // 'object'
null instanceof Object // false
```

---

## 8️⃣6️⃣ Virtual DOM Diff (shallow)

```js
function diff(a, b) {
  const changes = {};
  for (let k in b) if (a[k] !== b[k]) changes[k] = [a[k], b[k]];
  return changes;
}
```

---

## 8️⃣7️⃣ Eval Scope Example

```js
var a = 1;
(function(){
  var a = 2;
  eval('console.log(a)');
})(); // 2
```

---

## 8️⃣8️⃣ Priority Queue Implementation

```js
class PQ {
  constructor() { this.q=[]; }
  insert(v,p) { this.q.push({v,p}); this.q.sort((a,b)=>a.p-b.p); }
  extractMin() { return this.q.shift().v; }
}
```

---

## 8️⃣9️⃣ Async Constructor Example

```js
class Foo {
  constructor() {
    return (async () => {
      this.val = await Promise.resolve(42);
      return this;
    })();
  }
}
(async () => { const f = await new Foo(); console.log(f.val); })();
```

---

## 9️⃣0️⃣ Safe get(obj, path) like Lodash

```js
function get(obj, path, def) {
  return path.split('.').reduce((o,k) => (o||{})[k], obj) ?? def;
}
get({a:{b:2}},'a.b'); // 2
```

---

## 9️⃣1️⃣ Destructuring with Rest Output

```js
const [a, ...rest] = [1,2,3];
console.log(a, rest); // 1 [2,3]
```

---

## 9️⃣2️⃣ Polyfill for Promise.any

```js
Promise.myAny = function(proms) {
  let errs = [], n = 0;
  return new Promise((res, rej) => {
    proms.forEach((p,i) => Promise.resolve(p).then(res, e => {
      errs[i]=e; n++;
      if (n===proms.length) rej(new AggregateError(errs));
    }));
  });
};
```

---

## 9️⃣3️⃣ Async/Await Error Handling with try/catch

```js
async function f() {
  try { await Promise.reject('err'); }
  catch(e) { console.log('caught', e); }
}
```

---

## 9️⃣4️⃣ Deep Clone with Circular References

```js
function deepClone(obj, map=new WeakMap()) {
  if(obj==null||typeof obj!=='object') return obj;
  if(map.has(obj)) return map.get(obj);
  const res = Array.isArray(obj)?[]:{};
  map.set(obj,res);
  for(let k in obj) res[k]=deepClone(obj[k], map);
  return res;
}
```

---

## 9️⃣5️⃣ Object.defineProperty writable false Example

```js
const o = {};
Object.defineProperty(o, 'x', { value: 1, writable: false });
o.x = 2;
console.log(o.x); // 1
```

---

## 9️⃣6️⃣ Task Runner with Concurrency Limit

See #78 Scheduler.

---

## 9️⃣7️⃣ Tricky Assignment (a.x = a = {...})

```js
var a = {x:1};
a.x = a = {y:2};
console.log(a); // {y:2}
```

---

## 9️⃣8️⃣ Polyfill Array.isArray

```js
Array.myIsArray = function(a) {
  return Object.prototype.toString.call(a)==='[object Array]';
}
```

---

## 9️⃣9️⃣ with Statement Output

```js
var a = 1;
with ({a:2}) { console.log(a); } // 2
```

---

## 🔟0️⃣0️⃣ Observable Implementation

```js
class Observable {
  constructor(sub) { this._sub = sub; }
  subscribe(obs) { return this._sub(obs); }
}
const obs = new Observable(obs => {
  obs.next(1); obs.next(2); obs.complete();
});
obs.subscribe({ next: x=>console.log(x), complete: ()=>console.log('done') });
```

---

# End of Interview Q&A


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

---

# End of Mock Interview Set


---

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

