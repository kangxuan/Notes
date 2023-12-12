# 脚手架配置代理

前端请求不同域名不同端口的接口，会出现跨域问题，通常使用nginx做代理，或者使用前端配置代理，这里介绍的是vue脚手架前端配置代理。

### 简单配置

在vue.config.js添加代理配置

```js
module.exports = {
  ...
  // 简单配置，但不能配置多个代理，不能灵活控制请求是否走代理
  devServer: {
    proxy: "http://localhost:5000"
  }
  ...
}
```

### 复杂配置

在vue.config.js添加代理配置

```js
module.exports = {
  ...
  // 可以配置多个代理，且可以灵活的请求是否走代理
  devServer: {
    proxy: {
      '/api1': { // 配置所有以/api1开头的请求路径
        target: 'http://localhost:5000', // 代理目标的基础的路径
        pathRewrite: {'^/api1': ''} // 路由重写，将'/api1'转成''
        changeOrigin: true, // 是否开启请求伪装
      },
      '/api2': {
        target: 'http://localhost:5001',
        pathRewrite: {'^/api2': ''},
      }
    }
  }
  ...
}
```

changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
changeOrigin默认值为true
