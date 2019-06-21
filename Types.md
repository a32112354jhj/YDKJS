# 型別

### JavaScript中有無型別?


### 內建行別
* null
* undefined
* boolean
* number
* string
* object
* symbol(ES6)

```js
typeof undefined === "undefined"; 
typeof true === "boolean";
typeof 42 === "number"; 
typeof "42" === "string"; 
typeof { life: 42 } === "object"; 
// added in ES6!
typeof Symbol() === "symbol"; 

typeof null === "object"; 
typeof function a(){ /* .. */ } === "function";
```

如果想藉由其型別來測試一個null值，你會需要一個複合條件:

```js
var a = null;
(!a && typeof a === "object"); // true
```

#### Numbers
```js
typeof 37 === 'number';
typeof 3.14 === 'number';
typeof(42) === 'number';
typeof Math.LN2 === 'number';
typeof Infinity === 'number';
typeof NaN === 'number'; // Despite being "Not-A-Number"
typeof Number('1') === 'number'; // Number tries to parse things into numbers

typeof 42n === 'bigint';
```

#### Strings
```js
typeof '' === 'string';
typeof 'bla' === 'string';
typeof `template literal` === 'string';
typeof '1' === 'string'; // note that a number within a string is still typeof string
typeof (typeof 1) === 'string'; // typeof always returns a string
typeof String(1) === 'string'; // String converts anything into a string, safer than toString
```

#### Strings
```js
typeof '' === 'string';
typeof 'bla' === 'string';
typeof `template literal` === 'string';
typeof '1' === 'string'; // note that a number within a string is still typeof string
typeof (typeof 1) === 'string'; // typeof always returns a string
typeof String(1) === 'string'; // String converts anything into a string, safer than toString
```

#### Booleans
```js
typeof true === 'boolean';
typeof false === 'boolean';
typeof Boolean(1) === 'boolean'; // Boolean() will convert values based on if they're truthy or falsy
typeof !!(1) === 'boolean'; // two calls of the ! (logical NOT) operator are equivalent to Boolean()
```

#### Symbols(ES6)
```js
typeof Symbol() === 'symbol'
typeof Symbol('foo') === 'symbol'
typeof Symbol.iterator === 'symbol'
```

#### Undefined
```js
typeof undefined === 'undefined';
typeof declaredButUndefinedVariable === 'undefined';
typeof undeclaredVariable === 'undefined'; 
```


#### Objects
```js
typeof {a: 1} === 'object';

// use Array.isArray or Object.prototype.toString.call
// to differentiate regular objects from arrays
typeof [1, 2, 4] === 'object';

typeof new Date() === 'object';
typeof /regex/ === 'object'; // See Regular expressions section for historical results


// The following are confusing, dangerous, and wasteful. Avoid them.
typeof new Boolean(true) === 'object'; 
typeof new Number(1) === 'object'; 
typeof new String('abc') === 'object';
```
#### Functions
```js
typeof function() {} === 'function';
typeof class C {} === 'function';
typeof Math.sin === 'function';
```
###型別的值
```js
var a = 42;
typeof a; // "number"
a = true;
typeof a; // "boolean"
```
###undefined(未定義) vs "undeclared"(未宣告)
```js
var a;
typeof a;
var b = 42;
var c;
// later
b = c;
typeof b; 
typeof c; 
```

```js
var a;
a; // undefined
b; // ReferenceError: b is not defined
```

```js
var a;
typeof a; // "undefined"
typeof b; // "undefined"
```

###typeof Undeclared
```js
// oops, this would throw an error!
if (DEBUG) {
console.log( "Debugging is starting" );
}
// this is a safe existence check
if (typeof DEBUG !== "undefined") {
console.log( "Debugging is starting" );
}
```

```js
//避免使用var來宣告atob
if (typeof atob === "undefined") {
atob = function() { /*..*/ };
}
```
不使用typeof方式對全域變數做檢查
```js
if (window.DEBUG) {
// ..
}
if (!window.atob) {
// ..
}
```

```js
function doSomethingCool() {
var helper =
(typeof FeatureXYZ !== "undefined") ?
FeatureXYZ :
function() { /*.. default feature ..*/ };
var val = helper();
// ..
```
依存性注入
```js
function doSomethingCool(FeatureXYZ) {
var helper = FeatureXYZ ||
function() { /*.. default feature ..*/ };
var val = helper();
// ..
}
```
