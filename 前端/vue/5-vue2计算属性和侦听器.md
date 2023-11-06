# 计算属性

### 计算属性用来解决什么问题

尽管插值语法中可以编写表达式，但如果复杂的业务写进去会导致代码维护难，为了保持代码简洁性，所以引入了计算属性。

计算属性还有另外一个更重要的功能就是缓存，每当触发重新渲染时，当数据没有任何变化时会读取缓存数据，提高效率。

```html
<body>
    <div id="root">
        姓：<input type="text" v-model="firstName"> <br/><br/>
        名：<input type="text" v-model="lastName"> <br/><br/>
        全名：<input type="text" v-model="fullName">
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                firstName: '山',
                lastName: '辣'
            },
            computed: {
                // fullName:{
                //     //get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
                //     //get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时。
                //     get(){
                //         console.log('get被调用了')
                //         // console.log(this) //此处的this是vm
                //         return this.firstName + '-' + this.lastName
                //     },
                //     //set什么时候调用? 当fullName被修改时。
                //     set(value){
                //         console.log('set',value)
                //         const arr = value.split('-')
                //         this.firstName = arr[0]
                //         this.lastName = arr[1]
                //     }
                // }
                // 这样简写只有getter，没有setter
                fullName(){
                    console.log('获取时才被调用')
                    return this.firstName + '-' + this.lastName
                }
            }
        })
    </script>
</body>
```

# 侦听器（监视属性）

### 侦听器是什么

侦听器是给属性增加的监视行为，使用`watch`实现，当一个数据变化时会调用，虽然说侦听器更加通用，但如果计算属性也能实现尽量选择计算属性。

```html
<body>
    <div id="root">
        姓：<input type="text" v-model="firstname"><br>
        名：<input type="text" v-model="lastname"><br>
        全名：<span>{{fullname}}</span>
    </div>
    <script>
        const firstname = '山'
        const lastname = '辣'
        const vm = new Vue({
            el: '#root',
            data: {
                firstname: firstname,
                lastname: lastname,
                fullname: firstname + '-' + lastname
            },
            watch: {
                // 接收变化后的值
                firstname(val) {
                    this.fullname = val + '-' + this.lastname
                },
                lastname(val) {
                    this.fullname = this.firstname + '-' + val
                }
            }
        })
    </script>
</body>
```

从上述例子可以看出，实际上通过计算属性往往更加简洁，选择时优先选择使用计算属性，而不是侦听器。

### 侦听器可以实现异步操作
