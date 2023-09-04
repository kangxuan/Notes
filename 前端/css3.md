# CSS3新增的东西

## 1. 新增的长度单位

- rem：根元素字体大小的倍数，只与根元素字体大小有关

- vw：视口宽度的百分之多少，比如`10vw`就是10%。

- vh：视口高度的百分之多少

- vmax：视口高宽中最大的值的百分之多少

- vmin：视口高度中最小的值的百分之多少

## 2. 新增的颜色设置方式

- rgba

- hsl

- hsla

## 3. 新增的选择器

动态伪类、目标伪类、语言伪类、 UI 伪类、结构伪类、否定伪类、伪元素。

## 4. 新增盒模型属性

- box-sizing
  
  - 作用：用来控制盒子大小的范围
  
  - 属性值选项
    
    - content-box：width和height设置的盒子内容区的大小（默认）
    
    - border-box：width和height设置的是盒子总大小（内容区+padding+border）

- resize
  
  - 作用：允许调整盒子大小
  
  - 属性值选项
    
    - none：不允许调整（默认）
    
    - both：用户可以调节元素的宽度和高度
    
    - horizontal：用户可以调节元素的宽度
    
    - vertical：用户可以调节元素的高度

```css
.box1 {
    width: 100px;
    height: 100px;
    background-color: blue;
    resize: both;
    overflow: hidden; /* 需要配合overflow */
}
```

- box-shadow
  
  - 作用：控制盒子阴影
  
  - 属性值选项
    
    - h-shadow：水平阴影的位置，必填可为负值。
    
    - v-shadow：垂直阴影的位置，必填可为负值。
    
    - blur：模糊距离，可选
    
    - spread：阴影的外延值，可选
    
    - color：阴影颜色，可选
    
    - inset：内部阴影，可选
  
  - 语法

```css
box-shadow: h-shadow v-shadow blur spread color inset;

/* 可以设置多个阴影效果 */
box-shadow:
                0 0 0 4px rgba(0,0,0,0.1),
                inset 0 0 0 3px #EFEFEF,
                inset 0 0 10px black,
                0 0 10px rgba(0,0,0,0.2);
```

```css
.box1 {
      width: 200px;
      height: 200px;
      background-color: cadetblue;
      /* 两个值：含义：水平位置 垂直位置 */
      /* box-shadow: 10px 10px; */

      /* 三个值：含义：水平位置 垂直位置 阴影颜色 */
      /* box-shadow: 10px 10px rebeccapurple; */

      /* 三个值：含义：水平位置 垂直位置 模糊程度 */
      /* box-shadow: 10px 10px 20px; */

      /* 四个值：含义：水平位置 垂直位置 模糊程度 阴影颜色 */
      /* box-shadow: 10px 10px 20px rebeccapurple; */

      /* 五个值：含义：水平位置 垂直位置 模糊程度 外延值 阴影颜色 */
      /* box-shadow: 10px 10px 20px 10px rebeccapurple; */

      /* 六个值：含义：水平位置 垂直位置 模糊程度 外延值 阴影颜色 内阴影 */
      box-shadow: 10px 10px 20px 10px rebeccapurple inset;
}
```

- opacity
  
  - 控制盒子的不透明度，从0-1越来越不透明。

## 5. 新增背景属性

- background-origin
  
  - 作用：设置背景图的原点
  
  - 属性值选项
    
    - padding-box：从padding区域开始显示背景图像，默认。
    
    - border-box：从border区域开始显示背景图像
    
    - content-box：从content区域开始显示背景图像

```css
.box1 {
      width: 400px;
      height: 400px;
      padding: 20px;
      border: 20px dotted black;
      background-color: #eee;

      background-image: url("../images/bg01.jpg");
      background-repeat: no-repeat;
      /* 从内容区开始 */
      /* background-origin: content-box; */
      /* 从边框开始 */
      /* background-origin: border-box; */
      /* 从padding开始 */
      background-origin: padding-box;
}
```

- background-clip
  
  - 作用：设置背景图的向外裁剪的区域
  
  - 属性值选项
    
    - border-box：从border区域开始向外的裁剪背景，默认
    
    - padding-box：从padding区域开始向外的裁剪背景
    
    - content-box：从content区域开始向外的裁剪背景
    
    - text：背景只呈现在文字上

```css
.box1 {
      width: 400px;
      height: 400px;
      padding: 20px;
      border: 20px dotted black;
      background-color: skyblue;
      font-size: 40px;
      font-weight: bold;
      background-image: url("../images/bg02.jpg");
      background-repeat: no-repeat;
      /* 从border区域外裁剪 */
      /* background-clip: border-box; */
      /* 从padding区域外裁剪背景 */
      /* background-clip: padding-box; */
      /* 从content区域外裁剪背景 */
      background-clip: content-box;
}
```

- background-size
  
  - 作用：设置图片背景的大小
  
  - 属性值选项
    
    - 用长度指定背景大小
    
    - 用百分比指定背景图片大小
    
    - auto：背景图片的真实大小，默认
    
    - contain：将背景图片等比缩放，使背景图片的宽或高，与容器的宽或高相等，再将完整背景图片包含在容器内，但要注意：可能会造成容器里部分区域没有背景图片
    
    - cover：将背景图片等比缩放，直到完全覆盖容器，图片会尽可能全的显示在元素上，但要注意：背景图片有可能显示不完整。—— 相对比较好的选择

