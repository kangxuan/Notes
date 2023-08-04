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

### 5. 操作表单元素属性

```html
<body>
    <input type="text" value="电脑">
    <input class="checkbox" type="checkbox" name="" id="">
    <button>点击</button>
    <script>
        const input = document.querySelector("input")
        // 获取值
        console.log(input.value)
        // 设置表单的值
        input.value = 'js设置的值'
        // 还可以设置type的值
        input.type = 'password'

        const checkbox = document.querySelector(".checkbox")
        // 设置选中状态
        checkbox.checked = true

        const button = document.querySelector("button")
        // 设置disabled
        button.disabled = true
    </script>
</body>
```

### 6. 自定义属性

自定义属性是H5推出的的，格式为`data-自定义属性`，在标签上一律以`data-`开头，并通过`dataset`方式获取。

```html
<body>
    <input type="checkbox" data-value="1" name="robby" id="">抽烟
    <input type="checkbox" data-value="2" name="robby" id="">喝酒
    <input type="checkbox" data-value="3" name="robby" id="">烫头
    <script>
        const first = document.querySelector("input")
        console.log(first.dataset.value)
    </script>
</body>
```

# Dom事件

### 1. 事件监听

**什么是事件监听**

就是让程序监听是否有事件发生，一旦有事件发生则调用一个函数响应。比如鼠标经过显示下拉菜单，鼠标经过轮播图的点显示不同的图。

**事件监听三要素**

- 事件源

- 事件类型

- 事件调用的函数

```javascript
// 事件源.addEventListener('事件类型', 事件调用的函数)
// 选择元素
const btn = document.querySelector("button")
btn.addEventListener('click', function() {
    alert("您点击了这个按钮")
})
```

**点击事件**

```javascript
<body>
    <div class="banner">
        广告位
        <button>X</button>
    </div>
    <script>
        const btn = document.querySelector(".banner button")
        btn.addEventListener('click', function() {
            const banner = document.querySelector(".banner")
            banner.classList.add("hidden")
        })
    </script>
</body>
```

### 2. 事件类型

**鼠标事件**

- click：鼠标点击

- mouseenter：鼠标经过

- mouseleave：鼠标离开

**焦点事件**

- focus：获取焦点

- blur：失去焦点

```js
const search = document.querySelector(".search")
const resultList = document.querySelector(".result-list")
search.addEventListener('focus', function() {
    resultList.classList.remove("hidden")
})
search.addEventListener('blur', function() {
    resultList.classList.add("hidden")
})
```

css也有对应的伪类选择器

```css
input {
    width: 200px;
    transition: all .3s;
}
/* 伪类选择器可以替换focus事件 */
input:focus {
    width: 300px;
}
```

**键盘事件**

- keydown：键盘按下

- keyup：键盘抬起

```js
input.addEventListener('keydown', function () {
  console.log('键盘按下了')
})
input.addEventListener('keyup', function () {
  console.log('键盘谈起了')
})
```

**文本事件**

- input：用户输入事件

```html
<body>
    <div class="comment">
        <input type="text" placeholder="请您友善发言">
        <button>发布</button>
    </div>
    <div class="stat"><span>0</span>/50字</div>
    <script>
        const input = document.querySelector(".comment input")
        const span = document.querySelector(".stat span")
        input.addEventListener('input', function(){
            span.innerHTML = input.value.length
        })
    </script>
</body>
```

### 3. 事件对象

事件对象是存事件的相关信息的对象，比如说鼠标点击事件，事件对象存了鼠标点击的位置等等。

**如何获取事件对象**

在事件绑定的回调函数的第一个参数就是事件对象

```js
// 对象.addEventListener('input', function(e) {...})
```

**示例**

```js
// 按下回车键提交
```

# 内置函数

### 1. 间歇函数-定时器

```javascript
function fn() {
    console.log('一秒执行一次')
}
// 开启定时器，setInterval(函数, 间隔时间)，时间是毫秒
let n = setInterval(fn, 1000)
console.log(n)
// 关闭定时器
clearInterval(n)
```

**用户注册协议案例**

```html
<body>
    <textarea name="" id="" cols="30" rows="10">
        用户注册协议
        欢迎注册成为京东用户！在您注册过程中，您需要完成我们的注册流程并通过点击同意的形式在线签署以下协议，请您务必仔细阅读、充分理解协议中的条款内容后再点击同意（尤其是以粗体或下划线标识的条款，因为这些条款可能会明确您应履行的义务或对您的权利有所限制）。
        【请您注意】如果您不同意以下协议全部或任何条款约定，请您停止注册。您停止注册后将仅可以浏览我们的商品信息但无法享受我们的产品或服务。如您按照注册流程提示填写信息，阅读并点击同意上述协议且完成全部注册流程后，即表示您已充分阅读、理解并接受协议的全部内容，并表明您同意我们可以依据协议内容来处理您的个人信息，并同意我们将您的订单信息共享给为完成此订单所必须的第三方合作方（详情查看
    </textarea>
    <br>
    <button disabled>我已经阅读用户协议(5)</button>
    <script>
        // 获取button元素
        const button = document.querySelector("button")
        // 定时器
        let i = 5
        let n = setInterval(function() {
            i--
            button.innerHTML = `我已经阅读用户协议(${i})`
            if (i === 0) {
                // 关闭定时器
                clearInterval(n)
                // 设置
                button.disabled = false
                button.innerHTML = "同意"
            }
        }, 1000)
    </script>
</body>
```
