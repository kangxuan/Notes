# 生命周期是什么

vue组件实例被创建时会都会经历一个生命周期。vue针对每个节点都有一个特定的表达，比如`mounted`等，可以在这些节点做一些特定的业务处理。

![生命周期](../images/生命周期.png)

### beforeCreate

在组件实例初始化完成之后立即调用，此时无法通过vm访问到data中的数据、methods中的方法。

### created

在组件实例处理完所有和状态相关的选项后调用，此时可以通过vm访问到data中的数据、methods中的方法。

### beforeMount

在组件被挂载之前调用，此时页面呈现的是未经vue编译的DOM结构，对DOM的操作，最终不奏效，因为挂载之后会重新替换页面。

### mounted

在组件被挂载之后调用，也就是vue解析完模板并将初始的DOM放入页面之后，刷新页面调用一次。此时页面中呈现的是经过Vue编译的DOM，对DOM的操作均有效。

一般在此开始定时器、发送网络请求、订阅消息、绑定自定义事件等初始化操作。

### beforeUpdate

在组件即将因为一个响应式状态变更而更新其 DOM 树之前调用，此时数据是新的，但页面是旧的，即页面尚未和数据保持同步。

### updated

在组件因为一个响应式状态变更而更新其 DOM 树之后调用，此时数据是最的，页面也是新的，即页面和数据保持同步。

### beforeDestroy

VM实例被卸载之前调用，此时VM中所有的data、methods、指令等都处于可用状态，一般在此关闭定时器、取消订阅消息、解绑自定义时间等收尾工作。

### Destroyed

VM实例被卸载之后调用，对应的Vue实例的所有指令都被解绑，所有的事件监听被移除，所有的子实例也都被销毁。

```html
<body>
    <div id="root">
        <h2 v-text="n"></h2>
        <h2>当前的n值是：{{n}}</h2>
        <button @click="add">点我n+1</button>
        <button @click="bye">点我销毁vm</button>
    </div>
    <script>
        new Vue({
            el: '#root',
            data: {
                n: 1
            },
            methods: {
                add() {
                    console.log('add')
                    this.n++
                },
                bye(){
                    console.log('bye')
                    this.$destroy()
                }
            },
            watch:{
                n(){
                    console.log('n变了')
                }
            },
            beforeCreate() {
                //钩子函数中的this指向vm
                console.log(this)
                console.log('beforeCreate')
            },
            created() {
                console.log('created')
            },
            beforeMount() {
                console.log('beforeMount')
            },
            mounted() {
                console.log('mounted')
            },
            beforeUpdate() {
                console.log('beforeUpdate')
            },
            updated() {
                console.log('updated')
            },
            beforeDestroy() {
                console.log('beforeDestroy')
            },
            destroyed() {
                console.log('destroyed')
            },
        })
    </script>
</body>
```
