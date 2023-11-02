# 安装

### 1. 初学者直接用`script`引入

开发时引入`vue.js`包含完整警告和调试模式，上线后使用`vue.min.js`删除了警告。

```html
<!-- 引入开发版 -->
<script type="text/javascript" src="../js/vue.js"></script>
<!-- 引入生产版本 -->
<script type="text/javascript" src="../js/vue.js"></script>
```

引入开发版时，调试会进行提示，可以进行规避提醒。

![image](/Users/kx/workspace/Notes/前端/images/WechatIMG149.jpg)

```js
// 阻止vue在启动时生成生产提示
Vue.config.productionTip = false
```

# 声明式渲染

vue通过声明式方式进行渲染，不用直接和html打交道，一个 Vue 应用会将其挂载到一个 DOM 元素上，然后对其进行完全控制。那个 HTML 是我们的入口。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>初识vue</title>
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
        <!-- 通过插值语法插入数据，可以使用表达式，但不能使用js语句 -->
        <h1>Hello, {{name.toUpperCase()}}</h1>
    </div>
    <script>
        // 创建vue实例
        const vm = new Vue({
            el: '#demo', // el指定vue实例为哪个容器服务
            data: {
                name: 'world',
            }
        })
    </script>
</body>
</html>
```

# 模板语法

vue分为两种模板语法，插值和指令。

### 1. 插值

插值语法用于解析标签体内容，插值语法是`{{xxx}}`，直接读取data中的所有属性，且支持表达式，但不支持js语句。

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>模板语法</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        <h1>插值语法</h1>
        <h3>你好，{{name}}</h3>
        <h3>实际年龄：{{age}}</h3>
        <h3>虚岁：{{age+1}}</h3>
    </div>
    <script>
        new Vue({
            el: "#root",
            data: {
                name: 'shanla',
                age: 18,
            }
        })
    </script>
</body>
```

### 2. 指令

指令语法用于解析标签，包括：标签属性、标签体内容、绑定事件.....，指令语法是`v-xxx`，且指令可以缩写，比如`v-bind`简写成`:`，`v-on`简写成`@`

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>模板语法</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        <h1>指令语法</h1>
        <!-- 指令语法中也可以使用表达式 -->
        <a v-bind:href="website.url.toUpperCase()" v-bind:x="hello">点我去{{website.name}}</a>
        <!-- 缩写方式 -->
        <a :href="website.url" x="hello1">点我去{{website.name}}</a>
    </div>
    <script>
        new Vue({
            el: "#root",
            data: {
                website: {
                    name: '百度',
                    url: 'http://www.baidu.com'
                },
                hello: "你好"
            }
        })
    </script>
</body>
</html>
```

![image](/Users/kx/workspace/Notes/前端/images/WechatIMG150.jpg)
