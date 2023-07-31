# 基础

### 1. JavaScript是什么

JavaScript是运行在客户端（浏览器）的变成语言。

### 2. JavaScript组成

JavaScript由ECMAscript、DOM、BOM组成，其中ECMAscript是基础语法

# DOM获取和属性操作

### 1. Dom是什么

Dom(Document Object Model)，即文档对象模型。

### 2. 获取Dom对象

```js
// 获取第一个元素
const ul = document.querySelector(".nav")
// 获取元素集合，返回的是伪数组，有长度和索引但没有push等方法
const lis = document.querySelectorAll(".nav li")

// 不常用，最早获取Dom对象的方法，已经被抛弃使用
// 根据id获取
const id1 = document.getElementById("id1")
// 通过标签获取
const divs = document.getElementsByTagName("div")
// 通过类名获取
const cls = document.getElementsByClassName("w")
```

### 3. 操作Dom对象内容

```js
const box = document.querySelector(".box")
box.innerText = "我还好"
box.innerHTML = "<strong>我真还好</strong>"
```

### 4. 操作Dom对象属性

**操作常用属性**

```js
// 常用属性有src、href、title等
const random = Math.floor(Math.random() * 7)
const img = document.querySelector("img")
img.src = `./images/${random}.webp`
```

**操作样式属性**

- 通过style方式修改（生成的是行内，权重较高）

```js
document.body.style.backgroundImage = `url("./images/desktop_${random}.jpg")`
```

- 通过className修改

```js
const div = document.querySelector("div")
div.className = "box"
```

- 通过classList修改

```js
// 添加类
div.classList.add("border")
// 移除类
div.classList.remove("border")
// 替换类
div.classList.toggle("border")
```
