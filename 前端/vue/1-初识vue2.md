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

# 
