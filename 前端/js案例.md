### 1. 全选按钮

```html
<body>
    <table>
        <thead>
            <tr>
                <th><input type="checkbox" name="allcheck" id="allcheck"><label for="allcheck">全选</label></th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><input type="checkbox" name="ids" class="soncheck"></td>
            </tr>
            <tr>
                <td><input type="checkbox" name="ids" class="soncheck"></td>
            </tr>
            <tr>
                <td><input type="checkbox" name="ids" class="soncheck"></td>
            </tr>
            <tr>
                <td><input type="checkbox" name="ids" class="soncheck"></td>
            </tr>
        </tbody>
    </table>
    <script>
        // 获取全选按钮
        const allCheck = document.querySelector('#allcheck')
        // 获取子选项按钮
        const cks = document.querySelectorAll('.soncheck')
        // 当选中全选按钮则选中所有的子选项按钮
        allCheck.addEventListener('click', function(){
            for (const key in cks) {
                cks[key].checked = this.checked
            }
        })
        // 选中子选项按钮时判断一下是否所有的子选项按钮都被选中，如果是则全选按钮也要选中，反之则不选中
        for (const key in cks) {
            cks[key].addEventListener('click', function(){
                // if (cks.length == document.querySelectorAll('.soncheck:checked').length) {
                //     allCheck.checked = true
                // } else {
                //     allCheck.checked = false
                // }
                allCheck.checked = cks.length === document.querySelectorAll('.soncheck:checked').length
            })
        }
    </script>
</body>
```

### 2. 电梯案例

```js
// 电梯随着滚动显隐
const elevator = document.querySelector('.xtx-elevator')
window.addEventListener('scroll', function(){
  const n = document.documentElement.scrollTop
  // 当卷300则显示，否则不显示
  elevator.style.opacity = n >= 300 ? 1 : 0
})
// 回到顶部
const toTop = document.querySelector('#backTop')
toTop.addEventListener('click', function(){
  // 设置scrollTop直接回到顶部
  document.documentElement.scrollTop = 0
  // 另一种写法采用scrollTo方法
  // window.scrollTo(0, 0)
})
```

### 3. 固定头部案例

```html

```

### 4. 倒计时

```js
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>倒计时</title>
    <style>
        .clocks {
            width: 240px;
            height: 305px;
            background-color: blue;
            color: #fff;
            overflow: hidden;
            text-align: center;
            font-weight: bold;
        }
        .today {
            font-size: 16px;
            margin: 10px auto;
        }
        .title {
            font-size: 30px;
        }
        .timer {
            font-size: 18px;
        }
        .footer {
            font-size: 30px;
        }
    </style>
</head>
<body>
    <div class="clocks">
        <p class="today">今天是2023年08月08日</p>
        <p class="title">下班倒计时</p>
        <p class="timer">12:12:30</p>
        <p class="footer">18:00下班</p>
    </div>
    <script>
        // 写一个随机颜色的函数
        function getRandomColor() {
            let color = '#'
            const arrs = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f']
            for (let i=0; i<6; i++) {
                let rand = Math.floor(Math.random() * arrs.length)
                color += arrs[rand]
            }
            return color
        }
        // 每次刷新给一个随机颜色
        const clocks = document.querySelector('.clocks')
        clocks.style.backgroundColor = getRandomColor()

        const today = document.querySelector('.today')
        const timer = document.querySelector('.timer')
        const date = new Date()
        const year = date.getFullYear()
        const month = date.getMonth() + 1
        const day = date.getDate()
        today.innerHTML = `今天是${year}年${month}月${day}日`

        // 获取倒计时
        function getCountTimer() {
            // 当前时间戳
            const currTime = +new Date()
            // 截止时间，设定每天18:00下班
            const endTime = +new Date(`${year}-${month}-${day} 18:00:00`)
            // 差距多少秒
            const diff = (endTime - currTime) / 1000
            if (diff <= 0) {
                return '下班了你愣着干嘛！'
            }
            // 计算时分秒
            let h = parseInt(diff / 3600 % 24)
            let m = parseInt(diff / 60 % 60)
            let s = parseInt(diff % 60)
            h = h < 10 ? '0' + h : h
            m = m < 10 ? '0' + m : m
            s = s < 10 ? '0' + s : s
            return h + ':' + m + ':' + s
        }
        timer.innerHTML = getCountTimer()
        // 定时器每秒变化
        setInterval(function(){
            timer.innerHTML = getCountTimer()
        }, 1000)
    </script>
</body>
</html>
```

### 5. 表格操作

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>学生信息表</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        .content {
            width: 980px;
            margin: 10px auto;
        }
        .content h2 {
            font-size: 30px;
            text-align: center;
            margin: 30px;
        }
        .input {
            display: flex;
            justify-content: center;
        }
        input,select {
            width: 80px;
            height: 27px;
            border-radius: 5px;
            border:1px solid aqua;
            outline: none;
            margin-right: 10px;
        }
        .content button {
            width: 80px;
            height: 27px;
            background-color: blueviolet;
            color: #FFF;
            border:none;
            border-radius: 5px;
            cursor: pointer;
        }
        .result table {
            width: 100%;
            border-spacing: 0;
            border-collapse: collapse;
        }
        .result table thead th {
            height: 36px;
            background-color: #cfe5ff;
            font-size: 20px;
            font-weight:400;
            color: #695ae8;
            border: 1px solid #b8daff;
        }
        .result table tbody td {
            height: 36px;
            font-size: 16px;
            font-weight: 400;
            text-align: center;
            border: 1px solid #b8daff;
        }
    </style>
