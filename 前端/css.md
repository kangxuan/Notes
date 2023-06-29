# CSS选择器

## 基础选择器

基础选择器一共分为4类，分别是：

1. 通配选择器

2. 元素选择器

3. 类选择器

4. ID选择器

**通配选择器**

通配选择器作用于所有的HTML元素，通配选择器整体来说没啥用，*主要用于清除样式*

- 语法

```css
* {
    属性名: 属性值;
}
/* 选择所有元素 */
* {
    color: orange;
    font-size: 30px;
}
```

**元素选择器**

元素选择器为页面中某种元素设置统一的样式

- 语法

```css
标签名 {
    属性名: 属性值;
}
/* 选中所有h2元素 */
h2 {
    color: cadetblue;
    font-size: 25px;
}
```

**类选择器**

类选择器根据元素的class值来选中元素

- 语法

```css
.类名 {
    属性名: 属性值;
}
/* 选中所有class值为anti的元素 */
.anti {
    color: chocolate;
}
```

- 规范
  
  - class值不使用纯数字、中文。尽量使用英文和数字的组合，多个单词用-连接
  
  - 一个元素可以有多个class，通过空格隔开

**ID选择器**

ID选择器根据元素的id值来选中元素

- 语法

```css
#ID值 {
    属性名: 属性值;
}
/* 选中ID为special的元素 */
#special {
    color: green;
    font-size: xx-large;
}
```

- 规范
  
  - ID值尽量由字母、数字、下划线( _ )、短杠( - )组成，最好以字母开头、不要包含空
    
    格、区分大小写
  
  - 一个元素只能拥有一个 id 属性，多个元素的 id 属性值不能相同
  
  - 一个元素可以同时拥有 id 和 class 属性

## 复合选择器

复合选择器是建立在基础选择器基础上的多种组合，主要用于在复杂架构中快速而准确选中元素，复合选择器分为：

1. 交集选择器

2. 并集选择器

3. 后代选择器

4. 子代选择器

5. ...

**交集选择器**

交集选择器用来选中同时满足多个条件的元素

- 语法

```css
选择器1选择器2... {
    属性名: 属性值
}
/* 这是交集选择器，即是p元素又是blue类 */
p.blue {
    color: blue;
}
```

- 规范
  
  - 有标签名，标签名必须写在前面，不然标签名会被认为和其他元素是一体
  
  - 交集选择器中不可能出现两个元素选择器，因为一个元素，不可能即是p元素又是span元素。
  
  - 最频繁使用的是：元素选择器配合类名选择器

**并集选择器**

并集选择器用来选中多个元素，是或的关系，任一满足都行

- 语法

```css
选择器1,选择器2,... {
    属性名: 属性值;
}
/* 选中class值为p1或p2或id值为id1的元素 */
.p1,
.p2,
#id1 {
    color: coral;
    background-color: aqua;
}
```

- 规范
  
  - 多个选择器一般隔行写

**后代选择器**

后代选择器用来选中一个元素的所有符合要求的后代（后代包括儿子、儿子的儿子....）

- 语法

```css
选择器1 选择器2 ... {
    属性名: 属性值;
}

/* 选中ul的所有li后代 */
ul li {
    color: chocolate;
}
/* 选中ul的所有li的所有a后代 */
ul li a {
    color: red;
}
/* 选中class值为subject的所有li后代 */
.subject li {
    color: blue;
}
/* 选中class值为subject的所有li且类名为step2的所有后代 */
.subject li.step2 {
    color: hotpink;
}
```

- 注意
  
  - 后代选择器选择的是后台，祖先不会被选择

**子代选择器**

子代选择器用来选中一个元素的所有符合要求的子代（只包括儿子）

- 语法

```css
选择器1>选择器2>... {
    属性名: 属性值;
}

/* 选中ul的所有li子代 */
ul>li {
    color: chocolate;
}
/* 选中class值为subject的所有li子代 */
.subject>li {
    color: aqua;
}
```

**兄弟选择器**

兄弟选择器是用来选择一个元素的所有符合要求的兄弟，包括相邻兄弟选择器和通用兄弟选择器。

1. 相邻兄弟选择器是选择紧紧相邻的第一个兄弟元素

2. 通用兄弟选择器是选择符合条件的所有兄弟元素
- 语法

