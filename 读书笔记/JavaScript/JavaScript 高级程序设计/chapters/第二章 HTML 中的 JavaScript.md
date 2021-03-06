# 第二章 HTML 中的 JavaScript

- [第二章 HTML 中的 JavaScript](#第二章-html-中的-javascript)
  - [Script 标签](#script-标签)
    - [一. async](#一-async)
    - [二. integrity](#二-integrity)
    - [三. type](#三-type)
  - [noscript 标签](#noscript-标签)
  - [动态加载脚本](#动态加载脚本)
    - [利用DOM Api 实现加载外部指定脚本](#利用dom-api-实现加载外部指定脚本)


## Script 标签

### 一. async
<mark>可选</mark>。表示应该立即开始下载脚本，但不能阻止其他页面动作，比如下载资源或等待其他脚本加载。只对外部脚本文件有效

### 二. integrity 
<mark>可选</mark>。允许比对接收到的资源和指定的加密签名以验证子资源完整性（SRI，Subresource Integrity）。**如果接收到的资源的签名与这个属性指定的签名不匹配，则页面会报错，脚本不会执行**。这个属性可以用于确保内容分发网络（CDN，Content Delivery Network）不会提供恶意内容。

### 三. type
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
- **<mark>注意</mark>**：<u>所有浏览器都支持`createElement()`方法，但不是所有浏览器都支持`async`属性</u>。因此，如果要统一动态脚本的加载行为，可以明确将其**设置为同步加载**`(script.async = false)`。
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

<div style="display:flex; justify-content:space-between;">
    <p><a href="/读书笔记/JavaScript/JavaScript 高级程序设计/index.md">目录</a></p>
    <p><a href="/读书笔记/JavaScript/JavaScript 高级程序设计/chapters/第三章 语言基础.md">下一章</a></p>
</div>