```css
div {
      width: 400px;
      height: 400px;
      padding: 50px;
      border: 50px dashed rgba(255, 0, 0, 0.7);

      background-image: url('../images/bg03.jpg');
      background-repeat: no-repeat;
      /* 根据具体长度设置具体背景大小 */
      /* background-size: 400px 400px; */
      /* 根据百分比设置背景大小，这个百分比是盒子的总宽和高的百分比 */
      /* background-size: 100% 100%; */
      /* 按照图片真实大小，默认 */
      /* background-size: auto; */
      /* 将背景图片等比缩放，使背景图片的宽或高，与容器的宽或高相等，再将完整背景图片包含在容器内 */
      /* background-size: contain; */
      /* 将背景图片等比缩放，直到完全覆盖容器，图片会尽可能全的显示在元素上，但要注意：背景图片有可能显示不完整 */
      background-size: cover;
}
```

**实现多背景图**

CSS3允许设置多个背景图，语法如下：

```css
/* 添加多个背景图 */
background: url(../images/bg-lt.png) no-repeat left top,
            url(../images/bg-rt.png) no-repeat right top,
            url(../images/bg-lb.png) no-repeat left bottom,
            url(../images/bg-rb.png) no-repeat right bottom;
```

## 6. 新增边框属性

- border-radius
  
  - 作用：设置盒子的圆角
  
  - 同时设置4个圆角

```css
border-radius:10px;
```

- 分开设置4个圆角
  
  - border-top-left-radius：设置左上角圆角，一个值是正圆半径，两个值是椭圆的x和y半径。
  
  - border-top-right-radius：设置右上角圆角
  
  - border-bottom-left-radius：设置左下角圆角
  
  - border-bottom-right-radius：设置右下角圆角

- 分开设置每个角的圆角，综合写法

```css
border-raidus: 左上角x 右上角x 右下角x 左下角x / 左上y 右上y 右下y 左下y
```

```css
.box1 {
      width: 400px;
      height: 400px;
      background-color: skyblue;

      /* 常用 */
      border-radius: 10px;
      /* 设置成圆形 */
      /* border-radius: 50%; */
      /* border-radius: 200px; */

      /* 分别设置四个角为不同的圆角 */
      /* border-top-left-radius: 100px;
      border-top-right-radius: 50px;
      border-bottom-right-radius: 20px;
      border-bottom-left-radius: 10px; */

      /* 分别设置四个角为不同的椭圆角 */
      /* border-top-left-radius: 100px 50px;
      border-top-right-radius: 50px 20px;
      border-bottom-right-radius: 20px 10px;
      border-bottom-left-radius: 10px 5px; */

      /* 综合设置 */
      /* border-radius:100px 50px 20px 10px / 50px 20px 10px 5px; */
}
```

- outline
  
  - 作用：设置边框外轮廓，理解成盒子外面的光，不占位
  
  - 只记综合属性

```css
outline:20px solid orange;
```

## 7. 新增的文本属性

- text-shadow
  
  - 作用：给文本增加阴影
  
  - 语法

```css
/* h-shadow:水平阴影的位置；v-shadow:垂直阴影的位置; blur: 模糊距离；color：阴影颜色 */
text-shadow: h-shadow v-shadow blur color;

body {
    background-color: black;
}
h1 {
    font-size: 80px;
    color: white;
    /* text-shadow: 3px 3px; */
    /* text-shadow: 3px 3px red; */
    /* text-shadow: 3px 3px 10px red; */
    /* text-shadow: 0px 0px 15px black; */
    text-shadow: 0px 0px 40px red;
    font-family: '翩翩体-简';
}
```

- white-space
  
  - 作用：设置文本换行方式
  
  - 属性值选项
    
    - normal：文本超出边界自动换行，文本中的换行被浏览器识别为一个空格。（默认值）
    
    - pre：原样输出，与 pre 标签的效果相同
    
    - pre-wrap：在 pre 效果的基础上，超出元素边界自动换行
    
    - pre-line：在 pre 效果的基础上，超出元素边界自动换行，且只识别文本中的换行，空格会忽略
    
    - nowrap：强制不换行

- text-overflow
  
  - 作用：设置文本溢出时的呈现方式
  
  - 属性值选项
    
    - clip：当内联内容溢出时，将溢出部分裁切掉。 （默认值）
    
    - ellipsis：当内联内容溢出块容器时，将溢出部分替换为 ... 。
  
  - 注意：
    
    - 设置elipsis时，需要配合`overflow:hidden`或其他非visible使用

```css
ul {
    width: 400px;
    height: 400px;
    border: 1px solid black;
    font-size: 20px;
    list-style: none;
    padding-left: 0;
    padding: 10px;
}
li {
    margin-bottom: 10px;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}
```

- text-decoration
  
  - 作用：文字修饰，CSS3升级了该属性将该属性变成了复合属性
  
  ```css
  text-decoration: text-decoration-line || text-decoration-style || text-decoration- color
  ```

## 8. 新增渐变

**线性渐变**

- 默认情况，从上到下渐变

- 使用关键词设置渐变色的方向

- 使用角度设置渐变色的方向

- 调整渐变色起始的位置

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>线性渐变</title>
    <style>
        div {
            width: 300px;
            height: 200px;
            border: 1px solid black;
            float: left;
            margin-left: 50px;
            font-size: 20px;
        }
        .box1 {
            /* 默认情况从上到下 */
            background-image: linear-gradient(red, yellow, green);
        }
        .box2 {
            /* 从下到上 */
            /* background-image: linear-gradient(to top, red, yellow, green); */
            /* 从左下到右上 */
            background-image: linear-gradient(to top right, red, yellow, green);
        }
        .box3 {
            /* 使用角度设置渐变 */
            background-image: linear-gradient(30deg, red, yellow, green);
        }
        .box4 {
            /* 调整渐变色起始的位置 */
            background-image: linear-gradient(red 50px, yellow 100px, green 150px);
        }
    </style>