```css
/* 选中li的紧紧相邻兄弟的li元素，不相邻不会被选中 */
li+li {
    color: aqua;
}
/* 选中li同级兄弟所有li元素，不相邻也会选中 */
li~li{
    color: brown;
}
```

- 注意
  
  - 这里的兄弟都是下面的兄弟，不往上数

**属性选择器**

属性选择器是选中属性符合一定要求的元素

- 语法

```css
/* 有此属性的都会被选择 */
[属性名] {
    ...
}
/* 有此属性且属性值完全等于的会被选择 */
[属性名="值"] {
    ...
}
/* 有此属性且属性值以什么开头会被选择 */
[属性名^="值"] {
    ...
}
/* 有此属性且属性值以什么结尾会被选择 */
[属性名$="值"] {
    ...
}
/* 有此属性且属性值包含什么会被选择 */
[属性名*="值"] {
    ...
}

/* 选中有title属性的所有元素 */
[title] {
    color: orange;
}
/* 选中title属性且值为a的所有元素 */
[title="a"] {
    font-size: 30px;
}
/* 选中title属性且以a开头的所有元素 */
[title^="a"] {
    color: red;
}
/* 选中title属性且值以b结尾的所有元素 */
[title$="b"] {
    color: green;
}
/* 选中title属性且值包含aa的所有元素 */
[title*="aa"] {
    color: aquamarine;
}
```

**伪类选择器**

伪类选择器又细分为：

1. 动态伪类

2. 结构伪类

3. 否定伪类

4. UI伪类

5. 目标伪类

6. 语言伪类

*动态伪类*

可以理解为有动态效果的选择器

- 语法
  
  - `:link`超链接未被访问的状态
  
  - `:visited`超链接访问过的状态
  
  - `:hover`鼠标悬停在元素上的状态
  
  - `:active`元素激活的状态，比如说鼠标按下
  
  - `:focus`获取焦点的元素

```css
/* 通常记忆是lvha */
a:link {
    color: orange;
}
a:visited {
    color: gray;
}
a:hover {
    color: aqua;
}
a:active{
    color: red;
}
span:hover {
    color: brown;
}
span:active {
    color: cadetblue;
}
input:focus,select:focus {
    color: yellow;
    background-color: green;
}
```

*结构伪类*

结构伪类选择器可以理解为结构中的多少个元素等

- 语法
  
  - `:first-child`所有兄弟元素中的第一个（都是针对某一个元素的子元素）
  
  - `:last-child`所有兄弟元素中的最后一个
  
  - `:nth-child(n)`所有兄弟元素中的第 n 个
  
  - `:first-of-type`所有同类型兄弟元素中的第一个
  
  - `:last-of-type`所有同类型兄弟元素中的最后一个
  
  - `:nth-of-type(n)`所有同类型兄弟元素中的 第n个
  
  - 以下了解即可
  
  - `:nth-last-child(n)`所有兄弟元素中的倒数第 n 个
  
  - `:nth-last-of-type(n)`所有同类型兄弟元素中的 倒数第n个
  
  - `:only-child`选择没有兄弟的元素（独生子女）
  
  - `:only-of-type`选择没有同类型兄弟的元素
  
  - `:root`根元素
  
  - `:empty`内容为空元素（空格也算内容）

```css
/* 选中div的第一个子元素且是p（按照所有兄弟计算），结构1 */
div>p:first-child {
    color: red;
}

/* 选中div的最后1个子元素且是p（按照所有兄弟计算），结构1 */
div>p:last-child {
    color: red;
}

/* 选中div的第几个子元素且是p（按照所有兄弟计算），结构1 */
div>p:nth-child(3) {
    color: red;
}

/* 选中的是div的偶数个儿子p元素（按照所有兄弟计算的）—— 结构2 */
/* 关于n的值 —— 结构2：
        1. 0或不写：什么都选不中 —— 几乎不用。
        2. n ：选中所有子元素  —— 几乎不用。
        3. 1 ~ 正无穷的整数，选中对应序号的子元素。
        4. 2n 或 even  ：选中序号为偶数的子元素。
        5. 2n+1 或 odd ：选中序号为奇数的子元素。
        6. -n+3 : 选中前三个。
 */
div>p:nth-child(2n) {
    color: red;
}

/* 选中的是div的第一个儿子p元素（按照所有同类型兄弟计算的）—— 结构3 */
div>p:first-of-type {
    color: red;
}

/* 选中的是div的最后一个儿子p元素（按照所有同类型兄弟计算的）—— 结构3 */
div>p:last-of-type {
    color: red;
}

/* 选中的是div的第n个儿子p元素（按照所有同类型兄弟计算的）—— 结构3 */
div>p:nth-of-type(3) {
    color: red;
}
```

