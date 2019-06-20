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
```

如果想藉由其型別來測試一個null值，你會需要一個複合條件:

```js
var a = null;
(!a && typeof a === "object"); // true

```




