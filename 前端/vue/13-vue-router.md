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

# 注意点

- 路由组件一般放在`pages`文件夹下，一般组件通常放在`components`下。

- 路由组件切换会先销毁以前的组件，然后再挂载要切换的组件。

- 每个组件都有自己的`route`信息，存储着自己的路由信息。

- 整个应用只有一个`router`信息，里面存储着更多的方法。

# 多级路由

路由组件中包含其他子组件可以使用`children`配置子路由

```js
export default new VueRouter({
    routes: [
        {
            path: '/about',
            component: BlogAbout
        },
        {
            path: '/home',
            component: BlogHome,
            children: [ // 通过children配置子路由
                {
                    path: 'message', // 子路由不用使用/
                    component: HomeMessage
                },
                {
                    path: 'news',
                    component: HomeNews
                }
            ]
        }
    ]
})
```

在路由组件中使用子路由

```html
<!-- 这里的to路由要写完整的 -->
<router-link class="list-group-item" active-class="active" to="/home/news">News</router-link>
```

# query参数

父组件传递参数

```html
<template>
  <div>
    <ul>
        <li v-for="m in messages" :key="m.id">
          <!-- to的字符串写法 -->
          <!-- <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{m.title}}</router-link>&nbsp;&nbsp; -->
          <!-- to的对象写法 -->
          <router-link :to="{
            path: '/home/message/detail',
            query: {
              id: m.id,
              title: m.title
            }
          }">{{m.title}}</router-link>
        </li>
      </ul>
      <hr>
      <router-view></router-view>
  </div>
</template>
```

子组件接收query参数通过`this.$route.query`进行接收

```html
<template>
  <div>
    id:{{this.$route.query.id}},title:{{this.$route.query.title}}
  </div>
</template>
```

# 路由命名

由于套娃式会导致多级路由的路径越来越长，通常可以给层级太深的路由配置名称，简化路径。

给路由命名：

```js
export default new VueRouter({
    routes: [
        {
            path: '/about',
            component: BlogAbout
        },
        {
            path: '/home',
            component: BlogHome,
            children: [
                {
                    path: 'message',
                    component: HomeMessage,
                    children: [
                        {
                            name: 'hello', // 给路由命令
                            path: 'detail',
                            component: HomeMessageDetail
                        }
                    ]
                },
                {
                    path: 'news',
                    component: HomeNews
                }
            ]
        }
    ]
})
```

使用命名路由：

```html
<template>
  <div>
    <ul>
        <li v-for="m in messages" :key="m.id">
          <!-- 使用命令定位路由 -->
          <router-link :to="{
            name: 'hello',
            query: {
              id: m.id,
              title: m.title
            }
          }">{{m.title}}</router-link>
        </li>
      </ul>
      <hr>
      <router-view></router-view>
  </div>
</template>
```

# params参数

配置路由，声明接收params参数

```js
export default new VueRouter({
    routes: [
        {
            path: '/about',
            component: BlogAbout
        },
        {
            path: '/home',
            component: BlogHome,
            children: [
                {
                    path: 'message',
                    component: HomeMessage,
                    children: [
                        {
                            name: 'hello',
                            path: 'detail/:id/:title', // 配置接收params参数规则
                            component: HomeMessageDetail
                        }
                    ]
                },
                {
                    path: 'news',
                    component: HomeNews
                }
            ]
        }
    ]
})
```

传递参数

```html
<template>
  <div>
    <ul>
        <li v-for="m in messages" :key="m.id">
          <!-- 第一种：to的字符串写法 -->
          <!-- <router-link :to="`/home/message/detail/${m.id}/${m.title}`">{{m.title}}</router-link>&nbsp;&nbsp; -->
          <!-- 第二种：to的对象写法 -->
          <router-link :to="{
            name: 'hello',
            params: {
              id: m.id,
              title: m.title
            }
          }">{{m.title}}</router-link>
        </li>
      </ul>
      <hr>
      <router-view></router-view>
  </div>
</template>
```

> 特别注意：使用to的对象写法时，不能写path配置，必须写成name的配置。

接收参数

```html
<template>
  <div>
    id:{{this.$route.params.id}},title:{{this.$route.params.title}}
  </div>
</template>
```

