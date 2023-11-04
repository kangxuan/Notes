# 监听事件

VUE中通过`v-on:xxx`对DOM元素进行监听事件，其中`xxx`是事件，常见的有`click`、`change`事件等，可以简写成`@xxx`。

### 简单表达式

`v-on:xxx`内容可以是简单的表达式，处理简单的业务逻辑。

```html
<body>
    <div id="root">
        <button v-on:click="count++">点击加1</button>
        <span>{{count}}</span>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            data: {
                count: 0
            },
        })
    </script>
</body>
```

### 事件处理方法

简单表达式没办法处理复杂的业务，所以引入事件方法，内容是一个方法，方法的第一个参数默认是这个事件本身。

```html
<body>
    <div id="root">
        <!-- <button v-on:click="showInfo">点我获取信息</button> -->
        <button @click="showInfo">点我获取信息</button>
        <button @click="showInfo2($event, 66)">点我获取信息2</button>
    </div>
    <script>
        const vm = new Vue({
            el: '#root',
            methods: {
                // 默认第一个形参是event
                showInfo(event) {
                    alert('这是你要的信息')
                },
                // 默认第一个形参是event，之后可以携带自己的参数
                showInfo2(event, number) {
                    alert('这是你要的信息！！')
                }
            }
        })
    </script>
</body>
```

### 事件修饰符

尽管我们能在事件处理方法中处理编写一些事件处理业务，比如阻止默认事件`event.preventDefault()`，但为了保证事件处理方法只关注纯业务方面的事，所以VUE提供了修饰符来处理类似的DOM事件细节。

```html
<body>
    <div id="root">
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

### 按键修饰符

针对键盘事件提供的修饰符也是为了保证方法中只专注业务本身而设计的。常见的键盘事件有`keydown`、`keyup`，修饰符为具体的哪个按键，比如enter键、空格键等，具体为`@keydown.enter`的形式，表示为按下enter时触发。

VUE提供了大部分的按键修饰符，但如果没有定义可以通过具体的keycode来绑定（不推荐），也可以通过自定义绑定。

```html
<body>
    <div id="root">
        <input type="text" placeholder="按下回车提示输入" @keydown.enter="showInfo">
    </div>
    <script>
        // 自定义按键修饰符，这里是距离，enter其实已经被vue定义
        Vue.config.keyCodes.enter = 13
        // Vue.config.keyCodes.f1 = 112
        new Vue({
			el:'#root',
			methods: {
				showInfo(e){
					// console.log(e.key,e.keyCode)
					console.log(e.target.value)
				}
			},
		})
    </script>
</body>
```

### 系统修饰符

系统修饰符主要是`ctrl`、`alt`、`shift`、`meta`与其他按键的结合，比如说`ctrl+c`、`ctrl+click`(按住ctrl，然后鼠标点击)、要和`keydown`、`keyup`、`click`等事件配合使用。

```html
<body>
    <div id="root">
        <!-- CTRL + C -->
        <input v-on:keyup.ctrl.c="showInfo2">
        <!-- 点击鼠标，然后按下enter键 -->
        <div v-on:click.enter="showInfo3">点击我获取机密</div>
    </div>
    <script>
        Vue.config.keyCodes.c = 67
        new Vue({
			el:'#root',
			methods: {
                showInfo2(e) {
                    alert('hello world')
                },
                showInfo3(e) {
                    alert('这是一个搞笑的故事')
                }
			},
		})
    </script>
</body>
```
