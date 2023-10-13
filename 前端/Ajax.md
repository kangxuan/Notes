# 原生Ajax

### 1. GET请求

```html
<body>
    <button>点击发送请求</button>
    <div id="result"></div>
    <script>
        const btn = document.querySelector('button')
        const result = document.querySelector('#result')

        btn.addEventListener('click', function() {
            // 创建对象
            const xhr = new XMLHttpRequest()
            // 初始化请求
            xhr.open('GET', 'http://127.0.0.1:3000/')
            // 发送
            xhr.send()
            // 处理服务端返回的结果
            xhr.onreadystatechange = function() {
                // 判断服务端返回的所有结果
                // readystate 是 xhr对象的属性，表示状态 0 1 2 3 4
                // status 一般为200 404 403 401 500
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status < 300) {
                        result.innerHTML = xhr.response
                    }
                }
            }
        })
    </script>
</body>
```

### 2. POST请求

```html
<body>
    <div id="result"></div>
    <script>
        const result = document.querySelector('#result')
        result.addEventListener('mouseover', () => {
            const xhr = new XMLHttpRequest()
            xhr.open('POST', 'http://127.0.0.1:3000/post-server')
            xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
            xhr.setRequestHeader('name', 'shanla')
            xhr.send('a=100&b=200&c=300')

            // 事件绑定
            xhr.onreadystatechange = () => {
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status < 300) {
                        result.innerHTML = xhr.response
                    }
                }
            }
        })
    </script>
</body>
```

### 3. 响应JSON

```html
<body>
    <div id="result"></div>
    <script>
        const result = document.querySelector('#result')
        document.addEventListener('keydown', () => {
            const xhr = new XMLHttpRequest()
            // 设置响应体数据的类型
            xhr.responseType = 'json'
            xhr.open('GET', 'http://127.0.0.1:3000/json-server')
            xhr.send()

            xhr.onreadystatechange = () => {
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status < 300) {
                        result.innerHTML = xhr.response.name
                    }
                }
            }
        })
    </script>
</body>
```

### 4. 超时和错误处理

```html
<body>
    <button>点击发送请求</button>
    <div id="result"></div>
    <script>
        const btn = document.querySelector('button')
        const result = document.querySelector('#result')

        btn.addEventListener('click', () => {
            const xhr = new XMLHttpRequest()
            // 设置超时时间
            xhr.timeout = 500

            xhr.ontimeout = () => {
                alert('网络异常，请稍后再试')
            }
            xhr.onerror = () => {
                alert('出现错误')
            }

            xhr.open('GET', 'http://127.0.0.1:3000/delay')
            xhr.send()
            xhr.onreadystatechange = () => {
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status < 300) {
                        result.innerHTML = xhr.response
                    }
                }
            }
        })
    </script>
</body>
```

### 5. 重复请求处理

```html
<body>
    <button>点击发送</button>
    <script>
        const btn = document.querySelector('button')
        let x = null
        // 标识
        let isSending = false

        btn.addEventListener('click', () => {
            // 如果还在请求则阻止
            if (isSending) x.abort()
            x = new XMLHttpRequest()
            isSending = true
            x.open('GET', 'http://127.0.0.1:3000/delay')
            x.send()
            x.onreadystatechange = () => {
                if (x.readyState == 4) {
                    isSending = false
                }
            }
        })
    </script>
</body>
```

# jQuery-AJAX

```html
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jQuery发送Ajax请求</title>
    <link crossorigin="anonymous" href="https://cdn.bootcss.com/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script crossorigin="anonymous" src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
</head>
<body>
    <div class="container">
        <h2 class="page-header">jquery发送AJAX请求</h2>
        <button class="btn btn-primary">GET</button>
        <button class="btn btn-danger">POST</button>
        <button class="btn btn-info">通用方法AJAX</button>
    </div>
    <script>
        // get 请求
        $('button').eq(0).click(function(){
            $.get('http://127.0.0.1:3000/jquery-server', {a:100, b:200}, function(data) {
                console.log(data)
            }, 'json')
        })

        // post 请求
        $('button').eq(1).click(function(){
            $.post('http://127.0.0.1:3000/jquery-server', {a: 100, b:200}, function(data) {
                console.log(data)
            }, 'json')
        })

        // 通用AJAX
        $('button').eq(2).click(function(){
            $.ajax({
                // url
                url: 'http://127.0.0.1:3000/jquery-server',
                // 参数
                data: {a:100, b:200},
                // 请求类型
                type: 'GET',
                // 响应体格式
                dataType: 'json',
                // 成功响应
                success: function(data) {
                    console.log(data)
                },
                // 错误响应
                error: function() {
                    console.log('error')
                },
                // 设置请求头信息
                headers: {
                    c: 300,
                    d: 400,
                }
            })
        })
    </script>
</body>
</html>
```

# AXIOS-AJAX(常用)

```html
<body>
    <button>GET</button>
    <button>POST</button>
    <button>AJAX</button>
    <script>
        const btns = document.querySelectorAll('button')

        axios.defaults.baseURL = 'http://127.0.0.1:3000'

        // GET请求
        btns[0].addEventListener('click', () => {
            axios.get('/axios-server', {
                params: {
                    id: 100,
                    vip: 7
                },
                headers: {
                    name: 'shanla',
                    age: 20
                }
            }).then(value => {
                console.log(value)
            })
        })

        // POST请求
        btns[1].addEventListener('click', () => {
            axios.post('/axios-server', {
                username: 'admin',
                password: 'admin'
            }, {
                // url
                params: {
                    id: 200,
                    vip: 9
                },
                // 设置请求头
                headers: {
                    height: 100,
                    width: 100
                }
            })
        })

        // 通用AJAX请求
        btns[2].addEventListener('click', () => {
            axios({
                method: 'POST',
                url: '/axios-server',
                params: {
                    vip: 10,
                    level: 20,
                },
                headers: {
                    a: 100,
                    b: 200
                },
                data: {
                    username: 'admin',
                    password: 'admin'
                }
            }).then(response => {
                // 响应状态码
                console.log(response.status)
                // 响应状态字符串
                console.log(response.statusText)
                // 响应头信息
                console.log(response.headers)
                // 响应体
                console.log(response.data)
            })
        })
    </script>
</body>
```

# Fetch-AJAX(少用)

```html
<body>
    <button>AJAX请求</button>
    <script>
        const btn = document.querySelector('button')
        btn.addEventListener('click', () => {
            fetch('http://127.0.0.1:3000/fetch-server?vip=10', {
                // 请求方法
                method: 'POST',
                // 头部信息
                headers: {
                    name: 'atshanla'
                },
                // body内容
                body: 'username=admin&password=admin',
            }).then(response => {
                return response.json()
            }).then(response => {
                console.log(response)
            })
        })
    </script>
</body>
```

# Cors跨域处理

```js

```
