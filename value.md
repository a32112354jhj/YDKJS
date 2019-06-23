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
字串'借取'不會變動內容的array方式
```js
a.join; // undefined
a.map; // undefined
var c = Array.prototype.join.call( a, "-" );
var d = Array.prototype.map.call( a, function(v){
return v.toUpperCase() + ".";
} ).join( "" );
c; // "f-o-o"
d; // "F.O.O."
```

反轉(陣列有一個在原處變動內容的reverse方法但字串沒有),借取的方式不適用於此
```js
a.reverse; // undefined
b.reverse(); // ["!","o","O","f"]
b; // ["!","o","O","f"]
```
另一種解決方式（將字串轉成陣列，操作完在回程字串）
```JS
var c = a
// split `a` into an array of characters
.split( "" )
// reverse the array of characters
.reverse()
// join the array of characters back to a string
.join( "" );
c; // "oof"
```
### 數字
非常大或非常小的number會以指數形式輸出，與toExponential()方法的輸出格式相同
```js
var a = 5E10;
a; // 50000000000
a.toExponential(); // "5e+10"
var b = a * a;
b; // 2.5e+21
var c = 1 / a;
c; // 2e-11
```

toFixed()方法可以指定用幾個小數位數來表示
```js
var a = 42.59;
a.toFixed( 0 ); // "43"
a.toFixed( 1 ); // "42.6"
a.toFixed( 2 ); // "42.59"
a.toFixed( 3 ); // "42.590"
a.toFixed( 4 ); // "42.5900"
```
toPrecision（）顯示幾個有效數字
```js
var a = 42.59;
a.toPrecision( 1 ); // "4e+1"
a.toPrecision( 2 ); // "43"
a.toPrecision( 3 ); // "42.6"
a.toPrecision( 4 ); // "42.59"
a.toPrecision( 5 ); // "42.590"
a.toPrecision( 6 ); // "42.5900"
```
注意
```js
// invalid syntax:
42.toFixed( 3 ); // SyntaxError
// these are all valid:
(42).toFixed( 3 ); // "42.000"
0.42.toFixed( 3 ); // "0.420"
42..toFixed( 3 ); // "42.000"
```

8進位與16進位
```js
0xf3; // hexadecimal for: 243
0Xf3; // ditto
0363; // octal for: 243（es6不允許）

//es6允許的方式
0o363; // octal for: 243
0O363; // ditto
0b11110011; // binary for: 243
0B11110011; // ditto
```
###小的十進位值
```js
0.1 + 0.2 === 0.3; // false
```

可使用Number.EPSILON來比較兩個number
```js
function numbersCloseEnoughToEqual(n1,n2) {
return Math.abs( n1 - n2 ) < Number.EPSILON;
}
var a = 0.1 + 0.2;
var b = 0.3;
numbersCloseEnoughToEqual( a, b ); // true
numbersCloseEnoughToEqual( 0.0000001, 0.0000002 ); // false
```

### 安全的整數範圍
（es6）
Number.MAX_SAFE_INTEGER
Number.MIN_SAFE_INTEGER

### 測試整數
```js
Number.isInteger( 42 ); // true
Number.isInteger( 42.000 ); // true
Number.isInteger( 42.3 ); // false
```
es6之前
```js
if (!Number.isInteger) {
Number.isInteger = function(num) {
return typeof num == "number" && num % 1 == 0;
};
}
```

測試安全整數
```js
Number.isSafeInteger( Number.MAX_SAFE_INTEGER ); // true
Number.isSafeInteger( Math.pow( 2, 53 ) ); // false
Number.isSafeInteger( Math.pow( 2, 53 ) - 1 ); // true
```
es6之前
```js
if (!Number.isSafeInteger) {
Number.isSafeInteger = function(num) {
return Number.isInteger( num ) &&
Math.abs( num ) <= Number.MAX_SAFE_INTEGER;
};
}
```

### 32位元整數
###特殊值
#### 非值的值
>* null
>* undefined

可以指定一個值給這個全域的undefined（非常不建議這樣做）

#### void
會讓任何值變成undefined，他不會現存的值，他只會讓這個運算子不會回傳任何值
```js
var a = 42;
console.log( void a, a ); // undefined 42
```
