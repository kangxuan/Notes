# HTML是什么

超文本标记语言

# HTML标签

HTML标签又称元素，是HTML的基本组成单位。

标签名不区分大小写，但推荐使用小写

**标签分类**

- 单标签

- 双标签（绝大多数标签都是双标签）

# HTML标签属性

用于给HTML标签提供附加信息。

一般属性都会有属性名和属性值，也有特殊的只有属性值。

属性支持大小写，但推荐使用小写。

**注意点**

- 不同标签有不同的属性，也有一些通用属性。

- 推荐使用双引号包裹属性值

- 同一个标签不能出现相同的属性，否则后写的会失效。

# HTML注释

注释的内容会被浏览器所忽略，不会呈现到页面中，但源代码中依然可见。

用来对代码进行解释说明。

```html
<!-- 这是一段注释 -->
```

**注意点**

- 注释不能嵌套

# HTML文档声明

文档声明用来告诉浏览器当前网页的版本。

```html
<!DOCTYPE html>
```

**注意点**

- 文档声明必须写在网页的第一行

# HTML字符编码

计算机存储数据时会编码，读取数据时会解码。

如果存储的时候编码有问题，那么数据就废了，读取数据编码错误，只需要更换数据编码可以正常读取。

**注意点**

- 统一使用UTF-8编码不会错

```html
...
<!-- 指定网页编码规范 -->
<meta charset="UTF-8">
```

# HTML设置语言

设置网页语言主要是为了让浏览器提示对应的翻译提示以及有利于SEO。

```html
<html lang="zh-CN">
```

# 官网

