# JavaScript高级程序设计
![](http://md.raizo.club/cover.png)
- **第四版**
- **作者：Matt Frisbie**
---
- [JavaScript高级程序设计](#javascript高级程序设计)
  - [- **作者：Matt Frisbie**](#--作者matt-frisbie)
  - [Script 标签](#script-标签)
    - [1. async](#1-async)
    - [2. integrity](#2-integrity)
    - [3. type](#3-type)
  - [noscript 标签](#noscript-标签)
  - [动态加载脚本](#动态加载脚本)
    - [利用DOM Api 实现加载外部指定脚本](#利用dom-api-实现加载外部指定脚本)
  - [严格模式](#严格模式)
  - [语法](#语法)
  - [变量](#变量)
    - [1. var 关键字](#1-var-关键字)
    - [2. let 关键字](#2-let-关键字)
    - [3. const 关键字](#3-const-关键字)
  - [数据类型](#数据类型)
    - [1. typeof 操作符](#1-typeof-操作符)
    - [2. Undefinde 类型](#2-undefinde-类型)
    - [3. Null 类型](#3-null-类型)
    - [Boolean 类型](#boolean-类型)
    - [Number 类型](#number-类型)
    - [String 类型](#string-类型)
---

## Script 标签

### 1. async
<mark>可选</mark>。表示应该立即开始下载脚本，但不能阻止其他页面动作，比如下载资源或等待其他脚本加载。只对外部脚本文件有效

### 2. integrity 
<mark>可选</mark>。允许比对接收到的资源和指定的加密签名以验证子资源完整性（SRI，Subresource Integrity）。**如果接收到的资源的签名与这个属性指定的签名不匹配，则页面会报错，脚本不会执行**。这个属性可以用于确保内容分发网络（CDN，Content Delivery Network）不会提供恶意内容。

### 3. type
如果这个值是 **<mark>module</mark>**，则代码会被当成 `ES6` 模块，而且只有这时候代码中才能出现`import`和`export`关键字。

---

## noscript 标签
>`noscript` 标签里的内容在脚本不可用时显示，可用则永远不会看到其内容。
```html
<noscript>
  <p>脚本不可用时才显示这段话</p>
</noscript>
```

---

## 动态加载脚本

### 利用DOM Api 实现加载外部指定脚本
- 除了`<script>`标签，还有其他方式可以加载脚本。因为 JavaScript 可以使用 DOM API，所以通过向 DOM 中动态添加script元素同样可以加载指定的脚本。只要创建一个script元素并将其添加到DOM 即可。
```javascript
// 异步加载外部脚本
let script = document.createElement('script'); 
script.src = 'gibberish.js'; 
document.head.appendChild(script);
```
**<mark>注意</mark>**：<u>所有浏览器都支持`createElement()`方法，但不是所有浏览器都支持`async`属性</u>。因此，如果要统一动态脚本的加载行为，可以明确将其**设置为同步加载**`(script.async = false)`。
```javascript
// 同步加载外部脚本
let script = document.createElement('script'); 
script.src = 'gibberish.js'; 
script.async = false; // 设置为同步加载
document.head.appendChild(script);
```
>`以这种方式获取的资源对浏览器预加载器是不可见的`。这会严重影响它们在资源获取队列中的优先级。这种方式可能会`严重影响性能`。要想让预加载器知道这些动态请求文件的存在，**可以在文档头部显式声明它们**。
```html
<link rel="preload" href="gibberish.js">
```
---

## 严格模式
1. 在文件开头，使整个文件都采用严格模式
```javascript
use strict;
```
2. 在函数内部使用，单独地使函数内部开启严格模式
```javascript
function doSomething(){
  use strict;
}
```

---

## 语法
1. 推荐代码使用分号（;）结尾，在压缩代码的时候不会出现错误。
2. 遇见if使用单个条件语句的时候，进来采用“{}”，而不推荐使用C风格的写法
```javascript
// 不推荐
if (true) console.log(true);
// 推荐
if (true){
  console.log(true);
}
```

---

## 变量
### 1. var 关键字 
>声明的范围是 **<mark>函数作用域</mark>**
1. `var` 定义变量的时候，自动被赋值 `undefined`
2. 定义多个变量时，可以用`逗号，`隔开
  ```javascript
   var name = "张三",
        age = 20,
   nickname = "法外狂徒";   
  ```
3. 声明作用域
```javascript
  function test(){
    var message = "test";
  }
  test();
  console.log(message); // 报错，找不到变量message，因为message只在函数内的作用域有效
  
  function test(){
    message = "test";
  }
  test();
  console.log(message) // “test” ，不带var的变量message的作用域被从函数内部提升到了全局作用域。
```
4. 声明提升
```javascript
function test(){
  console.log(age);
  var age = 20;
}
test(); // 不会报错，因为函数内部等价于

function test(){
  var age = 20;
  console.log(age);
}
// 自动将var 后面的变量声明提到了所在作用域的最前面。
var age 20;
var age 21;
var age 22;
这样反复操作也是没有问题的。
```

### 2. let 关键字
>声明的范围是 **<mark>块级作用域</mark>**
1. 块级作用域
```javascript
if (true){
  var message = "test";
  console.log(message); // 输出 “test”
}
console.log(message); // 输出 “test”
// 换成 let
if (true){
  let message = "test"
  console.log(message); // 输出 “test”
}
console.log(message); // ReferenceError 异常 找不到变量message
```
`let` 的作用域只限于if内部

2. 不可以重复定义
```javascript
let test = 1;
let test = 2;   // 这样是错误的
```

3. 暂时性锁区
```javascript
console.log(name);
var name = "张三"; // 不会报错，name 被 var 提升到了作用域的最前面

console.log(name);
let name = "张三"; // 报错，未定义 ReferenceError 
```

4. 全局声明
与 var 关键字不同，使用 `let` 在全局作用域中声明的变量不会成为 `window` 对象的属性（ `var` 声明的变量则会）

5. 对于 `let` 这个新的 ES6 声明关键字，不能依赖条件声明模式
6. `for`循环中的 `let` 声明
```javascript
// for循环中的var变量声明
for (var i = 0; i < 5;  i++){
  setTimeout(() => {
    console.log(i);// 输出结果为  “5，5，5，5，5”
  }, 0);
}

// for循环中的let变量声明
for(let i = 5; i < 5; i++){
  setTimeout(() => {
    console.log(i); // 输出结果为 “0，1，2，3，4”
  }, 0);
} 
```
>之所以会这样，是因为在退出循环时，迭代变量保存的是导致循环退出的值：5。在之后执行超时逻辑时，所有的 i 都是同一个变量，因而输出的都是同一个最终值。
>而在使用 let 声明迭代变量时，JavaScript 引擎在后台会为每个迭代循环声明一个新的迭代变量。每个 setTimeout 引用的都是不同的变量实例，所以 console.log 输出的是我们期望的值，也就是循环执行过程中每个迭代变量的值

### 3. const 关键字

1. 初始化即复制，不可更改；复制对象时，对象元素除外。
   
>`const` 的行为与 `let` 基本相同，唯一一个重要的区别是用它声明变量时必须同时初始化变量，且尝试修改 `const` 声明的变量会导致运行时错误。
>声明的限制只适用于它指向的变量的引用，**<mark>如果 const 变量引用的是一个`对象`，那么修改这个对象内部的属性并不违反 const 的限制</mark>**。 

2. `const` 不可用于 `for` 循环内定义使用 (迭代变量会自增)

---

## 数据类型
|数据类型|||||||
|---|---|---|---|---|---|---|
|简单数据类型|`Undefined`|`Null`|`Boolean`|`Number`|`String`|`Symbol`(符号)|
|复杂数据类型|`Object`||||||

### 1. typeof 操作符
|`undefined`|`boolean`|`string`|`number`|`object`|`function`|`symbol`|
|---|---|---|---|---|---|---|
>**注意**：严格来讲，<u>函数在 ECMAScript 中被认为是对象</u>，并不代表一种数据类型。可是，**函数也有自己特殊的属性**。为此，就有必要通过 `typeof` 操作符来区分函数和其他对象。

### 2. Undefinde 类型

变量在声明的时候并未初始化时，会被隐式地赋值 `undefined` ，无需显示赋值。**目的是**：<u>明确空指针对象与未赋值变量的区别</u>

1. 对于使用`typeof` 对 `只声明未定义的变量` 或 `未定义的变量` 的返回值都是 `undefined`
2. `undefined` 是个假值。


### 3. Null 类型

逻辑上表示空对象指针，所以使用 `typeof` 时，返回 `Object`。
1. 定义 **<mark>将来要保存对象值</mark>** 的变量时，建议使用 `null` 来初始化
```javascript
if (car != null) {
  // car 是一个对象的引用
}
```

2. `undefined值` 是由 `null值` 派生而来的
```javascript
console.log(null == undefined); // true
```
>即使 `null` 和 `undefined` 有关系，**它们的用途也是完全不一样的**。如前所述，永远不必显式地将变量值设置为 `undefined` 。但 `null` 不是这样的。**任何时候，只要变量要保存对象，而当时又没有那个对象可保存，就要用 `null` 来填充该变量**。<mark>这样就可以保持 `null` 是空对象指针的语义</mark>，并进一步将其与 `undefined` 区分开来。

3. `null` 是个假值

### Boolean 类型
1. 值为 `true` 和 `false`
2. 区分大小写：Ture 和 False 是有效的标识符但不是 `布尔值`
3. 可用其他类型的值转换 
```javascript
let message = "Hello world!";
let messageAsBoolean = Boolean(message);
```

### Number 类型
1. 表示整数和浮点值（在某些语言中也叫双精度值）
```javascript
let intNum = 55; // 整数

let octalNum1 = 070; // 八进制的 56
let octalNum2 = 079; // 无效的八进制值，当成 79 处理
let octalNum3 = 08; // 无效的八进制值，当成 8 处理

let hexNum1 = 0xA; // 十六进制 10
let hexNum2 = 0x1f; // 十六进制 31
```
- **<mark>使用八进制和十六进制格式创建的数值在所有数学操作中都被视为十进制数值</mark>**
- `正零` 和 `负零` 在所有情况下都被认为是等同的

2. 浮点值
```javascript
let floatNum1 = 1.1;
let floatNum2 = 0.1;
let floatNum3 = .1; // 有效，但不推荐
```

- 小数点后面没有数字或只是0，会当做整数处理
```javascript
let floatNum1 = 1.; // 小数点后面没有数字，当成整数 1 处理
let floatNum2 = 10.0; // 小数点后面是零，当成整数 10 处理
```

- 数值过大或过小，用 `科学计数法` 更整洁
```javascript
let floatNumOne = 3.125e7; // 等于 31250000
let floatNumTwo =  3e-17; // 等于  0.000 000 000 000 000 03
```

- 值的范围
`Number.MIN_VALUE` 保存最小值 （多数浏览器为5e-324）
`Number.MAX_VALUE` 保存最大值 （多数浏览器为1.797 693 134 862 315 7e+308）
`Infinity ` 无穷大，超出最大值的表示
`-Infinity ` 无穷小，超出最小值的表示
要确定一个值是不是有限大（即介于 JavaScript 能表示的最小值和最大值之间），可以使用`isFinite()` 函数
```javascript
let result = Number.MAX_VALUE + Number.MAX_VALUE;
console.log(isFinite(result)); // false
```

3. `NaN`
意思是“不是数值”（Not a Number），用于 **表示本来要返回数值的操作失败了**（而不是抛出错误）
```javascript
console.log(0/0); // NaN
console.log(-0/+0); // NaN
console.log(5/0); // Infinity
console.log(5/-0); // -Infinity
```

- 任何涉及 NaN 的操作始终返回 NaN （如 NaN/10）
- NaN 不等于包括 NaN 在内的任何值
```javascript
console.log(NaN == NaN); // false
```

- `isNaN()`  函数，接受任意数据类型，判断这个参数是否“不是数值” 该函数会尝试把它转换为数值
```javascript
console.log(isNaN(NaN)); // true
console.log(isNaN(10)); // false，10 是数值
console.log(isNaN("10")); // false，可以转换为数值 10
console.log(isNaN("blue")); // true，不可以转换为数值
console.log(isNaN(true)); // false，可以转换为数值 1
```

4. 数值转换
- `Number()` 转化规则
_1. 布尔值：`true` 转化为 1，`false` 转化为 0_
_2. 数值，直接返回_
_3. `null` ，返回 0_
_4. `undefined` ，返回 NaN_
_5. 字符串应用以下规则：_
_&nbsp; .1  Number("1") 返回 1， Number("123") 返回 123， Number("011") 返回 11（忽略前面的零）_
_&nbsp; .2  "1.1" ，则会转换为相应的浮点值（同样，忽略前面的零）_
_&nbsp; .3 字符串包含有效的十六进制格式如 "0xf" ，则会转换为与该十六进制值对应的十进制整数值_
_&nbsp; .4 空字符串（不包含字符），则返回 0_
_&nbsp; .5 上述情况之外的其他字符，则返回 NaN_
_6. 对象，调用 valueOf() 方法，并按照上述规则转换返回的值。如果转换结果是 NaN ，则调用toString() 方法，再按照转换字符串的规则转换_
```javascript
let num1 = Number("Hello world!"); // NaN
let num2 = Number(""); // 0
let num3 = Number("000011"); // 11
let num4 = Number(true); // 1
```

- `parseInt()`
从第一个非空格字符开始转换。如果第一个字符不是数值字符、加号或减号， parseInt() 立即
返回 NaN 。这意味着空字符串也会返回 NaN （这一点跟 Number() 不一样，它返回 0）<br/><br/>如果第一个字符是数值字符、加号或减号，则继续依次检测每个字符，直到字符串末尾，或碰到非数值字符。

```javascript
let num1 = parseInt("1234blue"); // 1234
let num2 = parseInt(""); // NaN
let num3 = parseInt("0xA"); // 10，解释为十六进制整数
let num4 = parseInt(22.5); // 22
let num5 = parseInt("70"); // 70，解释为十进制值
let num6 = parseInt("0xf"); // 15，解释为十六进制整数
```

&emsp;`parseInt()` 也接收第二个参数，用于指定底数（进制数）
```javascript
let num = parseInt("0xAF", 16); // 175
// 事实上，如果提供了十六进制参数，那么字符串前面的 "0x" 可以省掉：
let num1 = parseInt("AF", 16); // 175
let num2 = parseInt("AF"); // NaN

// 通过第二个参数，可以极大扩展转换后获得的结果类型。比如：
let num1 = parseInt("10", 2); // 2，按二进制解析
let num2 = parseInt("10", 8); // 8，按八进制解析
let num3 = parseInt("10", 10); // 10，按十进制解析
let num4 = parseInt("10", 16); // 16，按十六进制解析
```

- `parseFloat()`
工作方式跟 `parseInt()` 函数类似解析到字符串末尾或者解析到一个无效的浮点数值字符为止。这意味着**第一次出现的小数点是有效的**，但第二次出现的小数点就无效了<br/>`parseFloat()` 函数的另一个不同之处在于，它始终忽略字符串开头的零。**十六进制数值始终会返回 0**

```javascript
let num1 = parseFloat("1234blue"); // 1234，按整数解析
let num2 = parseFloat("0xA"); // 0
let num3 = parseFloat("22.5"); // 22.5
let num4 = parseFloat("22.34.5"); // 22.34
let num5 = parseFloat("0908.5"); // 908.5
let num6 = parseFloat("3.125e7"); // 31250000
```

### String 类型


---
