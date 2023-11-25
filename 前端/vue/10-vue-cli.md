# vue-cli使用步骤

### 1.安装vue-cli

全局安装@vue/cli

```shell
npm install -g @vue/cli
# 执行vue命令看是否安装成功
vue
```

### 2.创建项目

```shell
vue create vue_test
```

### 3.启动项目

```shell
# 进入项目
cd vue_test
# 启动服务
npm run serve
```

# vue-cli结构分析

```
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   │── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
├── package-lock.json: 包版本控制文件
```

### vue.js与vue.runtime.xxx.js的区别：

vue.js是完整版的Vue，包含：核心功能 + 模板解析器。

vue.runtime.xxx.js是运行版的Vue，只包含：核心功能；没有模板解析器。

因为vue.runtime.xxx.js没有模板解析器，所以不能使用template这个配置项，需要使用render函数接收到的createElement函数去指定具体内容。

# vue.config.js配置文件

输出配置命令，因为vue-cli的默认配置不想被随意修改进行了隐藏，通过下面的命令可以看出

```shell
vue inspect > output.js
```

如需要对配置进行修改，vue-cli提供了一个文件`vue.config.js`可以进行定制配置，具体配置参数参考：[配置参考 | Vue CLI](https://cli.vuejs.org/zh/config/)

```js
// 示例配置入口文件
module.exports = {
  pages: {
    index: {
      // 入口文件
      entry: 'src/index/main.js'
    }
  }
}
```

# ref属性

ref属性用来给元素或子组件注册引用信息（类似id）

```html
<template>
    <div>
        <!-- 给普通的DOM元素打ref标识 -->
        <h1 v-text="msg" ref="title"></h1>
        <button ref="btn" @click="showDOM">点我输入上方的DOM元素</button>
        <!-- 给子组件打ref标识 -->
        <SchoolInfo ref="sch"></SchoolInfo>
    </div>
</template>

<script>
    import SchoolInfo from './components/SchoolInfo.vue'
    export default {
        name: 'App',
        components: {SchoolInfo},
        data() {
            return {
                msg: 'welcome to vue'
            }
        },
        methods: {
            showDOM() {
                // ref放在DOM元素上返回的是真实的DOM元素
                console.log(this.$refs.title)
                console.log(this.$refs.btn)
                // ref写在组件上可以返回组件实例对象
                console.log(this.$refs.sch)
            }
        }
    }
</script>
```

# props配置项

props可以让组件接收到外部传来的数据

### 1. 传递数据

```html
<Demo name="xxx">
```

### 2. 接收数据

简单接收

```js
props: ["name"]
```

只限制类型

```js
props: {name: String}
```

限制类型、必要性、指定默认值

```js
props: {
    name: {
        type: String, // 限制类型
        required: true, // 限制必要性
        default: 'shanla', // 指定默认值
    }
}
```

**备注**：props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据。


