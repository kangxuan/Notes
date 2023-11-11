# 生命周期是什么

vue组件实例被创建时会都会经历一个生命周期。vue针对每个节点都有一个特定的表达，比如`mounted`等，可以在这些节点做一些特定的业务处理。

![生命周期](../images/生命周期.png)

### beforeCreate

在组件实例初始化完成之后立即调用，此时无法通过vm访问到data中的数据、methods中的方法。

### created

在组件实例处理完所有和状态相关的选项后调用，此时可以通过vm访问到data中的数据、methods中的方法。

### beforeMount

在组件被挂载之前调用

### mounted

在组件被挂载之后调用，也就是vue解析完模板并将初始的DOM放入页面之后，刷新页面调用一次。

### beforeUpdate

在组件即将因为一个响应式状态变更而更新其 DOM 树之前调用

### updated

在组件因为一个响应式状态变更而更新其 DOM 树之后调用

### beforeUnmount

在一个组件实例被卸载之前调用。

### unmounted

在一个组件实例被卸载之后调用

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