</head>
<body>
    <div class="box1">默认情况从上到下</div>
    <div class="box2">使用关键词设置线性渐变的方向</div>
    <div class="box3">使用角度设置线性渐变的方向</div>
    <div class="box4">调整开始渐变的位置</div>
</body>
</html>
```

**径向渐变**

- 默认情况，从圆心四散（圆可能是正圆，也可能是椭圆，根据具体的形状）

- 使用关键词调整渐变圆的圆心位置

- 使用像素值调整渐变圆的圆心位置

- 调整渐变形状为正圆

- 调整形状的半径

- 调整颜色开始渐变的位置

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>径向渐变</title>
</head>
<style>
    div {
        width: 300px;
        height: 200px;
        border: 1px solid black;
        float: left;
        margin-left: 50px;
        font-size: 20px;
    }
    .box1 {
        background-image: radial-gradient(red, yellow, green);
    }
    .box2 {
        /* 设置圆心位置在右上角 */
        background-image: radial-gradient(at right top, red, yellow, green);
    }
    .box3 {
        /* 设置圆心位置在x轴100px，y轴50px的 */
        background-image: radial-gradient(at 100px 50px, red, yellow, green);
    }
    .box4 {
        /* 设置渐变形状为正圆 */
        background-image: radial-gradient(circle, red, yellow, green);
    }
    .box5 {
        /* 调整渐变形状为一个半径100px的圆 */
        /* background-image: radial-gradient(100px, red, yellow, green); */
        /* 调整渐变形状为一个x半径为100px，y半径为50px的椭圆 */
        background-image: radial-gradient(100px 50px, red, yellow, green);
    }
    .box6 {
        /* 调整渐变颜色开始的位置 */
        background-image: radial-gradient(red 50px, yellow 100px, green 150px);
    }
</style>
<body>
    <div class="box1">默认情况，从圆心四散</div>
    <div class="box2">根据关键词设置圆心的位置</div>
    <div class="box3">使用像素值调整渐变圆的圆心位置</div>
    <div class="box4">调整渐变形状为正圆</div>
    <div class="box5">调整渐变形状的半径</div>
    <div class="box6">调整渐变颜色开始的位置</div>
</body>
</html>
```

**重复渐变**

无论线性渐变，还是径向渐变，在没有发生渐变的位置，继续进行渐变，就为重复渐变。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>重复渐变</title>
    <style>
        div {
            width: 300px;
            height: 200px;
            border: 1px solid black;
            float: left;
            margin-left: 50px;
            font-size: 20px;
        }
        .box1 {
            background-image: repeating-linear-gradient(red 50px, yellow 100px, green 150px);
        }
        .box2 {
            background-image: repeating-radial-gradient(red 50px,yellow 100px,green 150px);
        }
    </style>
</head>
<body>
    <div class="box1">线性渐变的重复渐变</div>
    <div class="box2">径向渐变渐变的重复渐变</div>
</body>
</html>
```

## 9. 新增web字体

**使用在线的web字体**

通过引入在线的字体，用户就不需要手动安装字体，而是访问时下载提供的字体，就可以浏览网站了。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>web字体</title>
    <style>
        /* 简单使用 */
        /* 先定义字体，这个字体是放到服务器中 */
        @font-face {
            font-family: "情书字体";
            src: url('./font1/方正手迹.ttf');
        }
        /* 使用自定义的字体 */
        .t1 {
            font-family: "情书字体";
        }
        /* 高兼容写法 */
        @font-face {
            font-family: "情书字体2";
            font-display: swap;
            src: url('./font2/webfont.eot'); /* IE9 */
            src: url('./font2/webfont.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
                url('./font2/webfont.woff2') format('woff2'),
                url('./font2/webfont.woff') format('woff'), /* chrome、firefox */
                url('./font2/webfont.ttf') format('truetype'), /* chrome、firefox、opera、Safari, Android, iOS 4.2+*/
                url('./font2/webfont.svg#webfont') format('svg'); /* iOS 4.1- */
        }
        .t2 {
            font-family: "情书字体2";
        }
    </style>
</head>
<body>
    <h1 class="t1">春风得意马蹄疾，不信人间有别离</h1>
    <h1 class="t2">春风得意马蹄疾，不信人间有别离</h1>
</body>
</html>
```

缺点：

1. 字体文件太大，会导致每个用户都会下载，对网络条件不好的用户不够友好

2. 字体文件太大，会导致服务器压力增大。

**定制字体**