- [W3C官网](https://www.w3.org/)

- [W3school](https://www.w3school.com.cn/)

- [MDN]([MDN Web Docs](https://developer.mozilla.org/zh-CN/))

# 语义化标签

HTML标签强调语义，而不强调其默认样式，样式可以通过css调整。

**作用**

- 代码结构清晰可读性强

- 有利于SEO

- 方便设备解析（如屏幕阅读器、盲人阅读器等）

# 元素/标签分类

| 分类   | 表现    | 使用原则         | 常见元素 |
| ---- | ----- | ------------ | ---- |
| 块级元素 | 独占一行  | 块元素几乎能包所有元素  |      |
| 行内元素 | 不独占一行 | 行内能包含行内，不能包块 |      |
|      |       |              |      |

**特殊规则**

- `h1-h6`不能互相嵌套

- `p`中不能写块

# 按功能分类

## 1.排版标签

| 标签名   | 语义       | 单/双标签 |
| ----- | -------- | ----- |
| h1-h6 | 标题       | 双     |
| p     | 段落       | 双     |
| div   | 无语义，用于布局 | 双     |

**注意点**

- `h1`最好只写一个，`h2`-`h6`可以写多个。

- `h1-h6`不能互相嵌套。

- `p`中不能写块

## 2.文本标签

通常用来包裹词汇、短语等。

**常用文本标签**

| 标签名    | 语义               | 单/双标签 |
| ------ | ---------------- | ----- |
| span   | 没有语义，用于包裹短语的通用容器 | 双     |
| em     | 要着重强调的内容         | 双     |
| strong | 十分重要的内容（比em还要强）  | 双     |
|        |                  |       |
|        |                  |       |

**不常用文本标签**

| 标签名        | 语义                               | 单/双标签 |
| ---------- | -------------------------------- | ----- |
| cite       | 作品标题                             | 双     |
| dfn        | 特殊术语或专属名词                        | 双     |
| del、ins    | 删除的文本、插入的文本                      | 双     |
| sub、sup    | 下标文字、上标文字                        | 双     |
| code       | 一段代码                             | 双     |
| samp       | 从正常的上下文中，将某些内容提取出来，例如：标识设备输出     | 双     |
| kbd        | 键盘文本，表示文本是通过键盘输入的，经常用在与计算机相关的手册中 | 双     |
| abbr       | 缩写，最好配合上`title`属性                | 双     |
| bdo        | 更改文本方向，要配合`dir`属性                | 双     |
| var        | 标记变量，可以与`code`标签一起使用             | 双     |
| small      | 附属细则，例如：包括版权、法律文本。               | 双     |
| b          | 摘要中的关键字、评论中的产品名称。                | 双     |
| i          | 现在多用于：呈现字体图标                     | 双     |
| u          | 与正常内容有反差文本，例如：错的单词、不合适的描述等。      | 双     |
| q          | 短引用                              | 双     |
| blockquote | 长引用                              | 双     |
| address    | 地址信息                             | 双     |

**注意点**

- 不常用文本标签使用时酌情使用，不用纠结，不使用也没关系

- 除了`blockquote`、`address`是块，其他的都是行内

## 3.图片标签

| 标签名 | 语义  | 单/双标签 |
| --- | --- | ----- |
| img | 图片  | 单     |

**注意点**

- 尽量不要修改图片的高和宽，会导致比例失调

- 图片是行内块

- `alt`属性很重要
  
  - 搜索引擎通过`alt`得知图片的内容
  
  - 图片无法展示时，浏览器会呈现`alt`
  
  - 盲人阅读器会阅读`alt`值。

## 4.超链接

主要用于从当前页面跳转

| 标签名 | 语义  | 单/双标签 |
| --- | --- | ----- |
| a   | 超链接 | 双     |

**跳转到页面**

```html
<!-- 跳转到其他页面 -->
<a href="https://miaosha.jd.com/" target="_blank">去秒杀</a>
<!-- 跳转到本地页面 -->
<a href="./07_HTML字符编码.html">去字符编码</a>
```

**跳转到文件**

```html
<a href="./奥特曼.jpg" target="_blank">看奥特曼</a>
<!-- 指定了download会下载图片 -->
<a href="./奥特曼.jpg" download="奥特曼图片.jpg">下载奥特曼</a>
```

- 若浏览器无法打开文件，则会引导用户下载

- 若想强制触发下载，请使用 download 属性，属性值即为下载文件的名称

**跳转到锚点**

```html
<!-- 通过a标签跳 -->
<a href="#p4">跳到p4</a>
<!-- 通过id跳 -->
<a href="#p5">跳到p5</a>
<!-- 跳转到其他页面的某一个锚点 -->
<a href="https://www.cctv.com/#nav06">跳转到cctv体育</a>
...
<a name="p4"></a>
...
<p id="p5">555555555555555555555</p>
```

**唤起应用**

```html
<a href="tel:10086">电话联系</a>
<a href="sms:10086">短信联系</a>
<a href="mailto:10086@qq.com">邮件联系</a>
```

## 5.列表标签

| 标签名      | 语义    | 单/双标签 |
| -------- | ----- | ----- |
| ol、li    | 有序列表  | 双     |
| ul、li    | 无序列表  | 双     |
| dl、dt、dd | 自定义列表 | 双     |
|          |       |       |

```html
<!-- 有序列表 -->
<h2>如何将大象装进冰箱</h2>
<ol>
    <li>把冰箱门打开</li>
    <li>把大象装进冰箱</li>
    <li>把冰箱门关闭</li>
</ol>

<!-- 无序列表 -->
<h2>你想去哪个城市旅游</h2>
<ul>
    <li>北京</li>
    <li>上海</li>
    <li>广州</li>
    <li>深圳</li>
</ul>

<!-- 自定义列表 -->
<h2>如何高效的学习？</h2>
<dl>
    <dt>做好笔记</dt>
    <dd>笔记是我们以后复习的一个抓手</dd>
    <dd>笔记可以是电子版，也可以是纸质版</dd>
    <dt>多加练习</dt>
    <dd>只有敲出来的代码，才是自己的</dd>
    <dt>别怕出错</dt>
    <dd>错很正常，改正后并记住，就是经验</dd>
</dl>

<!-- 列表嵌套 -->
<h2>你想去哪个城市旅游</h2>
<ul>
    <li>北京</li>
    <li>上海</li>
    <li>
        <span>广州</span>
        <ul>
            <li>广州塔</li>
            <li>珠江新城</li>
            <li>上下九步行街</li>
            <li>天河体育中心</li>
        </ul>
    </li>
    <li>深圳</li>
</ul>
```

## 6.表格标签

| 标签名     | 语义     | 单/双标签 |
| ------- | ------ | ----- |
| table   | 表格     | 双     |
| caption | 表格标题   | 双     |
| thead   | 表格头部   | 双     |
| tbody   | 表格主体   | 双     |
| tfoot   | 表格注脚   | 双     |
| tr      | 每一行    | 双     |
| th、td   | 每一个单元格 | 双     |

**常用属性**

- table
  
  - width：表格宽
  
  - height：表格高
  
  - border：表格外边框
  
  - cellspacing：单元格之间的间距

- thead、tbody、tfoot、tr
  
  - height：表头高
  
  - align：单元格的水平对齐方式
    
    - left：左
    
    - center：中
    
    - right：右
  
  - valign：单元格的垂直对齐方式
    
    - top：上
    
    - middle：中
    
    - bottom：下

- td、th
  
  - width：单元格宽
  
  - height：单元格高
  
  - align：单元格的水平对齐方式
  
  - valign：单元格的垂直对齐方式
  
  - rowspan：指定跨行数
  
  - colspan：指定跨列数

## 7.表单标签

| 标签名      | 语义                | 单/双标签 |
| -------- | ----------------- | ----- |
| form     | 表单                | 双     |
| input    | 输入框               | 单     |
| button   | 按钮                | 双     |
| textarea | 文本域               | 双     |
| select   | 下拉框               | 双     |
| option   | 下拉框的选项            | 双     |
| label    | 与表单控件做关联，点击选中表单控件 | 双     |
| fieldset | 表单边框              | 双     |
| iframe   | 框架                | 双     |

**常用属性**

- form
  
  - action：表单要提交的地址
  
  - target：要跳转的位置
    
    - _self：本页面
    
    - _blank：新页
  
  - method：请求方式

- input
  
  - type：指定表单控件的类型
    
    - text：文本输入框
    
    - password：密码输入框
    
    - radio：单选框
    
    - checkbox：复选框
    
    - hidden：隐藏域
    
    - submit：提交按钮
    
    - reset：重置按钮
    
    - button：普通按钮
  
  - name：指定数据名称
  
  - value：数据值
  
  - disabled：设置表单控件不可用
  
  - maxlength：用于输入框，设置最大可输入长度
  
  - checked：用于单选按钮和复选框，默认选中

- textarea
  
  - name：指定数据名称
  
  - rows：指定默认显示的行数，影响文本域的高度
  
  - cols：指定默认显示的列数，影响文本域的宽度
  
  - disabled：设置表单控件不可用

- select
  
  - name：指定数据名称
  
  - disabled：设置表单控件不可用

- option
  
  - disabled： 设置拉下选项不可用
  
  - value：该选项事件提交的数据
  
  - selected：默认选中

- button
  
  - disabled：设置按钮不可用
  
  - type
    
    - submit：提交按钮，默认
    
    - reset：重置按钮
    
    - button：普通按钮

- label
  
  - for：值与要关联的表单控件的ID值相同

- iframe
  
  - name：框架名字，可以与target属性配合实现跳转到iframe
  
  - width：框架宽
  
  - height：框架高
  
  - frameborder：是否显示边框

# HTML字符实体

HTML实体可以用来表示某个符号，字符实体由三部分组成，分别是`&`和字符实体（或`#`、字符编号组成的字符实体）和`;`组成。

**常用的字符实体**

| 符号  | 描述   | 实体名称       |
| --- | ---- | ---------- |
|     | 空格   | `&nbsp;`   |
| <   | 小于号  | `&lt;`     |
| >   | 大于号  | `&gt;`     |
| &   | 和号   | `&amp;`    |
| "   | 引号   | `&quot;`   |
| ´   | 反引号  | `&acute;`  |
| ￠   | 分    | `&cent;`   |
| £   | 镑    | `&pound;`  |
| ¥   | 元    | `&yen;`    |
| €   | 欧元   | `&euro;`   |
| ©   | 版权   | `&copy;`   |
| ®   | 注册商标 | `&reg;`    |
| ™   | 商标   | `&trade;`  |
| ×   | 乘号   | `&times;`  |
| `÷` | 除号   | `&divide;` |

[完整参考]([HTML Standard](https://html.spec.whatwg.org/multipage/named-characters.html#named-character-references))

# HTML全局属性

全局属性也就是所有的标签都可以是用的属性

| 属性名   | 含义             | 用途                                                         |
| ----- | -------------- | ---------------------------------------------------------- |
| id    | 给标签指定唯一标识，不能重复 | 1. 可以让 label 标签与表单控件相关联 2. 可以与 CSS 、 JavaScript 配合使用 3. 锚点 |
| class | 给标签指定类名        | 配合CSS等使用                                                   |
| style | 给标签设置CSS样式     |                                                            |
| dir   | 内容的方向          | `ltr`：左向右；`rtl`：右向左                                        |
| title | 给标签设置一个文字提示    | 一般超链接和图片用得比较多                                              |
| lang  | 给标签指定语言        |                                                            |

# HTML元信息

- 配置字符编码

```html
<meta charset="utf-8">
```

- 针对IE浏览器的兼容性配置

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

- 针对移动端的配置

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

- 配置网页关键字

```html
<meta name="keywords" content="8-12个以英文逗号隔开的单词/词语">
```

- 配置网页描述信息

```html
<meta name="description" content="80字以内的一段话，与网站内容相关">
```

- 针对搜索引擎爬虫配置

```html
<meta name="robots" content="此处可选值见下表">
```

- 配置网页作者

```html
<meta name="author" content="tony">
```

- 配置网页生成工具

```html
<meta name="generator" content="Visual Studio Code">
```

- 配置定义网页版权信息

```html
<meta name="copyright" content="2023-2027©版权所有">
```

- 配置网页自动刷新

```html
<meta http-equiv="refresh" content="10;url=http://www.baidu.com">
```

[更多元信息配置]([&lt;meta&gt;：元数据元素 - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta))

# HTML5的优势

1. 针对`javascript`，新增了很多课操作的接口。

2. 新增了一些语义化标签、全局属性。

3. 新增了多媒体标签，可以很好的替代`flash`。

4. 更加侧重语义化，对于`SEO`更友好。

5. 可移植性好，可以大量应用在移动设备上。

# HTML5兼容性

IE 浏览器必须是 9 及以上版本才支持 HTML5 ，且 IE9 仅支持部分 HTML5 新特性。

# HTML5新增标签

## 1. 新增的布局标签

| 标签名     | 语义                                     | 单/双标签 |
| ------- | -------------------------------------- | ----- |
| header  | 整个页面，或部分区域的头部                          | 双     |
| footer  | 整个页面，或部分区域的底部                          | 双     |
| nav     | 导航                                     | 双     |
| article | 文章、帖子、杂志、新闻、博客、评论等，通常表示独立的文章           | 双     |
| section | 页面中的某段文字，或文章中的某段文字                     | 双     |
| aside   | 侧边栏                                    | 双     |
| main    | 文档的主要内容 ( WHATWG 没有语义， IE 不支持)，几乎不用。   | 双     |
| hgroup  | 包裹连续的标题，如文章主标题、副标题的组合 （ W3C 将其删除），几乎不用 | 双     |

```html
<!-- 头部 -->
<header class="page-header">
    <h1>XXX</h1>
</header>
<hr>
<!-- 主导航 -->
<nav>
    <a href="#">首页</a>
    <a href="#">订单</a>
    <a href="#">购物车</a>
    <a href="#">我的</a>
</nav>
<!-- 主要内容 -->
<div class="main-content">
    <!-- 侧边导航栏 -->
    <aside style="float: right;">
        <nav>
            <ul>
                <li><a href="#">秒杀专区</a></li>
                <li><a href="#">会员专区</a></li>
                <li><a href="#">领取优惠券</a></li>
                <li><a href="#">品牌专区</a></li>
            </ul>
        </nav>
    </aside>
    <!-- 文章 -->
    <article>
        <h2>《如何一夜暴富》</h2>
        <section>
            <h3>第一种方式：通过做梦</h3>
            <p>你要这么做梦：xxxxxxxxxxxxxxxxxxxxxxx</p>
        </section>
        <section>
            <h3>第一种方式：通过做梦</h3>
            <p>你要这么做梦：xxxxxxxxxxxxxxxxxxxxxxx</p>
        </section>
        <section>
            <h3>第一种方式：通过做梦</h3>
            <p>你要这么做梦：xxxxxxxxxxxxxxxxxxxxxxx</p>
        </section>
    </article>
</div>
<hr>
<!-- 底部 -->
<footer>
    <nav>
        <a href="#">友情链接1</a>
        <a href="#">友情链接2</a>
        <a href="#">友情链接3</a>
        <a href="#">友情链接4</a>
    </nav>
</footer>
```

## 2. 新增的状态标签

| 标签名      | 语义                       | 单/双标签 |
| -------- | ------------------------ | ----- |
| meter    | 定义已知范围内的标量测量，例如：电量、磁盘使用量 | 双     |
| progress | 显示某个任务完成的进度的指示器，例如：进度条   | 双     |

**常用属性**

- meter
  
  - high：规定高值
  
  - low：规定低值
  
  - max：规定最大值
  
  - min：规定最小值
  
  - optimum：规定最优值
  
  - value：规定当前值

- progress
  
  - max：规定目标值
  
  - value：规定当前值

```html
<span>手机电量：</span>
<meter max="100" min="0" value="90" low="10" high="20" optimum="90"></meter>
<br>
<span>当前进度：</span>
<progress max="100" value="20"></progress>
```

## 3. 新增的列表标签

| 标签名      | 语义                         | 单/双标签 |
| -------- | -------------------------- | ----- |
| datalist | 用于搜索框的关键字提示                | 双     |
| details  | 用于展示问题和答案，或对专有名词进行解释       | 双     |
| summary  | 写在 details 的里面，用于指定问题或专有名词 | 双     |
|          |                            |       |
|          |                            |       |

```html
<!-- 搜索相关词可以提示 -->
<input type="text" list="mydatalist">
<datalist id="mydatalist">
    <option value="周冬雨">周冬雨</option> 
    <option value="周杰伦">周杰伦</option>
    <option value="温兆伦">温兆伦</option>
    <option value="马冬梅">马冬梅</option>
</datalist>

<!-- 问题和答案 -->
<details>
    <summary>画一个数轴，依次表示以下数：2, 5, -1，1.5，3/2</summary>
    <p>数轴三要素：原点、正方向、单位长度。</p>
</details>
```

## 4. 新增的文本标签

| 标签名  | 语义                | 单/双标签 |
| ---- | ----------------- | ----- |
| ruby | 包裹需要注音的文字         | 双     |
| rt   | 写注音，rt标签写在ruby的里面 | 双     |
| mark | 标记                | 双     |

```html
<ruby>
    <span>魑魅魍魉</span>
    <rt>chī mèi wǎng liǎng</rt>
</ruby>
<hr>
<div>
    <ruby>
        <span>魑</span>
        <rt>chī</rt>
    </ruby>
    <ruby>
        <span>魅</span>
        <rt>mèi</rt>
    </ruby>
</div>
<hr>
<p>Lorem ipsum <mark>dolor</mark> sit amet consectetur adipisicing elit. Laboriosam, nemo?</p>
```

## 5. 新增多媒体标签

| 标签名   | 语义   | 单/双标签 |
| ----- | ---- | ----- |
| video | 定义视频 | 双     |
| audio | 定义音频 | 双     |

**常用属性**

- video
  
  - src：视频地址
  
  - width：宽
  
  - height：高
  
  - controls：向用户显示控件
  
  - muted：视频静音
  
  - autoplay：自动播放，
    
    - 一般要自动播放需要视频静音muted
    
    - 还有一种情况如果Chrome的媒体参与高的话可以不静音的时候自动播放
      
      - chrome://media-engagement/
  
  - loop：循环播放
  
  - poster：视频封面
  
  - preload：视频预加载，如果使用 autoplay ，则忽略该属性
    
    - none：不预加载
    
    - metadata：仅预先获取视频的元数据
    
    - auto：可以下载整个视频文件

```html
<video src="./小电影.mp4" width="500px" controls autoplay muted loop poster="./封面.png"></video>
```

- audio
  
  - src：地址
  
  - controls：向用户显示音频控件
  
  - autoplay：音频自动播放
  
  - muted：音频静音
  
  - loop：循环播放
  
  - preload：视频预加载，如果使用 autoplay ，则忽略该属性
    
    - none：不预加载
    
    - metadata：仅预先获取视频的元数据
    
    - auto：可以下载整个视频文件

```html
<audio src="./小曲.mp3" controls autoplay muted loop preload="auto"></audio>
```

# HTML5新增属性

## 1. 新增表单属性

| 属性名        | 功能                           |
| ---------- | ---------------------------- |
| novalidate | 给form标签设置了该属性，表单提交的时候不再进行验证。 |

## 2. 新增表单控件属性

| 属性名          | 功能            | 使用范围                                   |
| ------------ | ------------- | -------------------------------------- |
| placeholder  | 提示文字          | 适用于文字输入类的表单控件                          |
| required     | 该输入项必填        | 适用于除按钮外其他表单控件                          |
| autofocus    | 自动获取焦点        | 适用于所有表单控件                              |
| autocomplete | 自动完成，历史输入过的数据 | 适用于文字输入类的表单控件，密码输入框、多行文本框不适用           |
| pattern      | 写正则表达式        | 适用于文本输入类表单控件，必须和required配合使用，多行文本框不适用。 |

```html
<form action="">
    <span>账号：</span>
    <input 
        type="text" 
        name="account" 
        placeholder="请输入账号" 
        required 
        autofocus 
        autocomplete="on" 
        pattern="\w{6}"
    >
    <br>
    <span>密码：</span>
    <input type="password" name="pwd" placeholder="请输入密码" required pattern="\w{6}">
    <br>
    <span>性别：</span>
        <input type="radio" value="male" name="gender" required>男
        <input type="radio" value="female" name="gender">女
    <br>
    <span>爱好：</span>
        <input type="checkbox" value="smoke" name="hobby">抽烟
        <input type="checkbox" value="drink" name="hobby" required>喝酒
        <input type="checkbox" value="perm" name="hobby">烫头
    <br>
    <span>其他：</span>
    <textarea name="other" placeholder="请输入密码" required></textarea>
    <br>
    <button>提交</button>
</form>
```

## 3. input-type新增属性值

| 属性值            | 功能                                |
| -------------- | --------------------------------- |
| email          | 邮箱类型的输入框，表单提交时会验证格式，输入为空则不验证格式    |
| url            | url类型的输入框，表单提交时会验证格式，输入为空则不验证格式   |
| number         | 数字类型的输入框，表单提交时会验证格式，输入为空则不验证格式    |
| search         | 搜索类型的输入框，表单提交时不会验证格式              |
| tel            | 电话类型的输入框，表单提交时不会验证格式，在移动端使用时，会唤起数 |
| 字键盘。           |                                   |
| range          | 范围选择框，默认值为 50 ，表单提交时不会验证格式        |
| color          | 颜色选择框，默认值为黑色，表单提交时不会验证格式          |
| date           | 日期选择框，默认值为空，表单提交时不会验证格式           |
| month          | 月份选择框，默认值为空，表单提交时不会验证格式           |
| week           | 周选择框，默认值为空，表单提交时不会验证格式            |
| time           | 时间选择框，默认值为空，表单提交时不会验证格式           |
| datetime-local | 日期+时间选择框，默认值为空，表单提交时不会验证格式        |

# HTML5处理兼容性问题

- 添加元信息，让浏览器处于最优渲染模式

```html
<!--设置IE总是使用最新的文档模式进行渲染-->
<meta http-equiv="X-UA-Compatible" content="IE=Edge">

<!--优先使用 webkit ( Chromium ) 内核进行渲染, 针对360等壳浏览器-->
<meta name="renderer" content="webkit">
```

- 使用 html5shiv 让低版本浏览器认识 H5 的语义化标签

```html
<!--[if lt ie 9]>
<script src="../sources/js/html5shiv.js"></script>
<![endif]-->
```