*否定伪类*

否定伪类是用来选中元素但要排除一些条件

- 语法
  
  - `:not(选择器)`排除满足括号中条件的元素

```css
/* 选中的是div的儿子p元素，但是排除类名为fail的元素 */
div>p:not(.fail) {
    color: red;
}

/* 选中div的儿子元素，但是排除属性是title且属性值以“你要加油啊”开头 */
div>p:not([title^="你要加油啊"]) {
    color: green;
}

/* 选中的是div的儿子p元素，但排除第一个儿子p元素 */
div>p:not(:first-child) {
    color: red;
}
```

*UI伪类*

- 语法
  
  - `:checked`被选中的复选框或单选按钮
  
  - `:enable`可用的表单元素（没有disabled属性）
  
  - `:disabled`不可用的表单元素（有disabled属性）

```css
input:checked {
    width: 100px;
    height: 100px;
}
input:disabled {
    background-color: gray;
}
input:enabled {
    background-color: green;
    color: yellow;
}
```

*目标伪类*

- 语法
  
  - `:target`选中锚点指向的元素

```css
div {
    color: yellow;
    background-color: yellowgreen;
}
div:target {
    background-color: brown;
}
```

*语言伪类*

- 语法
  
  - `:lang()`根据指定的语言选择元素（本质是看 lang 属性的值）

```css
div:lang(en) {
    color: red;
}
:lang(zh-CN) {
    color: green;
}
```

**伪元素选择器**

伪元素并不是元素，而是元素中的一些特殊位置

- 语法
  
  - `::first-letter`选中元素中的第一个文字
  
  - `::first-line`选中元素中的第一行文字
  
  - `::selection`选中被鼠标选中的内容
  
  - `::placeholder`选中输入框的提示文字
  
  - `::before`在元素最开始的位置，创建一个子元素
  
  - `::after`在元素最后的位置，创建一个子元素

```css
/* 选中元素中的第一个文字 */
div::first-letter {
    color: red;
    font-size: 30px;
}
/* 选中元素中的第一行 */
div::first-line {
    background-color: yellowgreen;
}
/* 选中元素中被选择的内容 */
p::selection {
    background-color: yellow;
}
/* 选中输入框中的提示文字 */
input::placeholder {
    color: aqua;
}
/* 选中的是p元素最开始的位置，随后创建一个子元素 */
p::before {
    content: "￥";
}
/* 选中的是p元素最后的位置，随后创建一个子元素 */
p::after {
    content: ".00";
}
```

# 选择器优先级

通过不同的选择器，选中相同的元素，并且为相同的样式名设置不同值时，会存在样式冲突，此时就需要优先级来决胜出采用谁的样式

**简单规则**

行内 > ID选择器 > 类选择器 > 元素选择器 > 通配选择器

**具体规则**

根据权重比较，每个选择器都可以计算出一组权重，格式为(a, b, c)，其中：

- a表示ID选择器的个数

- b表示类、伪类、属性选择器的个数

- c表示元素、伪元素选择器的个数

比较规则是从左往右比较，最大则胜出不用继续比较，如果最终一样则按照”后来居上“规则覆盖。但有两个特例，行内样式大于所有选择器，`!important`大于行内。

![选择器优先级](./images/选择器优先级.png)

# CSS三大特性

**层叠性**

如果发生了样式冲突，那就会根据一定的规则（选择器优先级），进行样式的层叠（覆盖）。

**继承性**

元素会自动拥有其父元素、或其祖先元素上所设置的某些样式，优先继承离得近的。

- 常见的可继承属性
  
  - `text??`
  
  - `font??`
  
  - `line??`
  
  - `color`
  
  - ...

**优先级**

也就是出现了样式冲突如何选择的问题

# CSS常用属性

**像素是什么**

显示器中的一个个小点，就是像素点，像素点越小越清晰。

**能使用的长度单位**

`cm`、`mm`、`px`等等

