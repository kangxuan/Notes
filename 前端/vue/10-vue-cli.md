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

# mixin（混入）

可以把多个组件公用的配置或功能提取出来形成一个混入对象

定义混合

```js
export const hunhe = {
    methods: {
        showName() {
            alert(this.name)
        }
    },
    mounted() {
        console.log('哈哈哈')
    },
}
// 配置数据
export const hunhe2 = {
    data() {
        return {
            x: 100,
            y: 200
        }
    },
}
```

使用混入

```js
// 第一种全局混入，一般在入口文件中使用，会在vc和vm上都有
import {hunhe, hunhe2} from './mixin'
Vue.mixin(hunhe)
Vue.mixin(hunhe2)

// 第二种局部混入，一般在组件中使用
import {hunhe,hunhe2} from '../mixin'
export default {
    name:'Student',
    data() {
        return {
            name:'张三',
            sex:'男'
        }
    },
    mixins:[hunhe,hunhe2]
}
```

**原则**：data、methods这些混合如果和组件的冲突，以组件的配置为准；如果是生命周期钩子冲突，都会被执行。

# 插件使用

用来增强vue，本质是包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是使用者传递的参数

### 1. 定义插件

一般单独起一个插件js文件

```js
// 导出
export const plugins = {
    install(Vue, a, b) {
        console.log('@@@', a, b)

        //全局过滤器
		Vue.filter('mySlice',function(value){
			return value.slice(0,4)
		})

        //定义全局指令
		Vue.directive('fbind',{
			//指令与元素成功绑定时（一上来）
			bind(element,binding){
				element.value = binding.value
			},
			//指令所在元素被插入页面时
			inserted(element){
				element.focus()
			},
			//指令所在的模板被重新解析时
			update(element,binding){
				element.value = binding.value
			}
		})

        //定义混入
		Vue.mixin({
			data() {
				return {
					x:100,
					y:200
				}
			},
		})

        //给Vue原型上添加一个方法（vm和vc就都能用了）
		Vue.prototype.hello = ()=>{alert('你好啊')}
    }
}
```

### 2. 导入插件

```js
// 导入插件
import {plugins} from './plugins'

// 使用插件
Vue.use(plugins, 1, 2)
```

### 3. 组件中使用插件定义的功能

```html
<template>
  <div>
    <h2>学生姓名：{{name}}</h2>
    <h2>学生性别：{{gender}}</h2>
    <input type="text" v-fbind:value="name">
  </div>
</template>
```

# scoped样式

scoped作用在style标签上，让样式在局部生效，防止冲突

```html
<style scoped>
    .demo {
        background-color: aqua;
    }
</style>
```


