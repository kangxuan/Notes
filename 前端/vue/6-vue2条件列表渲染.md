# 条件渲染

条件渲染主要的两个指令是`v-if`和`v-show`，这两个指令很像的，区别在于`v-if`是真实渲染的，如果条件为false，则DOM节点不会被渲染，而`v-show`不管true还是false，都会渲染出DOM节点，通过`display:none`来控制显隐。

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

列表渲染通过指令`v-for`来实现，可以遍历数组、对象、字符串、指定次数等。

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

### key（需要再看看原理）

`:key`对于列表非常重要，如果列表数据不会变动我们可以使用索引值来代替，但如果对于列表的元素有写操作（经常变化），我们最好使用唯一的id。如果在原数组开头增加一个元素，那么通过索引值作为key就会出现问题。

```html
<body>
    <!-- 
            面试题：react、vue中的key有什么作用？（key的内部原理）

                    1. 虚拟DOM中key的作用：
                                    key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】, 
                                    随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

                    2.对比规则：
                                (1).旧虚拟DOM中找到了与新虚拟DOM相同的key：
                                            ①.若虚拟DOM中内容没变, 直接使用之前的真实DOM！
                                            ②.若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。

                                (2).旧虚拟DOM中未找到与新虚拟DOM相同的key
                                            创建新的真实DOM，随后渲染到到页面。

                    3. 用index作为key可能会引发的问题：
                                        1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:
                                                        会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。

                                        2. 如果结构中还包含输入类的DOM：
                                                        会产生错误DOM更新 ==> 界面有问题。

                    4. 开发中如何选择key?:
                                        1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
                                        2.如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，
                                            使用index作为key是没有问题的。
    -->
    <div id="root">
        <h2>人员列表（遍历数组）</h2>
        <button @click.once="add">添加一个老刘</button>
        <ul>
            <!-- 通过index作为key会导致添加错误 -->
            <li v-for="(p, index) of persons" :key="index">
                {{p.name}}-{{p.age}}
                <input type="text">
            </li>

            <!-- 通过唯一id作为key则不会有问题 -->
            <!-- <li v-for="(p, index) of persons" :key="p.id">
                {{p.name}}-{{p.age}}
                <input type="text">
            </li> -->
        </ul>
    </div>
    <script>
        Vue.config.productionTip = false

        const vm = new Vue({
            el: '#root',
            data: {
                persons: [
                    {id:'001',name:'张三',age:18},
                    {id:'002',name:'李四',age:19},
                    {id:'003',name:'王五',age:20}
                ]
            },
            methods: {
                    add(){
                        const p = {id:'004',name:'老刘',age:40}
                        this.persons.unshift(p)
                    }
                },
        })
    </script>
</body>
```

### 列表过滤案例

```html
<body>
    <div id="root">
        <input type="text" placeholder="请输入要搜索的名字" v-model="keyWord">
        <ul>
            <li v-for="(p, index) of filPerons" :key="index">
                {{p.id}} - {{p.name}} - {{p.age}} - {{p.sex}}
            </li>
        </ul>
    </div>
    <script>
        // 使用watch实现
        // new Vue({
        //     el: '#root',
        //     data: {
        //         keyWord: '',
        //         persons: [
        //             {id:'001',name:'马冬梅',age:19,sex:'女'},
        //             {id:'002',name:'周冬雨',age:20,sex:'女'},
        //             {id:'003',name:'周杰伦',age:21,sex:'男'},
        //             {id:'004',name:'温兆伦',age:22,sex:'男'}
        //         ],
        //         filPerons: [],
        //     },
        //     watch: {
        //         keyWord: {
        //             immediate: true, // 初始化时让handler调用一下
        //             handler(newValue) {
        //                 this.filPerons = this.persons.filter((p) => {
        //                     return p.name.indexOf(newValue) !== -1
        //                 })
        //             }
        //         }
        //     }
        // })

        // 使用computed实现
        new Vue({
            el: '#root',
            data: {
                keyWord: '',
                persons: [
                    {id:'001',name:'马冬梅',age:19,sex:'女'},
                    {id:'002',name:'周冬雨',age:20,sex:'女'},
                    {id:'003',name:'周杰伦',age:21,sex:'男'},
                    {id:'004',name:'温兆伦',age:22,sex:'男'}
                ],
            },
            computed: {
                filPerons() {
                    // 使用数组的过滤函数，p是遍历的元素
                    return this.persons.filter((p) => {
                        // 返回包含keyword的元素
                        return p.name.indexOf(this.keyWord) !== -1
                    })
                }
            }
        })
    </script>
</body>
```

### 列表排序案例

在上面案例再增加一个age字段排序，需要先筛选再对筛选后的数据进行排序。

```html
<body>
    <div id="root">
        <input type="text" placeholder="请输入要搜索的名字" v-model="keyWord">
        <button @click="sortType = 2">年龄升序</button>
        <button @click="sortType = 1">年龄降序</button>
        <button @click="sortType = 0">原排序</button>
        <ul>
            <li v-for="(p, index) of filPerons" :key="index">
                {{p.id}} - {{p.name}} - {{p.age}} - {{p.sex}}
            </li>
        </ul>
    </div>
    <script>
        new Vue({
            el: '#root',
            data: {
                keyWord: '',
                sortType: 0,
                persons: [
                    {id:'001',name:'马冬梅',age:19,sex:'女'},
                    {id:'002',name:'周冬雨',age:20,sex:'女'},
                    {id:'003',name:'周杰伦',age:21,sex:'男'},
                    {id:'004',name:'温兆伦',age:22,sex:'男'}
                ]
            },
            computed: {
                filPerons() {
                    // 先过滤
                    const arrs = this.persons.filter((p) => {
                        return p.name.indexOf(this.keyWord) !== -1
                    })
                    // 再排序
                    if (this.sortType) {
                        // p1 p2 是前后元素
                        arrs.sort((p1, p2) => {
                            return this.sortType === 1 ? p2.age - p1.age : p1.age - p2.age
                        })
                    }
                    return arrs
                }
            }
        })
    </script>
</body>
```

 