```css
.at1 {
    width: 10cm;
    height: 10cm;
    background-color: green;
}
.at2 {
    width: 10mm;
    height: 10mm;
    background-color: gray;
}
.at3 {
    width: 10px;
    height: 10px;
    background-color: red;
}
```

**颜色的集中表达方式**

- 颜色名，通过颜色名表达，比如`red`、`yellow`、`green`。
  
  - 具体多少参考[官网颜色名表](https://developer.mozilla.org/en-US/docs/Web/CSS/named-color)

```css
div {
    background-color: green;
}
```

- rgb/rgba
  
  - 使用红、绿、蓝三原色组合，其中r对应红色、g对应绿、b对应蓝，a表示透明度。
  
  - 规律
    
    - 若三种颜色值相同，呈现的是灰色，值越大，灰色越浅
    
    - 对于 rbga 来说，前三位的 rgb 形式要保持一致，要么都是 0~255 的数字，要么都是百分比

```css
div {
    background-color: rgb(255, 0, 0);
}

/* 常用的颜色 */
/*  红 */
rgb(255, 0, 0)
/*  绿 */
rgb(0, 255, 0);
/*  蓝 */
rgb(0, 0, 255);
/* 黑 */
rgb(0, 0, 0);
/* 白 */
rgb(255, 255, 255);
/* 紫罗兰 */
rgb(138, 43, 226)
/* 灰色，数值一样表示灰色，值越大灰色越浅 */
rgb(100, 100, 100)
```

- HEX/HEXA
  
  - 和rgb类似，通过红、绿、蓝的16进制表达

```css
div {
    background-color: #00ff00;
}

/* 常用的颜色 */
/* 红色 */
#ff0000;
/* 绿色 */ 
#00ff00;
/* 蓝色 */
#0000ff;
/* 黑色 */
#000000; 
/* 白色 */
#ffffff; 
/* 如果每种颜色的两位都是相同的，就可以简写*/
#ff9988;/* 可简为：#f98 */
/* 但要注意前三位简写了，那么透明度就也要简写 */
#ff998866;/* 可简为：#f986 */
```

- HSL/HSLA
  
  - HSL 是通过：色相、饱和度、亮度，来表示一个颜色的，格式为： hsl(色相,饱和度,亮度)

**CSS字体属性**

- 字体大小
  
  - `font-size`
  
  - 注意
    
    - Chrome默认最小字体大小为12px，默认字体大小为16px，0px会自动消失。
    
    - 不同浏览器的默认字体大小不一样，故使用明确的字体大小。
    
    - 通常给body设置字体大小，方便继承。

- 字体族
  
  - `font-family`
  
  - 注意
    
    - 使用字体的英文名字兼容性会更好，具体的英文名可以自行查询，或在电脑的设置里去寻找。
    
    - 如果字体名包含空格，必须使用引号包裹起来。
    
    - 可以设置多个字体，按照从左到右的顺序逐个查找，找到就用，没有找到就使用后面的，且通常在最后写上 serif （衬线字体）或 sans-serif （非衬线字体）。

- 字体风格
  
  - `font-style`
  
  - 选项
    
    - `normal`默认值正常
    
    - `italic`斜体（默认使用字体的斜体，如果字体没有斜体，则将字体直接倾斜）
    
    - `oblique`斜体（直接将字体倾斜）

- 字体粗细
  
  - `font-weight`
  
  - 选项
    
    - `normal`正常默认
    
    - `lighter`细
    
    - `bold`粗
    
    - `bolder`更粗（如果字体有设计会有，如果未设计则是bold的样式）
    
    - 数字，范围100-1000

- 复合写法
  
  - `font`
  
  - 规则
    
    - 字体大小、字体族必须都写上且字体大小在倒数第二位，字体族在倒数第一位，并用空格隔开

```css
* {
    font-size: 16px;
}
p {
    font-size: 20px;
    font-family: "Microsoft YaHei",sans-serif;
    font-style: oblique;
    font-weight: normal;
}
/* 复合写法 */
p {
    font: lighter normal 20px "Microsoft YaHei",sans-serif;
}
```

**CSS文本属性**

- 文本颜色
  
  - `color`

- 文本间距
  
  - `letter-spacing`字母间距
  
  - `word-spacing`单词间距

- 文本修饰
  
  - `text-decoration`
  
  - 选项
    
    - `none`默认无装饰
    
    - `underline`下划线
    
    - `overline`上划线
    
    - `line-through`删除线
  
  - 搭配使用
    
    - `dotted`虚线
    
    - `wavy`波浪线
    
    - 颜色

- 文本缩进
  
  - `text-indent`
  
  - 单位
    
    - px按像素
    
    - em按

- 文本水平对齐
  
  - `text-align`
  
  - 选项
    
    - `left`默认左对齐
    
    - `right`右对齐
    
    - `center`居中对齐

- 行高
  
  - `line-height`
  
  - 选项
    
    - `normal`由浏览器根据文字大小决定
    
    - px，用户自行设置一个像素
    
    - 倍数，`font-size`的倍数，比如2，font-size为16px，那么行高则未32px
    
    - 百分比，`font-size`的百分比
  
  - 注意
    
    - `line-height`设置过小字体会压缩重叠，且最小值是0，不能小于0
    
    - `line-height`可以被继承，常用倍数方式比较方便
    
    - `line-height`和`height`的关系
      
      - 设置了`height`，那么高度就是`height`
      
      - 只设置了`line-height`，会根据`line-height`计算，高=行高×行数
  
  - 运用场景
    
    - 控制行与行之间的距离
    
    - `height`和`line-height`相等时，文本会垂直居中

- 文本垂直对齐
  
  - 文本垂直对齐没有专门的属性名
  
  - 顶部
    
    - 默认顶部对齐
  
  - 居中
    
    - `height`等于`line-height`
  
  - 底部
    
    - `line-height` = (`height` × 2 ) - `font-size` - `x`，x根据具体字体做调整。

- vertical-align
  
  - 用于指定同一行元素之间，或 表格单元格 内文字的 垂直对齐方式。
  
  - 选项
    
    - `baseline`使元素的基线与父元素的基线对齐（默认）
    
    - `top`使元素的顶部与其所在行的顶部对齐
    
    - `middle`使元素的中部与父元素的基线加上父元素字母 x 的一半对齐
    
    - `bottom`使元素的底部与其所在行的底部对齐

```css
/* 文本颜色，上线的例子有示例 */
/* 文本间距 */
.at2 {
    letter-spacing: 20px;
}
.at3 {
    word-spacing: 20px;
}
/* 文本修饰 */
.at1 {
    /* 上划的红色虚线 */
    text-decoration: overline dotted red;
}
.at2 {
    /* 下划的绿色波浪线 */
    text-decoration: underline wavy green;
}
.at3 {
    /* 删除线 */
    text-decoration: line-through;
}
.at4 {
    /* 没有各种线 */
    text-decoration: none;
}
/* 文本缩进 */
div {
    font-size: 20px;
    text-indent: 2em;
}
/* 文本水平对齐 */
div {
    font-size: 30px;
    background-color: green;
    /* 文本水平对齐 */
    text-align: center;
}
/* 行高 */
#d1 {
    font-size: 30px;
    background-color: skyblue;
    /* 第一种写法：像素 */
    line-height: 30px;
    /* 第二种写法：值为normal */
    line-height: normal;
    /* 第三种写法：倍数，fontsize的倍数 */
    line-height: 1.5;
    /* 第四种写法：百分比 */
    line-height: 200%;
}
/* 文本垂直对齐 */
div {
      font-size: 40px;
      height: 400px;
      background-color: skyblue;
      /* 垂直居中 */
      /* line-height: 400px; */
      /* 底部 height*2 - fontsize - x */
      line-height: 745px;
}
/* 同一行元素之间对齐 */
span {
    font-size: 40px;
    background-color: orange;
    vertical-align: middle;
}
img {
    height: 30px;
    vertical-align: top;
}
.san {
    vertical-align: bottom;
}
```

**CSS列表属性**

只作用于`ul`、`ol`、`li`的属性。

- 设置列表符号样式
  
  - `list-style-type`
  
  - 选项值
    
    - `none`不显示前面的标识（很常用！）
    
    - `square`实心方块
    
    - `disc`圆形
    
    - `decimal`数字
    
    - `lower-roman`小写罗马字
    
    - `upper-roman`大写罗马字
    
    - `lower-alpha`小写字母
    
    - `upper-alpha`大写罗马字

- 设置列表符号的位置
  
  - `list-style-position`
  
  - 选项值
    
    - `inside`在`li`里面
    
    - `outside`在`li`外边

- 自定义列表符号
  
  - `list-style-image`
    
    - `url(图片地址)`

- 复合属性
  
  - `list-style`
  
  - 没有顺序要求

```css
ul {
    /* list-style-type: none; */
    /* list-style-position: inside; */
    /* list-style-image: url(../images/video.gif); */
    list-style: none inside url(../images/video.gif);
}
```

**CSS表格属性**

- 边框宽度
  
  - `border-width`
  
  - 非表格独有

- 边框颜色
  
  - `border-color`
  
  - 非表格独有

- 边框风格
  
  - `border-style`
  
  - 非表格独有
  
  - 选项值
    
    - `none`默认值
    
    - `solid`实线
    
    - `dashed`虚线
    
    - `dotted`点线
    
    - `double`双实线

- 复合
  
  - `border`

- 设置列宽度
  
  - `table-layout`
  
  - table标签独有
  
  - 选项值
    
    - `auto`自动适应
    
    - `fixed`固定列宽，平分

- 单元格间距
  
  - `border-spacing`
  
  - table标签独有

- 合并单元格边框
  
  - `border-collapse`
  
  - table标签独有
  
  - 选项值
    
    - `collapse`合并
    
    - `separate`不合并

- 隐藏没有内容的单元格
  
  - `empty-cells`
  
  - table标签独有
  
  - 选项值
    
    - `show`显示（默认值）
    
    - `hide`隐藏

- 设置表格标题位置
  
  - `caption-side`
  
  - 选项值
    
    - `top`上面（默认值）
    
    - `bottom`下面

```css
table {
    /* 设置表宽为500px */
    width: 500px;
    /* 表格外边框为2px */
    border-width: 2px;
    /* 表格外边框颜色为green */
    border-color: green;
    /* 表格外边框为实线 */
    border-style: solid;
    /* 控制表格的列宽 */
    table-layout: fixed;
    /* 控制单元格间距 */
    border-spacing: 0px;
    /* 合并相邻的单元格的边框 */
    border-collapse: collapse;
    /* 隐藏没有内容的单元格 */
    empty-cells: hide;
    /* 设置表格标题的位置 */
    caption-side: top;
}
td,th {
    /* 设置每个单元格的边框为2px，天蓝色、实线 */
    border: 2px skyblue solid;
}
/* 不止table其他标签也可以设置 */
h2 {
    border:3px red solid;
}
span {
    border:3px purple dashed;
}
```

**CSS背景属性**

- 设置背景颜色
  
  - `background-color`
  
  - 默认值是`transparent`，透明

- 设置背景图片
  
  - `background-image`
  
  - `url(图片的地址)`

- 设置背景重复方式
  
  - `background-repeat`
  
  - 选项值
    
    - `repeat`重复，默认值
    
    - `repeat-x`水平重复
    
    - `repeat-y`垂直重复
    
    - `no-repeat`不重复

- 设置背景图位置
  
  - `background-position`
  
  - 选项值
    
    - 关键字设置
      
      - 水平：`left`、`center`、`right`
      
      - 垂直：`top`、`center`、`bottom`
      
      - 如果只写一个值，另一个方向的值取`center`
    
    - 通过长度设置坐标位置（针对的是左上的点）
      
      - 两个值，分别是 x 坐标和 y 坐标
      
      - 只写一个值，会被当做 x 坐标， y 坐标取`center`

- 复合属性
  
  - `background`

```css
div {
      width: 300px;
      height: 300px;
      border: 2px green solid;
      background-color: skyblue;
      background-image: url(../images/悟空.jpg);
      background-repeat: no-repeat;
      /* background-position: center center; */
      background-position: 100px 100px;
      /* 复合属性 */
      background: skyblue url(../images/悟空.jpg) no-repeat 100px 100px;
}
```

**CSS鼠标属性**

- 设置鼠标光标的样式
  
  - `cursor`
  
  - 选项值
    
    - `pointer`小手
    
    - `move`移动图标
    
    - `text`文字选择器
    
    - `crosshair`十字架
    
    - `wait`等待
    
    - `help`帮助
    
    - `url("./arrow.png"),pointer`自定义小图标

```css

```