因为字体文件太大，针对单独几个字进行定制，一般使用[阿里Web定制工具](%5Biconfont-webfont%E5%B9%B3%E5%8F%B0%5D(https://www.iconfont.cn/webfont#!/webfont/index))或其他平台工具。

**字体图标**

将图标制作成文字，这样就不会有图标不清晰的问题

优势：

1. 相比图片更加清晰

2. 灵活性高，更方便改变大小、颜色、风格等

3. 兼容性好， IE 也能支持

常用的平台：[阿里iconfont](https://www.iconfont.cn/)

## 10. 新增2D变换

### 10.1 2D位移

2D位移可以改变元素的位置，在X轴和Y轴上移动。

- transform
  
  - 属性值选项
    
    - translateX：设置在X轴的偏移量，如果指定百分比，这是自身宽度的百分比
    
    - translateY：设置在Y轴的偏移量，如果指定百分比，这是自身高度的百分比
    
    - translate：一个值代表水平方向，两个值代表：水平和垂直方向

注意点：

1. 位移与相对定位很相似，都不脱离文档流，不会影响到其它元素

2. 与相对定位的区别：相对定位的百分比值，参考的是其父元素；定位的百分比值，参考的是其自身

3. 浏览器针对位移有优化，与定位相比，浏览器处理位移的效率更高

4. transform 可以链式编写

5. 位移对行内元素无效

6. 位移配合定位，可实现元素水平垂直居中

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>2D位移</title>
    <style>
        .outer {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            margin: 0 auto;
            margin-top: 100px;
            position: relative;
        }
        .inner {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            /* 水平位移 */
            /* transform: translateX(10px); */
            /* 垂直位移 */
            /* transform: translateY(50px); */
            /* 水平+垂直位移 */
            transform: translate(50%, 50%);
        }
        /* 结合绝对定位完成元素垂直居中 */
        .inner2 {
            width: 60px;
            height: 60px;
            background-color: deepskyblue;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
    </style>
</head>
<body>
    <div class="outer">
        <div class="inner">你好</div>
    </div>

    <div class="outer">
        <div class="inner2">你好</div>
    </div>
</body>
</html>
```

### 10.2 2D缩放

2D缩放可以放大缩小元素

- transform
  
  - 属性值选项
    
    - scaleX：设置水平方向的缩放比例，值为一个数字，1表示不缩放，大于1放大，小于1缩小
    
    - scaleY：设置垂直方向的缩放比例，值为一个数字，1表示不缩放，大于1放大，小于1缩小。
    
    - scale：同时设置水平方向、垂直方向的缩放比例，一个值代表同时设置水平和垂直缩放；两个值分别代表：水平缩放、垂直缩放

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>2D缩放</title>
    <style>
        .outer {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            margin: 0 auto;
            margin-top: 100px;
            position: relative;
        }
        .inner {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            /* 水平方向放大2倍 */
            transform: scaleX(2);
            /* 垂直方向上缩小1倍 */
            transform: scaleY(0.5);
        }
        /* 将字体缩得很小 */
        span {
            display: inline-block;
            font-size: 20px;
            transform: scale(0.5);
        }
    </style>
</head>
<body>
    <div class="outer">
        <div class="inner">你好</div>
    </div>
    <span>好</span>
</body>
</html>
```

### 10.3 2D旋转

2D旋转可以让元素在顺时针很逆时针上旋转

- transform
  
  - 属性值选项
    
    - rotate：设置旋转角度，需指定一个角度值( deg )，正值顺时针，负值逆时针

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>2D旋转</title>
    <style>
        .outer {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            margin: 0 auto;
            margin-top: 100px;
        }
        .inner {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            transform: rotate(30deg);
        }
    </style>
</head>
<body>
    <div class="outer">
        <div class="inner">你好</div>
    </div>
</body>
</html>
```

### 10.4 多重变换

也就是2D位移、2D缩放、2D旋转一起使用。

注意：

1. 建议旋转写在最后，因为旋转之后会改变坐标系。

### 10.5 变换原点

- transform-origin：更改一个元素变形的原点
  
  - 属性值选项
    
    - `transform-origin: 50% 50%`：变换原点在元素的中心位置，百分比是相对于自身。
    
    - `transform-origin: left top`：变换原点在元素的左上角
    
    - `transform-origin: 50px 50px`：变换原点距离元素左上角 50px 50px 的位置
    
    - `transform-origin: 0`：只写一个值的时候，第二个值默认为 50%。

注意：

1. 元素变换时，默认的原点是元素的中心，使用 transform-origin 可以设置变换的原点

2. 修改变换原点对位移没有影响， 对旋转和缩放会产生影响

3. 如果提供两个值，第一个用于横坐标，第二个用于纵坐标

4. 如果只提供一个，若是像素值，表示横坐标，纵坐标取 50% ；若是关键词，则另一个坐标取 50%

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>05_多重变换</title>
    <style>
        .outer {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            margin: 0 auto;
            margin-top: 100px;
        }
        .inner {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            /* 通过关键词调整变换原点 */
            /* transform-origin: right bottom; */

            /* 通过具体像素值调整变换原点 */
            /* transform-origin: 50px 50px; */

            /* 通过百分比调整变换原点 */
            /* transform-origin: 25% 25%; */

            /* 只给一个值 */
            /* transform-origin:top; */

            /* transform-origin: right top; */

            /* 变换原点位置的改变对 旋转 有影响 */
            /* transform: rotate(0deg); */

            /* 变换原点位置的改变对 缩放 有影响 */
            /* transform: scale(1.3); */

            /* 变换原点位置的改变对 位移 没有影响 */
            /* transform: translate(100px,100px) */
        }
        .test {
            width: 50px;
            height: 100px;
            background-color: orange;
        }
    </style>
</head>
<body>
    <div class="outer">
        <div class="inner">
            <div class="test">你好啊</div>
        </div>
    </div>
</body>
</html>
```

## 11. 新增3D变换

### 11.1 开启3D空间

元素进行3d变换，必须先给父元素开启3d空间

- transform-style
  
  - 作用：开启3d空间
  
  - 属性值选项
    
    - flat：让子元素位于此元素的二位平面（2d）内，默认
    
    - reserve-3d：让子元素位于此元素的三维空间内（3d）

### 11.2 设置景深

如果元素要进行3d变换，必须先给父元素设置景深值

**什么是景深**

指定观察者与 z=0 平面的距离，能让发生 3D 变换的元素，产生透视效果，看来更加立

体

- perspective
  
  - 作用：设置景深
  
  - 属性值选项
    
    - none：不指定透视，默认
    
    - 长度：指定观察者距离 z=0 平面的距离，不允许负值

### 11.3 透视点位置

所谓透视点位置，就是观察者位置；默认的透视点在元素的中心

- perspective-origin
  
  - 作用：设置观察者位置
  
  - 两个值：`perspective-origin: 400px 300px;`相对坐标轴往右偏移400px， 往下偏移300px（相当于人蹲下300像素，然后向右移动400像素看元素）
  
  - 用得较少

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>3D变换和景深</title>
    <style>
        .outer {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            margin: 0 auto;
            margin-top: 100px;
            /* 父元素必须开启3d */
            transform-style: preserve-3d;
            /* 开启景深，也就是要离该元素多远才能看到3d效果 */
            perspective: 500px;
            /* 设置透视点的位置 */
            perspective-origin: 102px 102px;
        }
        .inner {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            /* 在X轴上旋转30度 */
            transform: rotateX(30deg);
        }

    </style>
</head>
<body>
    <div class="outer">
        <div class="inner">内容区</div>
    </div>
</body>
</html>
```

### 11.4 3D位移

3D位移比2D位移多了一个Z轴上的位移，因为元素没有厚度，所以Z轴上的位移变现为走近走远的感觉

- transform
  
  - 3D位移相关选项
    
    - translateZ：设置 z 轴位移，需指定长度值，正值向屏幕外，负值向屏幕里，且不能写百分比
    
    - translate3d：第1个参数对应 x 轴，第2个参数对应 y 轴，第3个参数对应 z 轴，且均不能省略

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>3D位移</title>
    <style>
        .outer {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            margin: 0 auto;
            margin-top: 100px;
            /* 父元素必须开启3d */
            transform-style: preserve-3d;
            /* 开启景深，也就是要离该元素多远才能看到3d效果 */
            perspective: 500px;
        }
        .inner {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            /* Z轴上位移，就是元素向我们走近的感觉，Z轴不能设置百分比，因为元素没有厚度 */
            /* transform: translateZ(150px); */
            /* X轴，Y轴，Z轴三个方向上移动，Z轴不能设置百分比 */
            transform: translate3d(50%, 50%, 10px);
        }

    </style>
</head>
<body>
    <div class="outer">
        <div class="inner">内容区</div>
    </div>
</body>
</html>
```

### 11.5 3D旋转

3D旋转是让元素可以在X轴、Y轴上旋转

- transform
  
  - 3D旋转的选项值
    
    - rotateX：设置 x 轴旋转角度，需指定一个角度值( deg )，面对 x 轴正方向：正值顺时针，负值逆时针。
    
    - rotateY：设置 y 轴旋转角度，需指定一个角度值( deg )，面对 y 轴正方向：正值顺时针，负值逆时针。
    
    - rotate3d：前 3 个参数分别表示坐标轴： x , y , z ，第 4 个参数表示旋转的角度，参数不允许省略

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>3D位移</title>
    <style>
        .outer {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            margin: 0 auto;
            margin-top: 100px;
            /* 父元素必须开启3d */
            transform-style: preserve-3d;
            /* 开启景深，也就是要离该元素多远才能看到3d效果 */
            perspective: 500px;
        }
        .inner {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            /* 在X轴上顺时针旋转30度 */
            /* transform: rotateX(30deg); */
            /* 在Y轴上顺时针旋转40度 */
            /* transform: rotateY(40deg); */
            /* 在XYZ轴坐标上分别顺时针旋转30度，0表示不在，1表示在 */
            transform: rotate3d(1,1,1,30deg);
        }

    </style>
</head>
<body>
    <div class="outer">
        <div class="inner">内容区</div>
    </div>
</body>
</html>
```

### 11.6 3D缩放

在2D基础上，增加了Z轴上的缩放，Z轴缩放需要加一个旋转才看得出来

- transform
  
  - 有关3D缩放的选项值
    
    - scaleZ：在Z轴方向的缩放比例，值为一个数字，1表示不缩放，大于1放大，小于1缩小
    
    - scale3d：第1个参数对应 x 轴，第2个参数对应y轴，第3个参数对应z轴，参数不允许省略

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>3D缩放</title>
    <style>
        .outer {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            margin: 0 auto;
            margin-top: 100px;
            /* 父元素必须开启3d */
            transform-style: preserve-3d;
            /* 开启景深，也就是要离该元素多远才能看到3d效果 */
            perspective: 500px;
        }
        .inner {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            /* 单纯在Z轴上缩放是看不出来的，要加一个旋转才能看得出来 */
            /* transform: scaleZ(4) rotateY(30deg); */
            transform: scale3d(1.5, 1.5, 1) rotate(45deg);
        }

    </style>
</head>
<body>
    <div class="outer">
        <div class="inner">内容区</div>
    </div>
</body>
</html>
```

### 11.7 多重变换

```css
transform: translateZ(100px) scaleZ(3) rotateY(40deg);
```

注意：建议把旋转卸载最后

### 11.8 背部可见性

设置在背向元素时是否可见

- backface-visibility
  
  - 属性值选项
    
    - visible：可见，默认
    
    - hidden：背部可不见

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>背部可见性</title>
    <style>
        .outer {
            width: 200px;
            height: 200px;
            border: 2px solid black;
            margin: 0 auto;
            margin-top: 100px;
            /* 父元素必须开启3d */
            transform-style: preserve-3d;
            /* 开启景深，也就是要离该元素多远才能看到3d效果 */
            perspective: 500px;
        }
        .inner {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            /* 绕Y轴旋转到背面是不会被看见 */
            transform: rotateY(0deg);
            backface-visibility: hidden;
        }

    </style>
</head>
<body>
    <div class="outer">
        <div class="inner">内容区</div>
    </div>
</body>
</html>
```

## 新增过渡

过渡将元素从一个样式过渡到另一个样式

- transition-property
  
  - 作用：用于控制哪些属性可以过渡，一般有值或可以转换成值的都可以过渡
  
  - 属性值选项
    
    - `transition-property: width, height, color`允许单个或多个属性
    
    - `transition-property: all`：允许所有属性可以过渡的属性

- transition-duration
  
  - 作用：用于控制过渡的时间，秒
  
  - 属性值选项
    
    - `transition-duration: 1s`：控制过渡时间为1s

- transition-delay
  
  - 作用：控制过渡开始的延迟时间，秒

- transition-timing-function
  
  - 作用：用于控制过渡的类型
  
  - 属性值选项
    
    - ease：平滑过渡，默认
    
    - linear：线性过渡
    
    - ease-in：慢到快
    
    - ease-out：快到慢
    
    - ease-in-out：慢到快再到慢
    
    - steps( integer,?)：接受两个参数的步进函数。第一个参数必须为正整数，指定函数的步数。第二个参数取值可以是 start 或 end ，指定每一步的值发生变化的时间点。第二个参数默认值为 end
    
    - step-start：等同于 steps(1, start)，不考虑过渡的时间，直接就是终点
    
    - step-end：等同于 steps(1, end)，考虑过渡时间，但无过渡效果，过渡时间到了以后，瞬间到达终点
    
    - cubic-bezie ( number, number, number, number)：特定的贝塞尔曲线类型

- 复合属性
  
  - 如果设置了一个时间，表示 duration ；如果设置了两个时间，第一是 duration ，第二个是delay ；其他值没有顺序要求。
  
  - `transition:1s 1s linear all;`

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>高级使用</title>
    <style>
        .outer {
            width: 900px;
            height: 900px;
            border: 1px solid black;
        }
        .outer:hover .box {
            width: 900px;
        }
        .box {
            width: 100px;
            height: 100px;
            transition-property: all;
            transition-duration: 1s;
        }
        .box1 {
            background-color: skyblue;
            transition-timing-function: ease;
        }
        .box2 {
            background-color: orange;
            transition-timing-function: linear;
        }
        .box3 {
            background-color: gray;
            transition-timing-function: ease-in;
        }
        .box4 {
            background-color: tomato;
            transition-timing-function: ease-out;
        }
        .box5 {
            background-color: green;
            transition-timing-function: ease-in-out;
        }
        .box6 {
            background-color: purple;
            transition-timing-function: step-start;
        }
        .box7 {
            background-color: deeppink;
            transition-timing-function: step-end;
        }
        .box8 {
            background-color: chocolate;
            transition-timing-function: steps(10, start);
        }
        .box9 {
            background-color: darkseagreen;
            transition-timing-function: cubic-bezier(1,-0.01,.23,1.55);
        }
    </style>
</head>
<body>
    <div class="outer">
        <div class="box box1">ease（平滑的慢->快->慢）</div>
        <div class="box box2">linear（均速）</div>
        <div class="box box3">ease-in（快->慢）</div>
        <div class="box box4">ease-out（慢->快）</div>
        <div class="box box5">ease-in-out（慢->快->慢）</div>
        <div class="box box6">step-start（不考虑过渡时间直接到终点）</div>
        <div class="box box7">step-end（考虑过渡时间，等过渡时间结束后，直接到终点）</div>
        <div class="box box8">steps（分布过渡）</div>
        <div class="box box9">cubic-bezie（贝塞尔曲线）</div>
    </div>
</body>
</html>
```

## 新增动画

**什么是帧**

一段动画，就是一段时间内连续播放 n 个画面。每一张画面，我们管它叫做“帧”。一定时间内连续快速播放若干个帧，就成了人眼中所看到的动画。同样时间内，播放的帧数越多，画面看起来越流畅。

**什么是关键帧**

关键帧指的是，在构成一段动画的若干帧中，起到决定性作用的 2-3 帧。

- animation-name
  
  - 作用：给元素指定具体的动画

- animation-duration
  
  - 作用：设置动画所需时间

- animation-delay
  
  - 作用：设置动画延迟

- animation-timing-function
  
  - 作用：设置动画的类型
  
  - 属性值选项：和过渡一样

- animation-iteration-count
  
  - 作用：指定动画播放的次数
  
  - 属性值选项
    
    - number：具体次数
    
    - infinite：无限循环

- animation-direction
  
  - 作用：指定动画方向
  
  - 属性值选项
    
    - normal：正常方向，默认
    
    - reverse：反方向
    
    - alternate：先正常方向，然后再反方向
    
    - alternate-reverse：动画先反运行再正方向运行，并持续交替运行

- animation-fill-mode
  
  - 作用：设置动画之外的状态
  
  - 属性值选项
    
    - forwards：设置对象状态为动画结束时的状态
    
    - backwards：设置对象状态为动画开始时的状态

- animation-play-state
  
  - 作用：设置动画的播放状态
  
  - 属性值选项
    
    - running：运动
    
    - paused：暂停

- 复合属性
  
  - 只设置一个时间表示 duration ，设置两个时间分别是： duration 和 delay ，其他属性没有数量和顺序要求

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>03_复合属性</title>
    <style>
        .outer {
            width: 1000px;
            height: 100px;
            border: 1px solid black;
        }
        @keyframes xiangyoudong {
            from {

            }
            to {
                transform: translate(900px) rotate(360deg);
                background-color: purple;
                border-radius: 50%;
            }
        }
        @keyframes xiangyoudong2 {
            0% {

            }
            100% {
                transform: translate(900px) rotate(360deg);
                background-color: purple;
                border-radius: 50%;
            }
        }
        .inner {
            width: 100px;
            height: 100px;
            background-color: deepskyblue;
            /* 应用动画到元素 */
            /* animation-name: xiangyoudong; */
            /* 动画持续的时间 */
            /* animation-duration: 3s; */
            /* 动画延迟时间 */
            /* animation-delay: 0.2s; */

            /* ********其他属性--start*********** */
            /* 设置动画的方式 */
            /* animation-timing-function: linear; */

            /* 动画播放的次数 */
            /* animation-iteration-count: infinite; */

            /* 动画的方向 */
            /* animation-direction: alternate; */

            /* 动画以外的状态（不发生动画的时候在哪里） */
            /* animation-fill-mode: forwards; */

            /* 复合属性 */
            animation: xiangyoudong2 3s linear 0.2s infinite alternate forwards;
        }
        .outer:hover .inner {
            /* 动画的播放状态 */
            animation-play-state: paused;
        }

    </style>
</head>
<body>
    <div class="outer">
        <div class="inner"></div>
    </div>
</body>
</html>
```

## 过渡和动画的区别

- 过渡：过渡需要触发条件

- 动画：不需要触发条件，比过渡有更多的表达，可以设置多个帧

## 多列布局

用来专门做类似报纸样式的布局

- column-count：指定列数

- column-width：指定列宽

- columns：同时指定count和width的复合属性

- column-gap：设置列边距

- column-rule-style：设置列与列之间边框的风格，和border-style类似

- column-rule-width：设置列与列之间边框的宽度

- column-rule-color：设置列与列之间边框的颜色

- coumn-rule：设置列边框，复合属性

- column-span：是否跨列
  
  - 属性值选项
    
    - none：不跨列，默认
    
    - all：跨所有列

## 伸缩盒模型

### 1. 伸缩盒模型简介

- 2009年，W3C提出了一种新的盒子模型 —— Flexible Box （伸缩盒模型，又称：弹性盒子）

- 它可以轻松的控制：元素分布方式、元素对齐方式、元素视觉顺序...

- 截止目前，除了在部分 IE 浏览器不支持，其他浏览器均已全部支持

- 伸缩盒模型的出现，逐渐演变出了一套新的布局方案 —— flex布局

### 2. 伸缩容器、伸缩项目

**伸缩容器**

开启了flex的元素就是伸缩容器

- display
  
  - 属性值选项
    
    - flex
    
    - inline-flex：可以让容器变成行内块，并排在一起

**伸缩项目**

被伸缩容器包裹的子元素就是伸缩项目

- 仅伸缩容器的子元素才是伸缩项目，孙子元素、重孙元素等后代都不是伸缩项目

- 无论原来是什么元素（行内、行内块、块），一旦成为伸缩项目，都会”块状化“，可以设置宽高等等。

### 3. 主轴和侧轴

**主轴**

伸缩项目沿着主轴排列，主轴默认是水平的，默认方向是：从左到右。

**侧轴**

与主轴垂直的就是侧轴，侧轴默认是垂直的，默认方向是：从上到下。

### 4. 属性

- flex-direction
  
  - 作用：设置主轴方向
  
  - 属性值选项
    
    - row：主轴方向水平从左到右，默认值
    
    - row-reverse：主轴方向水平从右到左
    
    - column：主轴方向垂直从上到下
    
    - column-reverse：主轴方向垂直从下到上

- flex-wrap
  
  - 作用：设置主轴换行方式
  
  - 属性值选项
    
    - nowrap：不换行，默认值
    
    - wrap：主动换行
    
    - wrap-reverse：反向换行

- flex-flow
  
  - 作用：是fiex-direction和flex-wrap的复合属性
  
  - 写法：`flex-flow: row wrap`

- justify-content
  
  - 作用：主轴对齐方式
  
  - 属性值选项
    
    - flex-start：主轴起点对齐，默认值
    
    - flex-end：主轴终点对齐
    
    - center：居中对齐
    
    - space-between：均匀分布，两端对齐
    
    - space-around：均匀分布，两端距离是中间距离的一半
    
    - space-evenly：均匀分布，两端距离与中间距离一致

- align-items
  
  - 作用：侧轴对齐方式（只有一行的情况）
  
  - 属性值选项
    
    - flex-start：侧轴的起点对齐
    
    - flex-end：侧轴的终点对齐
    
    - center：侧轴的中点对齐
    
    - baseline：伸缩项目的第一行文字的基线对齐
    
    - stretch：如果伸缩项目未设置高度，将占满整个容器的高度（没有设置height时），默认值

- align-content
  
  - 作用：侧轴对齐方式（多行的情况）
  
  - 属性值选项
    
    - flex-start：与侧轴的起点对齐
    
    - flex-end：与侧轴的终点对齐
    
    - center：与侧轴的中点对齐
    
    - space-between：与侧轴两端对齐，中间平均分布
    
    - space-around：伸缩项目间的距离相等，比距边缘大一倍
    
    - space-evenly：在侧轴上完全平分
    
    - stretch：占满整个侧轴（没有设置height时），默认值

- flex-basis
  
  - 作用：设置主轴方向上的基准长度，会让宽或高失效，浏览器根据这个属性设置的值，计算主轴上是否有多余空间，默认值 auto ，即：伸缩项目的宽或高
  
  - 备注：主轴横向：宽度失效；主轴纵向：高度失效

- flex-grow
  
  - 作用：伸缩项目的放大比例，默认为0，即：纵使主轴存在剩余空间，也不拉伸（放大）。
  
  - 规则：
    
    - 若所有伸缩项目的 flex-grow 值都为 1 ，则它们将等分剩余空间。
    
    - 若三个伸缩项目的 flex-grow 值分别为： 1 、 2 、 3 ，则：分别瓜分到： 1/6 、 2/6 、3/6 的空间。

- flex-shrink
  
  - 作用：定义了项目的压缩比例，默认为 1 ，即：如果空间不足，该项目将会缩小。
  
  - 规则
    
    - 比如2个伸缩项目，宽度分别200px、300px、200px，flex-shrink设置为1，2，3，那么先计算出分母=200×1+300×2+200×3=1400，然后再计算比例，A的比例=(200×1) / 1400，同理算出B和C的比例，最终收缩=总的少的距离×各自的比例。

- flex
  
  - 作用：是复合flex-basis、flex-grow、flex-shrink三个属性
  
  - 写法：
    
    - `flex:1 1 auto`：可以拉伸 可以压缩 不设置基准长度，可简写为：flex:auto
    
    - `flex:1 1 0`：可以拉伸 可以压缩 设置基准长度为0，可简写为：flex:1
    
    - `flex:0 0 auto`：不可以拉伸 不可以压缩 不设置基准长度，可简写为：flex:none
    
    - `flex: 0 1 auto;`：不可以拉伸 可以压缩 不设置基准长度，可简写为：flex:0 auto

- order
  
  - 作用：定义项目的排列顺序。数值越小，排列越靠前，默认为 0

- align-self
  
  - 作用：单独调整某个伸缩项目的对齐方式

## 新增响应式布局

### 1. 媒体查询-媒体类型

| 值      | 含义                        |
| ------ | ------------------------- |
| all    | 检测所有设备                    |
| screen | 检测电子屏幕，包括：电脑屏幕、平板屏幕、手机屏幕等 |
| print  | 检测打印机                     |

```css
@media print {
    h1 {
        background: transparent;
    }
}
/* 只有在屏幕上才应用的样式 */
@media screen {
    h1 {
        font-family: "翩翩体-简";
    }
}
/* 一直都应用的样式 */
@media all {
    h1 {
        color: red;
    }
}
```

### 2. 媒体查询-媒体特性

| 值                | 含义                                                           |
| ---------------- | ------------------------------------------------------------ |
| width            | 检测视口宽度                                                       |
| max-width        | 检测视口最大宽度                                                     |
| min-width        | 检测视口最小宽度                                                     |
| height           | 检测视口高度                                                       |
| max-height       | 检测视口最大高度                                                     |
| min-height       | 检测视口最小高度                                                     |
| device-width     | 检测设备屏幕的宽度                                                    |
| max-device-width | 检测设备屏幕的最大宽度                                                  |
| min-device-width | 检测设备屏幕的最小宽度                                                  |
| orientation      | 检测视口的旋转方向，portrait：视口处于纵向，即高度大于等于宽度；landscape：视口处于横向，即宽度大于高度 |

```css
/* 检测到视口的宽度为800px时，应用如下样式 */
@media (width:800px) {
    h1 {
        background-color: green;
    }
}

/* 检测到视口的宽度小于等于700px时，应用如下样式 */
@media (max-width:700px) {
    h1 {
        background-color: orange;
    }
}

/* 检测到视口的宽度大于等于900px时，应用如下样式 */
@media (min-width:900px) {
    h1 {
        background-color: deepskyblue;
    }
}

/* 检测到视口的高度等于800px时，应用如下样式 */
/* @media (height:800px){
    h1 {
        background-color: yellow;
    }
} */

/* 检测到屏幕的宽度等于1536px时，应用如下样式 */
/* @media (device-width:1536px) {
    h1 {
        color: white;
    }
} */
```

### 3. 运算符

| 值      | 含义  |
| ------ | --- |
| and    | 且   |
| , 或 or | 或   |
| not    | 非   |
| only   | 只是  |

```css
/* 且运算符 */
/* @media (min-width:700px) and (max-width:800px) {
    h1 {
        background-color: orange;
    }
} */
/* @media screen and (min-width:700px) and (max-width:800px) {
    h1 {
        background-color: orange;
    }
} */

/* 或运算符 */
/* @media screen and (max-width:700px) or (min-width:800px) {
    h1 {
        background-color: orange;
    }
} */

/* 否定运算符 */
/* @media not screen {
    h1 {
        background-color: orange;
    }
} */

/* 肯定运算符 */
@media only screen and (width:800px) {
    h1 {
        background-color: orange;
    }
}
```

### 4. 常用阈值

**方式一**

```html
<link rel="stylesheet" media="screen and (min-width:1200px)" href="./css/huge.css">
```

**方式二**

```css
@media screen and (max-width:768px) {
    /*CSS-Code;*/ 
}
@media screen and (min-width:768px) and (max-width:1200px) {
    /*CSS-Code;*/ 
}
```

## BFC

**什么是BFC**

块格式化上下文（Block Formatting Context，BFC） 是 Web 页面的可视 CSS 渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。

**更通俗的解释**

- BFC 是 Block Formatting Context （块级格式上下文），可以理解成元素的一个“特异功能”。

- 该 “特异功能”，在默认的情况下处于关闭状态；当元素满足了某些条件后，该“特异功能”被激活。

- 所谓激活“特异功能”，专业点说就是：该元素创建了 BFC （又称：开启了 BFC ）

**开启BFC能解决什么问题**

- 元素开启 BFC 后，其子元素不会再产生 margin 塌陷问题

- 元素开启 BFC 后，自己不会被其他浮动元素所覆盖

- 元素开启 BFC 后，就算其子元素浮动，元素自身高度也不会塌陷

**如何开启BFC**

- 根元素

- 浮动元素

- 绝对定位、固定定位的元素

- 行内块元素

- 表格单元格： table 、 thead 、 tbody 、 tfoot 、 th 、 td 、 tr 、 caption

- overflow 的值不为 visible 的块元素

- 伸缩项目

- 多列容器

- column-span 为 all 的元素（即使该元素没有包裹在多列容器中）

- display 的值，设置为 flow-root
