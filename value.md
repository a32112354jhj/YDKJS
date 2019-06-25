# 值（Values）

## 陣列
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
* * * 
## 類陣列
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

### 字串
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
var c = a
// split `a` into an array of characters
.split( "" )
// reverse the array of characters
.reverse()
// join the array of characters back to a string
.join( "" );
c; // "oof"
```
* * *
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
* * *
### 小的十進位值
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
* * *
### 32位元整數
### 特殊值
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
確保一個運算式沒有結果值時可使用

#### 特殊數字
##### 非數字的數字
NaN (not a number)，但他的型別還是number
例如:
```js
var a = 2 / "foo"; // NaN
typeof a === "number"; // true
```
不可直接用NaN做比較，與null和undefined不同
```js
var a = 2 / "foo";
a == NaN; // false
a === NaN; // false
```

NaN的判斷方式，使用isNaN()
```js
var a = 2 / "foo";
isNaN( a ); // true
```
NaN的bug
```js
var a = 2 / "foo";
var b = "foo";
a; // NaN
b; // "foo"
window.isNaN( a ); // true
window.isNaN( b ); // true -- ouch!
```
ES6的替代方法Number.isNaN(..)
```js
if (!Number.isNaN) {
Number.isNaN = function(n) {
return (
typeof n === "number" &&
window.isNaN( n )
);
};
}
var a = 2 / "foo";
var b = "foo";
Number.isNaN( a ); // true
Number.isNaN( b ); // false -- phew!
```
***
#### 無限
```js
var a = 1 / 0; // Infinity
var b = -1 / 0; // -Infinity
```
> 只要溢位成Infinity就無法回頭了
> Infinity / Infinity 結果會是 NaN
> 一個有限的Number/Infinity 結果會是 0
 
***

#### 零
> Js中提供了一半的零與負零(-0)
 
```js
var a = 0 / -3; // -0
var b = 0 * -3; // -0
```
加減法中沒有-0的狀況，舊瀏覽器-0可能還是會被顯示為0。

`若字串化一個負0值會得到一個"0"，JSON也有同樣情況`

```js
var a = 0 / -3;
// (some browser) consoles at least get it right
a; // -0
// but the spec insists on lying to you!
a.toString(); // "0"
a + ""; // "0"
String( a ); // "0"
// strangely, even JSON gets in on the deception
JSON.stringify( a ); // "0"
```
若反過來從string轉Number
```js
+"-0"; // -0
Number( "-0" ); // -0
JSON.parse( "-0" ); // -0
```

`在比較運算子上-0與0也都視為相同`
```js
var a = 0;
var b = 0 / -3;
a == b; // true
-0 == 0; // true
a === b; // true
-0 === 0; // true
0 > -0; // false
a > b; // false
```

```js
function isNegZero(n) {
n = Number( n );
return (n === 0) && (1 / n === -Infinity);
}
isNegZero( -0 ); // true
isNegZero( 0 / -3 ); // true
isNegZero( 0 ); // false
```
***
#### 特殊相等性
ES6的`Object.is()`可以用來測試兩個值是否絕對相等
```js
var a = 2 / "foo";
var b = -3 * 0;
Object.is( a, NaN ); // true
Object.is( b, -0 ); // true
Object.is( b, 0 ); // false
```
es6之前的作法
```js
if (!Object.is) {
Object.is = function(v1, v2) {
// test for `-0`
if (v1 === 0 && v2 === 0) {
return 1 / v1 === 1 / v2;
}
// test for `NaN`
if (v1 !== v1) {
return v2 !== v2;
}
// everything else
return v1 === v2;
};
}
```

***
### 值 vs 參考

```js
var a = 2;
var b = a; //b是a值的一份拷貝
b++;
a; // 2
b; // 3
var c = [1,2,3];
var d = c; //d是指向共有值[1,2,3]的一個參考
d.push( 4 );
c; // [1,2,3,4]
d; // [1,2,3,4]
```
>* 簡單的值都是藉由值的拷貝來指定或傳遞的:`null ,
undefined , string , number , boolean , symbol(ES6)`
>* 復合值object(包括陣列)與function都會在指定或傳遞時建立參考的一份拷貝

參考指向的是值的本身，而非變數，所以無法使用一個參考來變更另一個參考的東西

```js
var a = [1,2,3];
var b = a;
a; // [1,2,3]
b; // [1,2,3]
// later
b = [4,5,6];
a; // [1,2,3]
b; // [4,5,6]
```

```js
function foo(x) {
x.push( 4 );
x; // [1,2,3,4]
// later
x = [4,5,6];
x.push( 7 );
x; // [4,5,6,7]
}
var a = [1,2,3];
foo( a );
a; // [1,2,3,4] not [4,5,6,7]
```
`沒辦法使用x變更a所指的東西，只能修改x及a公同指向的那個共有值內容。若要修改陣列裡的東西不能新建立一個陣列，而是要直接修改現有陣列的值`
```js
function foo(x) {
x.push( 4 );
x; // [1,2,3,4]
// later
x.length = 0; // empty existing array in-place
x.push( 4, 5, 6, 7 );
x; // [4,5,6,7]
}
var a = [1,2,3];
foo( a );
a; // [4,5,6,7] not [1,2,3,4]
```

想要傳入純量基型值，並讓其值可被更新(類似參考那樣)，可以將該值包裹在另一個能夠以參考的拷貝傳遞的複合值中:
```js
function foo(wrapper) {
wrapper.a = 42;
}
var obj = {
a: 2
};
foo( obj );
obj.a; // 42
```

```js
function foo(x) {
x = x + 1;
x; // 3
}
var a = 2;
var b = new Number( a ); // or equivalently `Object(a)`
foo( b );
console.log( b ); // 2, not 3
```
