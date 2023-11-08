# 数据收集

html中常见的数据收集有`input`、`textarea`、`select`，其中`input`又可以是普通文本、单选、多选等。

vue通过`v-model`对表单数据进行收集，但`v-model`针对不同的表单效果不一样。

### 普通文本

针对`<input type="text>"`类似值的普通文本表单，则`v-model`收集的value值，也就是用户输入的值。

### 单选框

针对`<input type="radio">`来说，`v-model`收集的是value值，且要为其设置value值，如果不设置value值，则收集到的是`null`。

### 复选框

针对`<input type="checkbox">`来说，`v-model`收集的是value值。

未设置value值，会收集到null或[null]。

设置了value值分为两种情况，对于初始化值为非数组，那么收集到的是bool值（true or false）;如果初始化为数组则收集到的是value值组成的数组。

```html
<body>
    <div id="root">
        <form @submit.prevent="demo">
            账号：<input type="text" name="account" v-model="userInfo.account"><br>
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
```

### 运用在v-model上的修饰符

**trim**

去除前后的空格

**lazy**

当数据修改时才再次加载

**number**

自动转换为数字

```html
账号：<input type="text" name="account" v-model.trim="userInfo.account"><br>
年龄：<input type="number" name="age" v-model.number="userInfo.age"><br>
其他信息：<textarea v-model.lazy="userInfo.other"></textarea><br>
```
