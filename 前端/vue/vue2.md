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

数据绑定分为单向绑定和双向绑定

```html

```
