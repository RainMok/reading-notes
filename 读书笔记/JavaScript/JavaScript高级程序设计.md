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
  // ===================================
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
//=================
console.log(name);
let name = "张三"; // 报错，未定义 ReferenceError 
```

4. 全局声明
与 var 关键字不同，使用 let 在全局作用域中声明的变量不会成为 window 对象的属性（ var 声明的变量则会）

5. 对于 let 这个新的 ES6 声明关键字，不能依赖条件声明模式

---