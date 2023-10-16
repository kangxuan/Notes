# 初识VUE

1. 想让VUE工作，必须创建一个Vue实例，且要传入一个配置对象。

2. root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法。

3. root容器里的代码被称为【Vue模板】。

4. Vue实例和容器是一一对应的。

5. 真实开发中只有一个Vue实例，并且会配合着组件一起使用。

6. {{xxx}}中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性。

7. 一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新。

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>初始vue</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <!-- 
            注意区分：js表达式 和 js代码(语句)
                    1.表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方：
                                (1). a
                                (2). a+b
                                (3). demo(1)
                                (4). x === y ? 'a' : 'b'

                    2.js代码(语句)
                                (1). if(){}
                                (2). for(){}
    -->

    <!-- 这是一个容器 -->
    <div id="demo">
        <h1>Hello, {{name.toUpperCase()}}, {{address}}</h1>
    </div>
    <script>
        // 阻止vue在启动时生成生产提示
        Vue.config.productionTip = false

        // 创建vue实例
        var app = new Vue({
            el: '#demo', // el指定vue实例为哪个容器服务
            data: {
                name: 'shanla',
                address: '杭州'
            }
        })
    </script>
</body>
```

# 模板语法

VUE分为两种模板语法，插值语法和指令语法。

1. 插值语法用于解析标签体内容。

2. 指令语法用于解析标签（包括：标签属性、标签体内容、绑定事件.....）。

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>模板语法</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <!-- 
        Vue模板语法有2大类：
            1.插值语法：
                    功能：用于解析标签体内容。
                    写法：{{xxx}}，xxx是js表达式，且可以直接读取到data中的所有属性。
            2.指令语法：
                    功能：用于解析标签（包括：标签属性、标签体内容、绑定事件.....）。
                    举例：v-bind:href="xxx" 或  简写为 :href="xxx"，xxx同样要写js表达式，
                                且可以直接读取到data中的所有属性。
                    备注：Vue中有很多的指令，且形式都是：v-????，此处我们只是拿v-bind举个例子。
    -->
    <div id="root">
        <h1>插值语法</h1>
        <h3>你好，{{name}}</h3>
        <hr>
        <h1>指令语法</h1>
        <a v-bind:href="website.url.toUpperCase()" v-bind:x="hello">点我去{{website.name}}</a>
        <!-- 缩写方式 -->
        <a :href="website.url" x="hello1">点我去{{website.name}}</a>
    </div>
    <script>
        Vue.config.productionTip = false

        new Vue({
            el: "#root",
            data: {
                name: 'shanla',
                website: {
                    name: '百度',
                    url: 'http://www.baidu.com'
                },
                hello: "你好"
            }
        })
    </script>
</body>
```

# 数据绑定

数据绑定分为单向绑定和双向绑定。

- 单向绑定(v-bind)：数据只能从data流向页面。

- 双向绑定(v-model)：数据不仅能从data流向页面，还可以从页面流向data。
  
  - 双向绑定一般都应用在表单类元素上（如：input、select等）
  
  - v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值。

```html
<body>
    <!-- 容器 -->
    <div id="root">
        <!-- 普通写法 -->
        单向数据绑定：<input type="text" v-bind:value="name"><br>
        双向数据绑定：<input type="text" v-model:value="name"><br>

        <!-- 简写 -->
        单向数据绑定：<input type="text" :value="name"><br>
        双向数据绑定：<input type="text" v-model="name"><br>
    </div>

    <script>
        Vue.config.productionTip = false

        var vm = new Vue({
            el: '#root',
            data: {
                name: 'shanla'
            }
        })
    </script>
</body>
```

# MVVM模型

M：Model，模型，对应data

V：View，视图，对应模板

VM：ViewModel，视图模型，对应vue实例

![](/Users/kx/Library/Application%20Support/marktext/images/2023-10-16-15-31-13-image.png)

# 数据代理

Js中的数据代理：通过一个对象代理对另一个对象中的属性进行操作（读/写）。

```js
let obj = {
    x: 100
}
let obj2 = {
    y: 200
}

// 通过obj2代理obj
Object.defineProperty(obj2, 'x', {
    get() {
        return obj.x
    },
    set(value) {
        obj.x = value
    }
})
```

Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）

实现原理：通过`Object.defineProperty()`把data对象中所有属性添加到vm上，为每一个添加到vm上的属性，都指定一个getter/setter，再getter/setter内部去操作（读/写）data中对应的属性。

```html
<body>
    <div id="root">
        <h2>姓名：{{name}}</h2>
        <h2>班级：{{class1}}</h2>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: '周杰伦',
                class1: '三年二班'
            }
        })
    </script>
</body>
```

# 事件处理

### 1. 事件监听

事件绑定使用`v-on:xxx`或`@xxx`，其中`xxx`是事件名，事件回调需要配置到`methods`，最终会到vm上。

```html
<body>
    <div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <!-- <button v-on:click="showInfo">点我获取信息</button> -->
        <button @click="showInfo">点我获取信息</button>
        <button @click="showInfo2($event, 66)">点我获取信息2</button>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: '清华大学'
            },
            methods: {
                showInfo(event) {
                    console.log(event.target.innerHTML)
                    alert('这是你要的信息')
                },
                showInfo2(event, number) {
                    console.log(event.target.innerHTML)
                    console.log(number)
                    alert('这是你要的信息！！')
                }
            }
        })
    </script>
</body>
```

### 2. 事件修饰符

尽管能够在事件函数方法处理`event.preventDefault()`等，但vue为了保持函数内尽量只处理业务，使用了事件修饰符来处理类似的DOM 事件细节。

```html

```
