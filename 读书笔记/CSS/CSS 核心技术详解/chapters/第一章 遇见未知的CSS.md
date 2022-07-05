# 第一章 遇见未知的CSS

- [第一章 遇见未知的CSS](#第一章-遇见未知的css)
  - [CSS 的一些技巧](#css-的一些技巧)
    - [1. 使用 pointer-events 控制鼠标事件](#1-使用-pointer-events-控制鼠标事件)
    - [2. 玩转选择器](#2-玩转选择器)
    - [3. 隐藏元素](#3-隐藏元素)


## CSS 的一些技巧
### 1. 使用 pointer-events 控制鼠标事件
- **场景1：验证码等待时，禁用点击操作**
> 可以给当前获取短信验证码的按钮一个点击后的禁用标志 `class="disable" `

- **`a` 标签禁止跳转**
- **给 `body` 标签添加，防止在网页下拉过程中的误触事件发生**
- **给遮盖层添加 `pointer-event: none` 后可以操作被遮盖的元素**
  
### 2. 玩转选择器
- **`:only-child` 选择器**

### 3. 隐藏元素
- **通过 `width: 0` 和 `height: 0` 来隐藏元素**
> **元素本身的内容还存在**
> **缺点：** <mark>无法隐藏文字</mark>，需要更改元素的背景色以及文字的大小来配合
```css
div {
  width: 0;
  height: 0;
  background-color: transparent;
  font-size: 0;
}
```

- **将元素的 `opacity` 设置成 `0`**
> **元素本身还存在，只是看不见**

- **通过定位将元素移出屏幕范围**
> **元素本身还存在，只是移出屏幕范围**
```css
div {
  position: absolute;
  left: -9999px;
}
``` 

- **通过 `text-indent` 实现文字隐藏的效果**
> 给页面添加LOGO图片，若想让搜索引擎搜索到，则需要添加文字，但又不想显示文字，可以使用这个方法
```css
div {
  text-indent: -9999px;
}
```

- **通过 `z-index` 隐藏一个元素**
> 需要给上面的元素添加一个背景色，不然后面遮挡的内容会透过来

- **通过给元素设置 `overflow` 来隐藏元素**
> 元素超出所设置的宽高，溢出的部分会被隐藏

- **通过 `visibility` 将元素设置为不可见**

---
<div style="display:flex;justify-content:space-between;">
  <p><a href="/读书笔记/CSS/CSS 核心技术详解/index.md">目录</a></p>
  <p><a href="#">下一章</a>/p>
</div>