</head>
<body>
    <div class="content">
        <h2>新增学员</h2>
        <div class="input">
            <form action="#" id="info">
                姓名：<input type="text" name="name" id="name">
                年龄：<input type="text" name="age" id="age">
                性别：<select id="gender" name="gender">
                    <option value="男">男</option>
                    <option value="女">女</option>
                </select>
                薪资：<input type="text" id="salary" name="salary">
                就业城市：<select id="city" name="city">
                    <option value="北京">北京</option>
                    <option value="上海">上海</option>
                    <option value="广州">广州</option>
                    <option value="深圳">深圳</option>
                </select>
                <button type="submit">录入</button>
            </form>
        </div>
        <h2>就业榜</h2>
        <div class="result">
            <table>
                <thead>
                    <tr>
                        <th>学号</th>
                        <th>姓名</th>
                        <th>年龄</th>
                        <th>性别</th>
                        <th>薪资</th>
                        <th>就业城市</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody>

                </tbody>
            </table>
        </div>
    </div>
    <script>
        const uname = document.querySelector('#name')
        const age = document.querySelector('#age')
        const gender = document.querySelector('#gender')
        const salary = document.querySelector('#salary')
        const city = document.querySelector('#city')
        const info = document.querySelector('#info')
        const tbody = document.querySelector('tbody')
        const items = document.querySelectorAll('[name]')
        let arrs = []
        // console.log(items);
        // 添加成员
        info.addEventListener('submit', function(e){
            // 阻止默认提交事件
            e.preventDefault()
            // 校验参数
            for (let i=0; i<items.length; i++) {
                if (items[i].value === '') {
                    return alert("不能留空！")
                }
            }
            arrs.push({
                stuId: arrs.length + 1,
                name: uname.value,
                age: age.value,
                gender: gender.value,
                salary: salary.value,
                city: city.value,
            })
            this.reset()
            render(arrs)
        })
        // 删除学员
        // 事件委托
        tbody.addEventListener('click', function(e){
            if (e.target.tagName == 'A') {
                arrs.splice(e.target.dataset.id, 1)
            }
            render(arrs)
        })
        // 渲染数组
        function render(arrs) {
            tbody.innerHTML = ''
            for (let i=0; i<arrs.length; i++) {
                const tr = document.createElement('tr')
                tr.innerHTML = `
                <td>${arrs[i].stuId}</td>
                <td>${arrs[i].name}</td>
                <td>${arrs[i].age}</td>
                <td>${arrs[i].gender}</td>
                <td>${arrs[i].salary}</td>
                <td>${arrs[i].city}</td>
                <td><a href="javascript:;" data-id=${i}>删除</a></td>
                `
                tbody.appendChild(tr)
            }
        }

    </script>
</body>
</html>
```

### 6. 支付成功跳转

```js
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>支付成功自动跳转</title>
    <style>
        span {
            color: red;
        }
    </style>
</head>
<body>
    <a href="https://www.baidu.com">支付成功<span>5</span>秒后之后自动跳转到首页</a>
    <script>
        let i = 5
        const a = document.querySelector('a')
        setInterval(function(){
            i--
            if (i <= 0) {
                // 跳转
                location.href = a.href
                return 
            }
            a.innerHTML = `支付成功<span>${i}</span>秒后之后自动跳转到首页`
        }, 1000)
    </script>
</body>
</html>
```

### 7. 过滤敏感词

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>过滤敏感词</title>
</head>
<body>
    <textarea name="" id="" cols="30" rows="10"></textarea>
    <button>发布</button>
    <span></span>
    <script>
        const tx = document.querySelector('textarea')
        const btn = document.querySelector('button')
        const span = document.querySelector('span')
        btn.addEventListener('click', function(){
            span.innerHTML = tx.value.replace(/激情|基情/g, '**')
            tx.value = ''
        })
    </script>
</body>
</html>
```

### 8. 注册校验

```js

```

### 9. 弹框封装

```html
<body>
    <button id="delete">删除</button>
    <button id="login">登录</button>
    <script>
        function Modal(title = '', message = '') {
            this.title = title
            this.message = message

            // 1. 创建一个盒子
            this.modalBox = document.createElement('div')
            // 2. 添加类名
            this.modalBox.className = 'modal'
            // 3. 给盒子添加内容
            this.modalBox.innerHTML = `
            <div class="header">${title} <i>x</i></div>
            <div class="body">${message}</div>
            `
            // console.log(this.modalBox)
        }

        // 打开方法
        Modal.prototype.open = function() {
            // 不存在才继续创建
            if (!document.querySelector('.modal')) {
                document.body.appendChild(this.modalBox)
                this.modalBox.querySelector('i').addEventListener('click', () => {
                    this.close()
                })
            }

        }

        // 关闭方法
        Modal.prototype.close = function() {
            document.body.removeChild(this.modalBox)
        }

        document.querySelector('#delete').addEventListener('click', () => {
            const m = new Modal('温馨提示', '您没有权限删除')
            m.open()
        })

        document.querySelector('#login').addEventListener('click', () => {
            const m = new Modal('友情提示', '您没有注册账号')
            m.open()
        })
    </script>
</body>
```
