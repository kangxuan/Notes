# 路由的概念

一个路由（route）就是一组映射关系（key-value），多个路由需要路由器（router）进行管理。

### vue的路由

key是路径，value是组件

### 单页面

传统的多页面通过链接标签跳转实现，单页面是在单页面中进行跳转，vue中的单页面通过路由器技术检测路由的变化，实现组件中之间的切换，这样就可以不刷新页面实现功能。

# 基本使用

### 1. 安装vue-router

```shell
# vue2安装vue-router@3
npm i vue-router
```

### 2. 配置router

一般新建一个router文件夹，在其下面创建文件`index.js`

```js
// 引入vue-router
import VueRouter from "vue-router"
// 引入要使用的组件
import BlogAbout from '../pages/BlogAbout'
import BlogHome from '../pages/BlogHome'
// 暴露并配置route规则
export default new VueRouter({
    routes: [
        {
            path: '/about', // 路径
            component: BlogAbout // 组件名
        },
        {
            path: '/home',
            component: BlogHome
        }
    ]
})
```

### 3. 应用插件

在`main.js`中注册插件

```js
import Vue from 'vue'
import App from './App'
// 导入vue-router插件
import VueRouter from 'vue-router'

// 引入编辑好的router
import router from './router'
// 使用插件
Vue.use(VueRouter)

new Vue({
    el: '#app',
    render(h) {
        return h(App)
    },
    router: router // 将router配置到VM中
})
```

### 4. 使用router

使用`router-link`标签代替`a`标签，`active-class`可配置选中的样式，`to`配置路由

```html
<router-link class="list-group-item" active-class="active" to="/about">About</router-link>
```

使用`router-view`指定展示区域，路由配置的组件会显示在这里。

```html
<router-view></router-view>
```


