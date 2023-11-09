# 组件基础

在网页中，有内容相似的部分，且可以复用的板块可以单独抽出来，形成一个组件。

### 使用组件的步骤

vue要使用组件，首先定义组件、然后注册组件、最后使用组件。

### 使用一个简单的组件

```html
<body>
    <div id="root">
        <!-- 第三步：使用组件 -->
        <hello></hello>
    </div>
    <script>
        // 第一步：定义组件
        const hello = Vue.extend({
            // 模板
            template:`
                <div>
                    <h2>你好啊！{{name}}</h2>
                </div>
            `,
            // data必须写成函数，避免组件被复用时，数据存在引用关系。
            data() {
                return {
                    name: 'shanla'
                }
            },
        })
        // 第二步：注册全局组件
        // Vue.component('hello', hello)

        new Vue({
            el: '#root',
            // 第二步：局部注册组件
            components: {
                hello
            }
        })
    </script>
</body>
```

### 使用组件嵌套

一个组件可以使用另一个组件，这就是组件嵌套，使用组件必须要注册被调用的组件（components）。

```html
<body>
    <div id="root">

    </div>
    <script>
        const student = Vue.extend({
            name: 'student',
            template:`
                <div>
                    <h2>学生姓名：{{name}}</h2>	
                    <h2>学生年龄：{{age}}</h2>	
                </div>
            `,
            data() {
                return {
                    name: '张三',
                    age: 21
                }
            },
        })

        const school = Vue.extend({
            name: 'school',
            template:`
                <div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
					<student></student>
				</div>
            `,
            data() {
                return {
                    name: '北京大学',
                    address: '北京'
                }
            },
            // 组件嵌套组件需要将组件注册进组件
            components: {
                student
            }
        })

        const hello = Vue.extend({
            template: `<h1>{{msg}}</h1>`,
            data() {
                return {
                    msg: 'hello world'
                }
            }
        })

        const app = Vue.extend({
            template:`
                <div>	
					<hello></hello>
					<school></school>
				</div>
            `,
            components: {
                school,
                hello,
            }
        })

        new Vue({
            template: '<app></app>',
            el: '#root',
            components:{
                app
            }
        })
    </script>
</body>
```

# 组件分离写法

在上面的代码中，template都定义在组件内部，这样会导致html代码都挤到一个字符串中，非常不方便，可以使用分离写法。

```html
<body>
    <div id="root">
        <t1></t1>
        <t2></t2>
    </div>
    <!-- 定义模板方法一 -->
    <script type="text/x-template" id="t1">
        <div>
            <h2>组件模板的分离写法1</h2>
            <p>type必须是text/x-template</p>
        </div>
    </script>
    <!-- 定义模板方法二 -->
    <template id="t2">
        <div>
            <h2>组件模板的分离写法2</h2>
        </div>
    </template>
    <script>
        new Vue({
            el: '#root',
            // 注册组件
            components: {
                t1: {
                    template: '#t1'
                },
                t2: {
                    template: '#t2'
                }
            }
        })
    </script>
</body>
```

![image](/Users/kx/workspace/Notes/前端/images/WechatIMG156.jpg)

# 传递数据

组件中的一些数据是不固定的，需要从上一层组件传入数据，vue中通过`props`进行传递。

### 父组件向子组件传递数据

通过`props`进行传递

```html
<body>
    <div id="root">
        <!-- 将数据传递给子组件 -->
        <cpn :cmessage="message" :ccars="cars"></cpn>
    </div>
    <template id="cpn">
        <div>
            <h2>{{cmessage}}</h2>
            <ul>
                <li v-for="(val, index) of ccars" :key="index">
                    {{val}}
                </li>
            </ul>
        </div>
    </template>
    <script>
        // 创建组件
        const cpn = Vue.extend({
            template: '#cpn',
            // props是定义传递的数据字段以及其类型等
            props: {
                cmessage: {
                    type: String, // 限制类型
                    required: true, // 在组件中使用必须传递true
                    default: 'this is a message' // 默认值
                },
                ccars: {
                    type: Array, // 限制类型是数组
                    required: true,
                    default() { // 如果是数组或对象，默认值必须是一个函数
                        return []
                    }
                }
            },
            // data() {
            //     return {}
            // },
            // methods: {
                
            // },
        })
        new Vue({
            el: '#root',
            data: {
                message: 'that are cars!',
                cars: ['比亚迪', '吉利', '长城']
            },
            // 注册组件
            components: {
                cpn
            }
        })
    </script>
</body>
```

### 子组件向父组件传递值

通过内置的`$emit`，调用一个父组件的定义的函数对父组件的值进行操作。

```html
<body>
    <div id="root">
        <!-- 通过:将数据传递给子组件，通过@将方法传递给子组件，注意不要写驼峰，比如@num1change -->
        <cpn :number1="num1" :number2="num2" @num1change="num1Change" @num2change="num2Change"></cpn>
        <h2>父组件{{num1}}</h2>
        <input type="text" v-model="num1" >
        <h2>父组件{{num2}}</h2>
        <input type="text" v-model="num2">
    </div>
    <template id="cpn">
        <div>
            <h2>子组件number1{{number1}}</h2>
            <h2>子组件dnumber1{{dnumber1}}</h2>
            <input type="text" :value="dnumber1" @input="num1input">
            <h2>子组件number2{{number2}}</h2>
            <h2>子组件dnumber2{{dnumber2}}</h2>
            <input type="text" :value="dnumber2" @input="num2input">
        </div>
    </template>
    <script>
        const cpn = {
            template: '#cpn',
            data() {
                return {
                    dnumber1: this.number1,
                    dnumber2: this.number2,
                }
            },
            // 传递两个数据
            props: {
                number1: [Number, String],
                number2: [Number, String]
            },
            methods: {
                num1input(event) {
                    this.dnumber1 = event.target.value
                    // 通过内置的$emit函数调用父组件注册进来的函数修改父组件的值
                    this.$emit('num1change', this.dnumber1)
                },
                num2input(event) {
                    this.dnumber2 = event.target.value
                    this.$emit('num2change', this.dnumber2)
                }
            },
        }

        const vm = new Vue({
            el: '#root',
            data: {
                num1: 1,
                num2: 2,
            },
            methods: {
                num1Change(value) {
                    this.num1 = value
                },
                num2Change(value) {
                    this.num2 = value
                }
            },
            components: {
                cpn
            }
        })
    </script>
</body>
```








