# 值（Values）

##陣列
```js
var a = [ ];
a.length; // 0
a[0] = 1;
a[1] = "2";
a[2] = [ 3 ];
a.length; // 3
```

陣列delete後不會更新length
```js
var a = [ ];
a[0] = 1;
// no `a[1]` slot set here
a[2] = [ 3 ];
a[1]; // undefined
a.length; // 3
```
array能夠鍵值/特性新增給他們但不在length範圍內

```js
var a = [ ];
a[0] = 1;
a["foobar"] = 2;
a.length; // 1
a["foobar"]; // 2
a.foobar; // 2
```

如果要被作key的string值能夠被強制轉型成number，JS會視為一個number索引來用
```js
var a = [ ];
a["13"] = 42;
a.length; // 14
```


##類陣列
slice
```js
function foo() {
var arr = Array.prototype.slice.call( arguments );
arr.push( "bam" );
console.log( arr );
}
foo( "bar", "baz" ); // ["bar","baz","bam"]
```
Array.from（ES6)
```js
...
var arr = Array.from( arguments );
...
```

###字串
```js
var a = "foo";
var b = ["f","o","o"];
```

字串與陣列相似之處
```js
a.length; // 3
b.length; // 3
a.indexOf( "o" ); // 1
b.indexOf( "o" ); // 1
var c = a.concat( "bar" ); // "foobar"
var d = b.concat( ["b","a","r"] ); // ["f","o","o","b","a","r"]
a === c; // false
b === d; // false
a; // "foo"
b; // ["f","o","o"]
```

```js
a[1] = "O";
b[1] = "O";
a; // "foo"
b; // ["f","O","o"]
```

```js

```