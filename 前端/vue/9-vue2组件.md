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