# props传递数据

让路由组件更方便的收到数据。在路由中配置，使用时通过props声明。

```js
...
{
    name: 'hello',
    // path: 'detail/:id/:title',
    path: 'detail',
    component: HomeMessageDetail,
    // props第一种写法：对象写法，该对象中的所有k-v都会以props的形式传给Detail组件
    // props:{a: 1, b: 2}
    // props第二种写法：布尔值，如果为true时，会将params参数放到props中
    // props: true,
    // props第三种写法：函数
    props($route) {
        return {
            id: $route.query.id,
            title: $route.query.title,
            a: 1, 
            b: 'hello'
        }
    }
}
...
```

# 浏览器浏览记录模式

### 1. push模式

这种模式是默认的，是对记录记录进行追加，通过前进后退可以进行访问

### 2. replace模式

这种模式是替换当前的记录。

```html
<!-- 开启replace模式 -->
<router-link replace="true" class="list-group-item" active-class="active" to="/home/news">News</router-link>
<!-- 可省去true -->
<router-link replace class="list-group-item" active-class="active" to="/home/news">News</router-link>
```

# 编程式路由导航

不借助`<router-link>`实现跳转，让路由跳转更加灵活

```js
this.$router.push({
  name: 'hello',
  query: {
    id: xxx,
    title: xxx
  }
})
this.$router.replace({
  name: 'hello',
  query: {
    id: xxx,
    title: xxx
  }
})

this.$router.forward() //前进
this.$router.back() //后退
this.$router.go(n) //可前进也可后退，n>0则为前进n步，n<0则为后退n步
```

# 缓存路由组件

让不展示的路由组件保持挂载，不被销毁。

```html
<!-- 缓存多个路由组件 -->
<!-- <keep-alive :include="['News','Message']"> -->

<!-- 缓存一个路由组件 -->
<keep-alive include="News">
	<router-view></router-view>
</keep-alive>
```

# 路由守卫

路由守卫的作用是对路由进行权限控制

### 1. 全局守卫

针对router全局进行权限控制

```js
...
// 全局前置路由守卫---初始化时被调用，每次路由切换之前被调用
router.beforeEach((to, from, next) => {
    console.log('前置路由守卫', to, from)
    if (to.meta.isAuth === true) {
        if (localStorage.getItem('school') === 'hechixueyuan') {
            next()
        } else {
            alert("学校名称不对")
        }
    } else {
        next()
    }
})

// 全局后置路由守卫---初始化的时候被调用，每次路由切换之后被调用
router.afterEach((to, from) => {
    console.log('后置路由守卫', to, from)
    document.title = to.meta.title || '默认标题'
})
...
```

### 2. 独享守卫

针对某一个路由的权限控制

```js
{
    path: 'news',
    component: HomeNews,
    meta: {isAuth: true, title: '新闻'},
    // 进入之前调用
    beforeEnter: (to, from, next) => {
        console.log('独享路由守卫', to, from)
        if (to.meta.isAuth) {
            if (localStorage.getItem('school') === 'hechixueyuan') {
                next()
            } else {
                alert('学校名不对，无权限查看！')
            }
        } else {
            next()
        }
    }
}
```

### 3. 组件内守卫

写在组件内的守卫，分为进入守卫和离开守卫。

```js
...
// 进入守卫：通过路由规则，进入该组件时被调用
beforeRouteEnter (to, from, next) {
  console.log('About--beforeRouteEnter', to, from)
  if (to.meta.isAuth) {
    if (localStorage.getItem('school') === 'hechixueyuan') {
      next()
    } else {
      alert('学校名不对，无权限查看！')
    }
  } else {
    next()
  }
},
// 离开守卫：通过路由规则，离开该组件时被调用
beforeRouteLeave (to, from, next) {
  console.log('About--beforeRouteLeave', to, from)
  next()
}
...
```

# 路由的两种模式

### 1. hash值

url`#`之后的叫做hash值，hash值不会带给服务器

### 2. hash模式

1. 地址中永远带着`#`号，不美观 。
2. 若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法。
3. 兼容性较好。

### 3. history模式

1. 地址干净，美观 。
2. 兼容性和hash模式相比略差。
3. 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题。


