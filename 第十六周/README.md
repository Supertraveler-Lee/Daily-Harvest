* [基本语句](#基本语句)
* [数据类型](#数据类型)
* [数值](#数值)
* [字符串](#字符串)
* [对象](#对象)
* [数组](#数组)
* [函数](#函数)
## <a id="基本语句">基本语句</a>
1. **变量与标识符**
 1. 严格地说，`var a = 1` 与 `a = 1`，这两条语句的效果不完全一样，主要体现在`delete`命令无法删除前者。不过，绝大多数情况下，这种差异是可以忽略的，只要有使用了var的语句才会有变量提升。

 1. 标识符是用来识别具体对象的一个名称。最常见的标识符就是变量名，以及后面要提到的函数名。标识符命名规则如下：
      * 第一个字符，可以是任意Unicode字母（包括英文字母和其他语言的字母），以及美元符号（`$`）和下划线（`_`）。
      * 第二个字符及后面的字符，除了Unicode字母、美元符号和下划线，还可以用数字`0-9`。
     * 保留字以及`Infinity`、`NaN`、`undefined`不能作为标识符
 2. 历史上JavaScript兼容HTML代码的注释，所以`<!--`和`-->`也被视为单行注释，但是`->`只有在行首，才会被当成单行注释，否则就是一个运算符。
    ```
      x = 1; <!-- x = 2;
      --> x = 3; //只有x = 1会被执行
      ```
2. **条件语句**
 1. 多个`if...else`连在一起使用
 ```
 if (m === 0) {
   // ...
 } else if (m === 1) {
  // ...
 } else if (m === 2) {
   // ...
 } else {
   // ...
 }
 ```
 2. 多个`if...else`连在一起使用的时候，可以转为使用更方便的`switch`结构。但是`switch`语句后面的表达式与`case`语句后面的表示式，在比较运行结果时，采用的是严格相等运算符（`===`），而不是相等运算符（`==`），这意味着比较时不会发生类型转换。
 ```
switch (fruit) {
    case "banana":
    // ...
    break;
  case "apple":
    // ...
    break;
  default:
    // break不能缺少
}
 ```
 1. JavaScript语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置，标签的格式如下
 ```
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
 ```
3. **循环**
  * `for ... in`对`Array`的循环得到的是`String`而不是`Number`
## <a id="数据类型">数据类型</a>
 *   数值（number）：整数和小数（比如1和3.14）
 *   字符串（string）：字符组成的文本（比如”Hello World”）
 *   布尔值（boolean）：`true`（真）和`false`（假）两个特定值
 *   `undefined`：表示“未定义”或不存在，即此处目前没有任何值,`undefined`仅仅在判断函数参数是否传递的情况下有用
 *   `null`：表示空缺，即此处应该有一个值，但目前为空,多数情况下用`null`
 *   对象（object）：各种值组成的集合
 
 *   对象又可以分为三个子类型：
     *   狭义的对象（object）
     *   数组（array）
     *   函数（function）
 *   ES6新增Map和Set
     * Map 
     ```
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
     ```
     * Set
     ```
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}
     ```
 > 遍历`Array`可以采用下标循环，遍历`Map`和`Set`就无法使用下标。为了统一集合类型，ES6标准引入了新的`iterable`类型，`Array`、`Map`和`Set`都属于`iterable`类型。具有`iterable`类型的集合可以通过新的`for ... of`循环来遍历。
 
* `iterable`内置的`forEach`方法，它接收一个函数，每次迭代就自动回调该函数
   ```
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    alert(element);
});
//`forEach()`方法是ES5.1标准引入
   ```
 * JavaScript有三种方法，可以确定一个值到底是什么类型
     *   `typeof`运算符
     *   `instanceof`运算符
     *   `Object.prototype.toString`方法 
     *   typeof可以用来检查一个没有声明的变量，而不报错
     ```
typeof window // "object"
typeof {} // "object"
typeof [] // "object"
typeof // "object"
    ```

 除了下面六个值被转为`false`，其他值都视为`true`
*  `undefined`
*  `null`
*  `false`
*  `0`
*  `NaN`
*  `""`或`''`（空字符串）

## <a id="数值">数值</a> 
 * JavaScript内部，所有数字都是以64位浮点数形式储存
 * JavaScript浮点数的64个二进制位，从最左边开始，是这样组成的。
    *   第1位：符号位，`0`表示正数，`1`表示负数
    *   第2位到第12位：储存指数部分
    *   第13位到第64位：储存小数部分（即有效数字）
 * JavaScript对整数提供四种进制的表示方法：十进制、十六进制、八进制、2进制。
    *   十进制：没有前导0的数值。
    *   八进制：有前缀`0o`或`0O`的数值，或者有前导0、且只用到0-7的七个阿拉伯数字的数值。
    *   十六进制：有前缀`0x`或`0X`的数值。
    *   二进制：有前缀`0b`或`0B`的数值。
 * 需要注意的是，`NaN`不是一种独立的数据类型，而是一种特殊数值，它的数据类型依然属于`Number`，使用`typeof`运算符可以看得很清楚。
  ```
typeof NaN // 'number'
  ```
 * `NaN`不等于任何值，包括它本身
    ```
NaN === NaN // false
    ```
 * `isNaN`方法可以用来判断一个值是否为`NaN`(不可靠)
 * 判断`NaN`更可靠的方法是，利用`NaN`是JavaScript之中唯一不等于自身的值这个特点
 ```
function myIsNaN(value) {
  return value !== value;
}
 ```
 * `isFinite`函数返回一个布尔值，检查某个值是不是正常数值，而不是`Infinity`
 ```
isFinite(Infinity) // false
isFinite(-1) // true
isFinite(true) // true
isFinite(NaN) // false
 ```
 * `parseInt`先把参数转成字符串，再转换，且逐字转换，遇到不正确的就停止，返回已经转好的，后面能跟第二个参数【2，36】。
 * 尤其值得注意，`parseFloat`会将空字符串转为`NaN`.
 ```
parseFloat(true)  // NaN
Number(true) // 1
parseFloat(null) // NaN
Number(null) // 0
parseFloat('') // NaN
Number('') // 0
parseFloat('123.45#') // 123.45
Number('123.45#') // NaN
 ```
 
## <a id="字符串">字符串</a>
* 字符串是不可变的，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果
```
var s = 'Test';
s[0] = 'X';
alert(s); // s仍然为'Test'
```
* JavaScript为字符串提供了一些常用方法，注意，调用这些方法本身不会改变原有字符串的内容，而是返回一个新字符串：
   * toUpperCase
   * toLowerCase
   * indexOf
   * substring
   ```
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7); // 从索引7开始到结束，返回'world'
   ```
* Base64是一种编码方法，可以将任意字符转成可打印字符。使用这种编码方法，主要不是为了加密，而是为了不出现特殊字符，简化程序的处理
  *   btoa()：字符串或二进制值转为Base64编码
  *   atob()：Base64编码转为原来的编码
  *   要将非ASCII码字符转为Base64编码，必须中间插入一个转码环节，再使用这两个方法。
     ```
function b64Encode(str) {
  return btoa(encodeURIComponent(str));
}
function b64Decode(str) {
  return decodeURIComponent(atob(str));
}
b64Encode('你好') // "JUU0JUJEJUEwJUU1JUE1JUJE"
b64Decode('JUU0JUJEJUEwJUU1JUE1JUJE') // "你好"
     ```
## <a id="对象">对象</a>
* 对象引用的是内存地址，而原始数据引用的是值。
* JavaScript规定，如果行首是大括号，一律解释为语句（即代码块）。如果要解释为表达式（即对象），必须在大括号前加上圆括号
   ```
({ foo: 123})
   ```
   ```
eval('{foo: 123}') // 123
eval('({foo: 123})') // {foo: 123}
   ```
* `delete`命令只能删除对象本身的属性，无法删除继承的属性
* `delete`命令不能删除`var`命令声明的变量，只能用来删除属性
* in 运算符
   ```
// 假设变量x未定义
// 写法一：报错
if (x) { return 1; }
// 写法二：不正确
if (window.x) { return 1; }
// 写法三：正确
if ('x' in window) { return 1; }
   ```
* `in`运算符的一个问题是，它不能识别对象继承的属性
   ```
var o = new Object();
o.hasOwnProperty('toString') // false
'toString' in o // true
   ```
* `for...in`循环有两个使用注意点
  *   它遍历的是对象所有可遍历（enumerable）的属性，会跳过不可遍历的属性
  *   它不仅遍历对象自身的属性，还遍历继承的属性。
* `with`的作用是操作同一个对象的多个属性时，提供一些书写的方便
   ```
// 例一
with (o) {
  p1 = 1;
  p2 = 2;
}
// 等同于
o.p1 = 1;
o.p2 = 2;
// 例二
with (document.links[0]){
  console.log(href);
  console.log(title);
  console.log(style);
}
// 等同于
console.log(document.links[0].href);
console.log(document.links[0].title);
console.log(document.links[0].style);
   ```
* `with`区块内部的变量，必须是当前对象已经存在的属性，否则会创造一个当前作用域的全局变量,这是`with`语句的一个很大的弊病，就是绑定对象不明确。
   ```
var o = {};
with (o) {
  x = "abc";
}
o.x // undefined
x // "abc"
   ```
* `with`语句少数有用场合之一，就是替换模板变量
## <a id="数组">数组</a>
* 常用方法
   * indexOf
   * slice(对应String的`substring()`),返回一个新的`Array`
   ```
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
var aCopy = arr.slice();
aCopy; // ['A', 'B', 'C', 'D', 'E', 'F', 'G']
aCopy === arr; // false
   ```
   * `push()`向`Array`的末尾添加若干元素，`pop()`则把`Array`的最后一个元素删除掉
   * `unshift()`方法在`Array`的头部添加元素，`shift()`方法则把`Array`的第一个元素删掉：
   * sort
   * reverse
   * `splice()`是`Array`的“ 万能方法”，可从指定索引开始删除，然后再从该位置添加元素：
   ```
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
   ```
   * `concat()`合并两个`Array`返回新的`Array`
   * `join()`方法把`Array`用指定的字符串连接起来
* 本质上，数组属于一种特殊的对象。
  ```
typeof [1, 2, 3] // "object"
  ```
* JavaScript语言规定，对象的键名一律为字符串，所以，数组的键名其实也是字符串。之所以可以用数值读取，是因为非字符串的键名会被转为字符串
  ```
var arr = ['a', 'b', 'c'];
arr['0'] // 'a'
arr[0] // 'a'
  ```
* 数组中的运用in
   ```
var arr = [ 'a', 'b', 'c' ];
2 in arr  // true
'2' in arr // true
4 in arr // false
   ```
   ```
var a = [1, 2, 3];
a.foo = true;
for (var key in a) {
  console.log(key);
}
// 0
// 1
// 2
// foo
   ```
* 数组的空位
   ```
var a = [1, 2, 3,];
a.length // 3
a // [1, 2, 3]
   ```
* delete的删除，`length`属性不过滤空位。所以，使用`length`属性进行数组遍历，一定要非常小心
   ```
var a = [1, 2, 3];
delete a[1];
a[1] // undefined
a.length // 3
   ```
* 数组的某个位置是空位，与某个位置是`undefined`，是不一样的。如果是空位，使用数组的`forEach`方法、`for...in`结构、以及`Object.keys`方法进行遍历，空位都会被跳过
## <a id="函数">函数</a>
* 函数声明
   * function命令
   ```
function print(s) {
  console.log(s);
}
   ```
   * 函数表达式
   ```
var print = function(s) {
  console.log(s);
};
   ```
   * 采用函数表达式声明函数时，`function`命令后面不带有函数名。如果加上函数名，该函数名只在函数体内部有效，在函数体外部无效。这种写法的用处有两个:
      * 一是可以在函数体内部调用自身
      * 二是方便除错（除错工具显示函数调用栈时，将显示函数名，而不再显示这里是一个匿名函数）
   ```
var print = function x(){
  console.log(typeof x);
};
x
// ReferenceError: x is not defined
print()
// function
   ```
   * Function构造函数(`Function`构造函数可以不使用`new`命令，返回结果完全一样)
   ```
var add = new Function(
  'x',
  'y',
  'return x + y'
);
// 等同于
function add(x, y) {
  return x + y;
}
   ```
* 函数名提升
```
f();
var f = function (){};
// TypeError: undefined is not a function
//等同于
var f;
f();
f = function () {};
//上面代码第二行，调用`f`的时候，`f`只是被声明了，还没有被赋值，等于`undefined`，所以会报错。
//因此，如果同时采用`function`命令和赋值语句声明同一个函数，最后总是采用赋值语句的定义
```
* 不能在条件语句中声明函数,最常见的情况就是if和try语句
```
if (false) {
  function f() {}
}

f() // 不报错

//上面代码的原始意图是不声明函数`f`，但是由于`f`的提升，导致`if`语句无效，
//所以上面的代码不会报错。要达到在条件语句中定义函数的目的，只有使用函数表达式。
if (false) {
  var f = function () {};
}

f() // undefined
```
* 函数的属性和方法
   * name
   * length(预期的参数个数)，`length`属性提供了一种机制，
   判断定义时和调用时参数的差异，以便实现面向对象编程的”方法重载“（overload）
   * toString()(返回源码)
* 函数作用域

   >函数本身也是一个值，也有自己的作用域。它的作用域与变量一样，就是其**声明**时所在的作用域，与其**运行**时所在的作用域无关。总之，函数执行时所在的作用域，是定义时的作用域，而不是调用时所在的作用域。
   
```
var a = 1;
var x = function () {
  console.log(a);
};
function f() {
  var a = 2;
  x();
}
f() // 1
```
```
var x = function () {
  console.log(a);
};
function y(f) {
  var a = 2;
  f();
}
y(x)
// ReferenceError: a is not defined
```
* JavaScript有一个关键字`arguments`，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。`arguments`类似`Array`但它不是一个`Array`
   * `arguments`最常用于判断传入参数的个数
   ```
/ foo(a[, b], c)
// 接收2~3个参数，b是可选参数，如果只传2个参数，b默认为null：
function foo(a, b, c) {
    if (arguments.length === 2) {
        // 实际拿到的参数是a和b，c为undefined
        c = b; // 把b赋给c
        b = null; // b变为默认值
    }
    // ...
}
   ```
    * 数组专有的方法（比如`slice`和`forEach`），不能在arguments对象上直接使用可,通过`apply`方法，把`arguments`作为参数传进去，让`arguments`使用数组方法
    ```
// 用于apply方法
myfunction.apply(obj, arguments).
// 使用与另一个数组合并
Array.prototype.concat.apply([1,2,3], arguments)
    ```
    * 让arguments对象使用数组方法，真正的解决方法是将arguments转为真正的数组——slice方法和逐一填入新数组
    ```
var args = Array.prototype.slice.call(arguments);
// or
var args = [];
for (var i = 0; i < arguments.length; i++) {
  args.push(arguments[i]);
}
    ```
* ES6引入了`rest`,`rest`参数只能写在最后，前面用`...`标识
```
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}
foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]
foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```
* `ES6`JavaScript的变量作用域实际上是函数内部，我们在`for`循环等语句块中是无法定义具有局部作用域的变量
    * 新的关键字`let`，用`let`替代`var`可以申明一个块级作用域的变量
    ```
'use strict';
function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    i += 1; // SyntaxError
}
     ```
     * ES6新的关键字`const`来定义常量，`const`与`let`都具有块级作用域
     ```
'use strict';
const PI = 3.14;
PI = 3; // 某些浏览器不报错，但是无效果！
PI; // 3.14
    ```
* this
    * 要保证`this`指向正确，必须用`obj.xxx()`的形式调用
    * 定函数的`this`指向哪个对象，可以用函数本身的`apply`方法
       * 第一个参数就是需要绑定的`this`变量，第二个参数是`Array`，表示函数本身的参数
    ```
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};
xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
    ```
    * 与`apply()`类似的方法是`call()`
       *   `apply()`把参数打包成`Array`再传入；
       *   `call()`把参数按顺序传入。
       ```
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
//普通函数调用，我们通常把`this`绑定为`null`
       ```
       * apply装饰器？
* 为参数设置默认值
```
function f(a) {
  (a !== undefined && a !== null) ? a = a : a = 1;
  return a;
}

f() // 1
f('') // ""
f(0) // 0
```
* map()和reduce()
   ```
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(String); // ['1', '2', '3', '4', '5', '6', '7', '8', '9']
    ```
    ```
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x * 10 + y;
}); // 13579
    ```
* filter
* sort
   * `Array`的`sort()`方法默认把所有元素先转换为String再排序
   * `sort()`方法也是一个高阶函数，它还可以接收一个比较函数来实现自定义的排序
   ```
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
}); // [1, 2, 10, 20]
   ```
* 传递方式
   * 传值（原始数据）在函数体内修改参数值，不会影响到函数外部
   ```
var p = 2;
function f(p) {
  p = 3;
}
f(p);
p // 2
   ```
   * 传地址（复核类型）传入函数的原始值的地址，因此在函数内部修改参数，将会影响到原始值
   ```
var obj = {p: 1};
function f(o) {
  o.p = 2;
}
f(obj);
obj.p // 2
   ```
   > 如果函数内部修改的，不是参数对象的某个属性，而是替换掉整个参数，这时不会影响到原始值.在函数`f`内部，参数对象`obj`被整个替换成另一个值。这时不会影响到原始值。这是因为，形式参数（`o`）与实际参数`obj`存在一个赋值关系.对`o`的修改都会反映在`obj`身上。但是，如果对`o`赋予一个新的值，就等于切断了`o`与`obj`的联系，导致此后的修改都不会影响到`obj`了
   
   ```
var obj = [1, 2, 3];
function f(o){
  o = [2, 3, 4];
}
f(obj);
obj // [1, 2, 3]
   ```
* `闭包`特点
    * 记住"诞生"的环境
   