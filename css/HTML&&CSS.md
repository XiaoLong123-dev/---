[TOC]



# HTML、CSS

## 基础知识

<b>background-clip</b>:设置元素的背景（背景图片或颜色）是否延伸到边框、内边距盒子、内容盒子下面。

### BFC

**BFC(Block formatting context)直译为"块级格式化上下文"。**

它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

BFC是一个独立的布局环境，其中的元素布局是不受外界的影响，并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。

#### BFC的布局规则

　　**1.** 内部的Box会在垂直方向，一个接一个地放置。

　　**2.** Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。

　　**3.** 每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

　　**4.** BFC的区域不会与float box重叠。

　　**5.** BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

　　**6.** 计算BFC的高度时，浮动元素也参与计算。

#### 如何创建BFC

　　**1、**float的值不是none。

　　**2、**position的值不是static或者relative。

　　**3、**display的值是inline-block、table-cell、flex、table-caption或者inline-flex

　　**4、**overflow的值不是visible

#### BFC的应用

避免margin重叠

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>防止margin重叠</title>
</head>
<style>
    *{
        margin: 0;
        padding: 0;
    }
    p {
        color: #f55;
        background: yellow;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 30px;
    }
    div{
        overflow: hidden;
    }
</style>
<body>
    <p>看看我的 margin是多少</p>
    <div>
        <p>看看我的 margin是多少</p>
    </div>
</body>
</html>
```

自适应两栏布局 (BFC的区域不会与float box重叠。）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<style>
    *{
        margin: 0;
        padding: 0;
    }
    body {
        width: 100%;
        position: relative;
    }
 
    .left {
        width: 100px;
        height: 150px;
        float: left;
        background: rgb(139, 214, 78);
        text-align: center;
        line-height: 150px;
        font-size: 20px;
    }
 
    .right {
        overflow: hidden;
        height: 300px;
        background: rgb(170, 54, 236);
        text-align: center;
        line-height: 300px;
        font-size: 40px;
    }
</style>
<body>
    <div class="left">LEFT</div>
    <div class="right">RIGHT</div>
</body>
</html>
```

清除浮动(没有清除浮动父元素没有高度)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>清除浮动</title>
</head>
<style>
    .par {
        border: 5px solid rgb(91, 243, 30);
        width: 300px;
        overflow: hidden;
    }
    
    .child {
        border: 5px solid rgb(233, 250, 84);
        width:100px;
        height: 100px;
        float: left;
    }
</style>
<body>
    <div class="par">
        <div class="child"></div>
        <div class="child"></div>
    </div>
</body>
</html>
```

### clip-path

**`clip-path`** [CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) 属性使用裁剪方式创建元素的可显示区域。区域内的部分显示，区域外的隐藏。

#### clip

clip属性是用于**裁剪**元素的，它可以在对应元素内指定一个矩形区域进行裁剪，最终拿到对应元素的矩形子域，它的用法示例如下：

```css
/* 不支持百分比的写法，如 clip:rect(50%, 60%, 30%, 50%)是不合法的 */
div{
  position:absolute;
  clip:rect(0px,60px,200px,0px); 
  clip:rect(50px 250px 250px 50px); /* 兼容IE6, IE7，需要将值之间的逗号去掉 */
}
```

**以对应元素的左上角为坐标原点，分别设置top, right, bottom, left，同时将该元素的定位设置为absolute或fixed**

**缺点**

* 只对绝对定位(absolute,fixed)的元素有效，而对于relative和static的元素无效
* 因为只有rect()函数可用，所以只能用来裁剪出矩形

#### clip-paths属性

支持设置三个属性，

* ``clip-source``可以使用形如 ``clip-path:url(resources.svg#c1)`` 的方式从svg导入裁剪元素的路径
* ``basic-shape``
  * circle，用于裁剪出圆形
  * ellipse，用于裁剪出椭圆形
  * inset，用于裁剪出矩形
  * polygon，用于裁剪出任意图形，最为强大的一个api
* ``geometry-box``可以看成裁剪区域的坐标系，里的api的坐标都将以指定的盒子进行定位，如果不指定，默认是``border-box``，它的取值列举如下：
  * margin-box
  * border-box
  * padding-box
  * content-box
  * fill-box
  * stroke-box
  * view-box

#### 各api的具体用法

```css
/*
    需要注意的是，所有api里面的百分比都是以元素宽度作为基准来计算的
*/


/* 
    circle(radius at x-axis y-axis) 
    为了创建圆形，需要给circle传入三个值：圆心的坐标（X值和Y值）以及半径。当定义圆的半径时，我们可以用at关键字来定义圆心坐标。
*/
div {   
  clip-path: circle(50% at 50% 50%);   
}  

/* 
    ellipse(x-radius y-radius at x-axis y-axis) 
    为了实现椭圆，需要给ellipse提供4个值：椭圆的x轴半径、y轴半径、定位椭圆位置的x坐标和y坐标，后面两个值用at关键字和前面两个值分开。
*/
div {   
  clip-path: ellipse(100px 200px at 50% 50%);  
} 

/* 
    inset(<top> <right> <bottom> <left> round <top-radius> <right-radius> <bottom-radius> <left-radius>)
    使用四个值（对应“上 右 下 左”的顺序）来裁剪矩形和设置圆角半径。 
*/
div {   
  clip-path: inset(25% 0 25% 0 round 0 25% 0 25%);  
  clip-path: inset(25% 0 round 0 25%); /* 简写形式 */
} 

/* 
    polygon(x1-axis y1-axis, x2-axis y2-axis, … )   
    指定各个节点的坐标，不限制数量，最终将所有点连接起来形成裁剪框，从而裁剪出我们需要的形状
    它是最强大的api，通过它我们可以裁剪出包括但不限于上述所有api的图形 
*/
div {   
  clip-path: polygon(0% 0%, 100% 0%, 100% 75%, 75% 75%, 75% 100%, 50% 75%, 0% 75%);   
}
```



### 初始化样式

有些标签默认含有内外边距，且不同浏览器大小也不一样。为了统一我们可以重置标签的 CSS 默认样式。

最简单的方式就是使用插件[css-reset (opens new window)](https://meyerweb.com/eric/tools/css/reset/)完成，你也可以在 vscode 等编辑器中定义代码片段。

```css
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/meyer-reset/2.0/reset.min.css" />
```

### 选择器

#### 基本选择器

| 选择器            | 示例       | 描述                                   |
| :---------------- | ---------- | :------------------------------------- |
| .class            | .intro     | 选择 class="intro" 的所有元素          |
| #id               | #firstname | 选择 id="firstname" 的所有元素         |
| *                 | *          | 选择所有元素                           |
| element           | p          | 选择所有 p 元素                        |
| element,element   | div,p      | 选择所有 div 元素和所有 p 元素         |
| element element   | div p      | 选择 div 元素内部的所有 p 元素         |
| element>element   | div>p      | 选择父元素为 div 元素的所有 p 元素     |
| element+element   | div+p      | 选择紧接在 div 元素之后的所有 p 元素   |
| element1~element2 | p~ul       | 选择p元素同级并在p元素后面的所有ul元素 |

##### 子元素选择器

只选中子元素不选中孙级及以下元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <style>
        main article>h2 {
            color: green;
        }
    </style>
</head>

<body>
    <main>
        <article>
            <h2 name="houdunren">houdunren.com</h2> <!--变绿-->
            <aside>
                <h2>houdunwang.com</h2>
            </aside>
        </article>
        <h2 name="hdcms.com">hdcms.com</h2>
        <h1>后盾人</h1>
    </main>
</body>

</html>
```

##### 紧邻兄弟元素

用于选择**紧挨**着的同级兄弟元素

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* hdcms.com变绿 */
        main article+h2 {
            color: green;
        }
    </style>
</head>
<body>
    <main>
        <article>
            <h2 name="houdunren">houdunren.com</h2>
            <aside>
                <h2>houdunwang.com</h2>
            </aside>
        </article>
        <h2 name="hdcms.com">hdcms.com</h2><!-- 变绿 -->
        <h1>后盾人</h1>
        <h2 name="hdcms2.com">hdcms2.com</h2>
    </main>
</body>

</html>
```

##### 后面的兄弟元素

article~* : 用于选择后面所有的兄弟元素；article~h2 ：用于选择后面指定的兄弟元素

```html

    <style>
        main article~* {
            color: green;
        }
        /* hdcms.com，hdcms2.com都变绿,h1不变绿
         main article~h2 {
            color: green;
        } */
    </style>
<body>
    <main>
        <article>
            <!--  -->
            <h2 name="houdunren">houdunren.com</h2>
            <aside>
                <h2>houdunwang.com</h2>
            </aside>
        </article>
        <h2 name="hdcms.com">hdcms.com</h2><!-- 变绿 -->
        <h1>后盾人</h1><!-- 变绿 -->
        <h2 name="hdcms2.com">hdcms2.com</h2><!-- 变绿 -->
    </main>
</body>
```

#### 属性选择器

根据属性来为元素设置样式

| 选择器              | 示例               | 描述                                                        |
| :------------------ | ------------------ | :---------------------------------------------------------- |
| [attribute]         | [target]           | 带有 target 属性所有元素                                    |
| [attribute=value]   | [target=_blank]    | targe 属性 等于"_blank" 的所有元素                          |
| [attribute~=value]  | [title~=houdunren] | title 属性包含单词 "houdunren" 的所有元素                   |
| [attribute\|=value] | [title\|=hd]       | `title 属性值为 "hd"的单词，或hd-cms` 以`-`连接的的独立单词 |
| [attribute*=value]  | a[src*="hdcms"]    | src 属性中包含 "hdcms" 字符的每个 a 元素                    |
| [attribute^=value]  | a[src^="https"]    | src 属性值以 "https" 开头的每个 a 元素                      |
| [attribute$=value]  | a[src$=".jpeg"]    | src 属性以 ".jpeg" 结尾的所有 a 元素                        |

为具有 `class` 属性的 h1 标签设置样式

```text
h1[class] {
    color: red;
}
...

<h1 class="container">houdunren.com</h1>
```

约束多个属性

```text
h1[class][id] {
    color: red;
}
...

<h1 class="container" id >houdunren.com</h1>
```

具体属性值设置样式

```text
a[href="https://www.houdunren.com"] {
    color: green;
}
...

<a href="https://www.houdunren.com">后盾人</a>
<a href="">HDCMS</a>
```

`^` 以指定值开头的元素

```text
h2[name^="hdcms"] {
    color: red;
}
...

<h2 name="houdunren">houdunren.com</h2>
<h2 name="hdcms.com">hdcms.com</h2>
```

`$` 以指定值结尾的元素

```text
<h2 name="houdunren">houdunren.com</h2>
<h2 name="hdcms.com">hdcms.com</h2>
...

h2[name$="com"] {
    color: red;
}
```

`*` 属性内部任何位置出现值的元素

```text
h2[name*="houdunren"] {
    color: red;
}
...

<h2 name="houdunren">houdunren.com</h2>
<h2 name="houdunren.com">hdcms.com</h2>
```

`~` 属性值中包含指定**词汇**的元素(必须是一个单词，不是包含关系)

```text
/* 可以不写冒号 */
h2[name~="houdunren"] {
    color: red;
}
...

<h2 name="houdunren">houdunren.com</h2><!-- 变色 -->
<h2 name="houdunren web">hdcms.com</h2><!-- 变色 -->
<h2 name="houdunrenweb">hdcms.com</h2><!-- 不变色 -->
```

`|` 以指定值开头或以属性连接破折号的元素

```text
h2[name|="houdunren"] {
    color: red;
}
...

<h2 name="houdunren">houdunren.com</h2><!-- 变色 -->
<h2 name="houdunren-web">hdcms.com</h2><!-- 变色 -->
<h2 name="houdunrenweb">hdcms.com</h2><!-- 不变色 -->
```

#### 伪类选择器

| 状态                 | 示例                  | 说明                                          |
| -------------------- | --------------------- | --------------------------------------------- |
| :link                | a:link                | 选择所有未被访问的链接                        |
| :visited             | a:visited             | 选择所有已被访问的链接                        |
| :hover               | a:hover               | 鼠标移动到元素上时                            |
| :active              | a:active              | 点击正在发生时                                |
| :focus               | input::focus          | 选择获得焦点的 input 元素                     |
| :root                | :root                 | 选择文档的根元素即 html。                     |
| :empty               | p:empty               | 选择没有子元素的每个 p 元素（包括文本节点）。 |
| :first-child         | p:first-child         | 选择属于父元素的第一个子元素的每个 p 元素     |
| :last-child          | p:last-child          | 选择属于其父元素最后一个子元素每个 p 元素。   |
| :first-of-type       | p:first-of-type       | 选择属于其父元素的首个 p 元素的每个 p 元素    |
| :last-of-type        | p:last-of-type        | 选择属于其父元素的最后 p 元素的每个 p 元素。  |
| :only-of-type        | p:only-of-type        | 选择属于其父元素唯一的 p 元素的每个 p 元素。  |
| :only-child          | p:only-child          | 选择属于其父元素的唯一子元素的每个 p 元素。   |
| :nth-child(n)        | p:nth-child(2)        | 选择属于其父元素的第二个子元素的每个 p 元素。 |
| :nth-child(odd)      | p:nth-child(odd)      | 选择属于其父元素的奇数 p 元素。               |
| :nth-child(even)     | p:nth-child(even)     | 选择属于其父元素的偶数 p 元素。               |
| :nth-of-type(n)      | p:nth-of-type(2)      | 选择属于其父元素第二个 p 元素的每个 p 元素。  |
| :nth-last-child(n)   | p:nth-last-child(2)   | 同上，从最后一个子元素开始计数。              |
| :nth-last-of-type(n) | p:nth-last-of-type(2) | 同上，但是从最后一个子元素开始计数。          |
| :not(selector)       | :not(p)               | 选择非 p 元素的每个元素                       |

##### :target

用于控制具有锚点目标元素的样式

```text
div {
	height: 900px;
}

div:target {
	color: red;
}
...

<a href="#hdcms">hdcms</a>
<div></div>
<div id="hdcms">
	hdcms内容管理系统
</div>
```

##### :empty

没有内容和空白的元素。下面第一个 p 标签会产生样式，第二个不会因为有空白内容

```text
:empty {
    border: solid 2px red;
}
...
<p></p>
<p> </p>
```

##### :first-of-type

选择父元素下第一个类型为span的元素

```text
article span:first-of-type {
    color: red;
}
...

   <article>
        <span>houdunren.com</span><!-- 变色 -->
        <span>houdunren2.com</span>
        <aside>
            <strong>hdcms.com</strong>
            <span>houdunwang.com</span><!-- 变色 -->
            <span>houdunwang2.com</span>
        </aside>
    </article>
```

##### :last-of-type

```html
article span:last-of-type {
            color: red;
}

<article>
        <span>houdunren.com</span>
        <span>houdunren2.com</span><!-- 变色 -->
        <aside>
            <strong>hdcms.com</strong>
            <span>houdunwang.com</span>
            <span>houdunwang2.com</span><!-- 变色 -->
        </aside>
</article>
```

##### :only-of-style

 ``article span:only-of-type``如果article中只有一个span元素，才会生效

```html
 <style>
        article span:only-of-type {
            color: red;
        }
    </style>

<body>
     <article>
        <span>houdunren.com</span><!-- 不变色 -->
        <!-- 只有一个span的时候才会生效 -->
        <span>houdunren2.com</span><!-- 不变色 -->
        <aside>
            <strong>hdcms.com</strong>
            <span>houdunwang.com</span><!-- 变色 -->
            <!-- 如果再加一个span两个都不会生效 -->
            <!-- <span>houdunwang2.com</span> -->
        </aside>
    </article>
</body>
```

##### :only:child

只有当父元素下只有一个子元素时才能生效

```html
    <style>
        article span:only-child {
            color: red;
        }
    </style>
 <article>
     <!-- 当只有span这一个子元素时才能生效 -->
        <span>houdunren.com</span>
     <!-- 加上p就不能生效 -->
        <p>1</p>
</article>
```

##### :nth-child()

选择第二个元素并且是 span 标签的

```html
<style>
         article span:nth-child(3) {
            color: red;
        } 
    </style>
    <article>
        <span>houdunren.com</span><!-- nth-child(1) -->
        <aside>
            <span>houdunwang.com</span><!-- nth-child(1) -->
            <span>hdcms.com</span><!-- nth-child(2) -->
        </aside>
        <span>hdphp.com</span><!-- nth-child(3) -->
    </article>
```

##### :nth-of-type()

选择指定的第几个元素，不管中间的其他元素

```html
<style>
        article span:nth-of-type(2) {
            color: red;
        }
</style>
    <article>
        <span>houdunren.com</span><!-- nth-of-type(1) -->
        <aside>
            <span>houdunwang.com</span><!-- nth-of-type(1) -->
            <span>hdcms.com</span><!-- nth-of-type(2) -->
        </aside>
        <span>hdphp.com</span><!-- nth-of-type(2) -->
    </article>
```

##### :not(selector)

排除第一个li元素

```html
ul li:not(:nth-child(1)) {
    background: red;
}
...

<ul>
  <li>houdunren.com</li>
  <li>hdcms.com</li>
  <li>后盾人</li>
</ul>
```

#### 表单伪类

| 选择器    | 示例           | 说明                        |
| --------- | -------------- | --------------------------- |
| :enabled  | input:enabled  | 选择每个启用的 input 元素   |
| :disabled | input:disabled | 选择每个禁用的 input 元素   |
| :checked  | input:checked  | 选择每个被选中的 input 元素 |
| :required | input:required | 包含`required`属性的元素    |
| :optional | input:optional | 不包含`required`属性的元素  |
| :valid    | input:valid    | 验证通过的表单元素          |
| :invalid  | input:invalid  | 验证不通过的表单            |

#### 字符伪类

| 状态           | 示例           | 说明                            |
| -------------- | -------------- | ------------------------------- |
| ::first-letter | p:first-letter | 选择每个 p 元素的首字母         |
| ::first-line   | p:first-line   | 选择每个 p 元素的首行           |
| ::before       | p:before       | 在每个 p 元素的内容之前插入内容 |
| ::after        | p:after        | 在每个 p 元素的内容之后插入内容 |

##### 添加属性内容

> CSS 表达式 `attr()` 用来获取选择到的元素的某一 HTML 属性值，并用于其样式。它也可以用于伪元素，属性值采用伪元素所依附的元素。

```html
h2::before {
	content: attr(title);
}
...

<h2 title="后盾人">houdunren.com</h2>
```

### 元素权重

#### 权重的应用

| 规则            | 粒度 |
| --------------- | ---- |
| ID              | 0100 |
| class，类属性值 | 0010 |
| 标签,伪元素     | 0001 |
| *               | 0000 |
| 行内样式        | 1000 |

#### 继承规则

子元素可以继承父元素设置的样式，但是高度、边框等并不会继承，继承的规则没有权重



### 文本样式

#### 单词内断行

#### 大小写转换

小写转大写(将小写字母转换为小型大写字母；)

```html
span {
font-variant: small-caps;
}
 <span>houdunren.com</span>
```

首字母大写(只要不是连着的单词都会把首字母大写，除了_连接的)

```html
text-transform: capitalize;
```

全部大写

```html
text-transform: uppercase;
```

全部小写

```css
text-transform: lowercase;
```

#### 阴影控制

参数顺序为：颜色，水平偏移，垂直偏移，模糊度。

```html
<style>
  h2 {
  	text-shadow: rgba(13, 6, 89, 0.8) 3px 3px 5px;
  }
</style>
```

#### 空白处理

使用 `white-space` 控制文本空白显示。

| 选项     | 说明                                              |
| -------- | ------------------------------------------------- |
| pre      | 保留文本中的所有空白，类似使用 pre 标签，不会换行 |
| nowrap   | 禁止文本换行                                      |
| pre-wrap | 保留空白，保留换行符，会换行                      |
| pre-line | 空白合并，保留换行符，会换行                      |

#### 文本溢出

##### 单行文本

溢出后换行

```css
h2 {
  overflow-wrap: break-word;
  width: 100px;
	border: solid 1px #ddd;
}
```

溢出后末尾添加``...``

```css
<style>
h2{
    width:200px;
    border:1px solid blueviolet;
    overfloat:hidden;
    white-space:nowrap;
    text-overflow:ellipsis;
}
</style>
```

##### 多行文本

多行文本溢出时添加``...``

```css
<style>
div{
    width:200px;
    border:1px solid #ddd;
    overflow:hidden; // 超出文本隐藏
    display:-webkit-box; // 将对象作为弹性伸缩盒子模型显示
    -webkit-box-orient:vertical; // 从上到下垂直排列子元素(设置伸缩盒子的子元素排列方式)
    -webkit-line-clamp:2; // 这个属性不是css的规范属性，需要组合上面两个属性，表示显示的行数
}
</style>
```

##### 表格文本溢出

表格文本溢出控制需要为tale标签定义``table-layout:fixed;``样式，表示列宽是由表格和单元格宽度定义

```css
<style>
table{
    table-layout:fixed;
}
table tbody tr td {
    white-space:nowrap;
    overflow:hiddden;
    text-overflow:ellipsis;
}
</style>
```

#### 垂直对齐

使用``vertical-align``用于定义内容的垂直对其风格，包括``middle | baseline | sub | super``等，也可以使用数值`` vertical-align: 10px;``

#### 排版模式

| 模式          | 说明                                     |
| ------------- | ---------------------------------------- |
| horizontal-tb | 水平方向自上而下的书写方式               |
| vertical-rl   | 垂直方向自右而左的书写方式               |
| vertical-lr   | 垂直方向内内容从上到下，水平方向从左到右 |

### 盒子模型

#### 外边距

##### 边距合并

这种现象发生在两个并列的元素之间。给一个元素设置下外边距（margin-bottom），并同时给一个元素设置上外边距（margin-top）。两个元素之间的距离不等于这两个外边距之和，而是等于其中最大的一个外边距。

```css
<style>
	h2 {
    border: solid 2px green;
    margin-bottom: 20px;
  }

  h3 {
    border: solid 2px green;
    height: 20px;
  }
</style>
...
    <!-- h2与h3的外边距会合并，两者的间隔是20px  -->
<h2>hdcms.com</h2>
<h3></h3>
```

解决方法：

* **解决方案一：只设置其中一个元素的margin值即可（推荐）**
* **解决方案二：给每一个元素添加父元素，然后触发BFC规则（不推荐）**

##### 外边距塌陷

两个嵌套关系的（父子关系）块元素，当父元素有上外边距或者没有上外边距（margin-top），子元素也有上外边距的时候。两个上外边距会合成一个上外边距，以值相对较大的上外边距值为准。

解决方法：

* **1、给父元素设置外边框（border）或者内边距（padding）(不建议)**
* **触发BFC（推荐）**

> BFC：Block Formatting Context，块级格式化上下文，BFC决定了元素对其内容定位，以及当前元素与其他元素之间的关系和相互作用。其目的就是形成一个独立的空间，让空间中的子元素不会影响到这个独立空间之外的布局。

* 主要解决方案如下：
  * 子元素或者父元素的**float**不为**none**
  * 子元素或者父元素的**position**不为**relative**或**static**
  * 父元素的**overflow**为**auto**或**scroll**或**hidden**
  * 父元素的**display**的值为**table-cell**或**inline-block**

#### 边框设计

##### 样式设计

| 类型   | 描述                                                  |
| :----- | :---------------------------------------------------- |
| none   | 定义无边框。                                          |
| dotted | 定义点状边框。在大多数浏览器中呈现为实线。            |
| dashed | 定义虚线。在大多数浏览器中呈现为实线。                |
| solid  | 定义实线。                                            |
| double | 定义双线。双线的宽度等于 border-width 的值。          |
| groove | 定义 3D 凹槽边框。其效果取决于 border-color 的值。    |
| ridge  | 定义 3D 垄状边框。其效果取决于 border-color 的值。    |
| inset  | 定义 3D inset 边框。其效果取决于 border-color 的值。  |
| outset | 定义 3D outset 边框。其效果取决于 border-color 的值。 |

#### 轮廓线

元素在获取焦点时产生，并且轮廓线不占用空间。可以使用伪类 `:focus` 定义样式。

- 轮廓线显示在边框外面
- 轮廓线不影响页面布局

组合定义

```css
outline: red solid 2px;
```

取消表单轮廓线

```css
input:focus {
	outline: none;
}
```

#### display 与 visibility

**display:none;**

使用该属性后，HTML元素（对象）的宽度、高度等各种属性值都将“丢失”;该元素的位置被其他元素顶替

**visibility:hidden;**

使用该属性后，HTML元素（对象）仅仅是在视觉上看不见（完全透明），而它所占据的空间位置仍然存在，也即是说它仍具有高度、宽度等属性值。

#### 尺寸定义

| 选项           | 说明                         |
| -------------- | ---------------------------- |
| width          | 宽度                         |
| height         | 高度                         |
| min-width      | 最小宽度                     |
| min-height     | 最小高度                     |
| max-width      | 最大宽度                     |
| max-height     | 最大高度                     |
| fill-available | 撑满可用的空间               |
| fit-content    | 根据内容适应尺寸             |
| min-content    | 将容器尺寸按最小元素宽度设置 |
| max-content    | 容器尺寸按子元素最大宽度设置 |

### 背景处理

#### 背景裁切

| 选项        | 说明                 |
| ----------- | -------------------- |
| border-box  | 包括边框             |
| padding-box | 不含边框，包括内边距 |
| content-box | 内容区域             |

```css
background-clip: border-box;
```

#### 背景滚动

用于设置在页面滚动时的图片处理方式

| 选项   | 说明     |
| ------ | -------- |
| scroll | 背景滚动 |
| fixed  | 背景固定 |

```text
background-attachment: fixed;
```

#### 多个背景

后定义的背景置于底层

```text
background-image: url(xj-small.png), url(bg.png);
```

多属性定义

```text
background-image: url(xj-small.png), url(bg.png);
background-repeat: no-repeat;
background-position: top left, right bottom;
```

可以一次定义多个背景图片。

```text
background: url(xj-small.png) left 50% no-repeat,
                url(bg.png) right 100% no-repeat red;
```

#### 盒子阴影

可以使用 `box-shadow` 对盒子元素设置阴影，参数为 `水平偏移,垂直偏移,模糊度,颜色` 构成。

```css
box-shadow:10px 10px 5px rgba(100 , 100 , 100 , .5)
```

#### 颜色渐变

##### 线性渐变

激变一般用在背景颜色中使用(默认是从上到下)

```text
background: linear-gradient(red, green);
```

渐变角度

```text
background: linear-gradient(30deg, red, green);
```

设置多个颜色

```text
background: linear-gradient(red, rgb(0, 0, 200), green, rgba(122, 211, 100, 0));
```

##### 径向渐变

设置渐变

```text
background: radial-gradient(red, blue, green);
```

设置渐变宽度与高度

```text
background: radial-gradient(100px 200px, red, blue, green);
```

左下渐变

```text
background: radial-gradient(at bottom left, red, blue)
```

##### 标识位

颜色未指定标识位时，颜色会平均分配

如果标识位相同，颜色的变化将没有过渡效果

```css
background: linear-gradient(45deg, red 50%, blue 50%);
```

红色与蓝色将在50%到60%间发生激变

```text
background: linear-gradient(45deg, red 50%, blue 60%);
```

利用径向渐变生成小太阳效果

```css
width: 200px;
height: 200px;
background: radial-gradient(red 0, yellow 30%, black 60%, black 100%);
```

通过在两个颜色间中间点定义过渡位置

```text
background: linear-gradient(45deg, red, 50%, blue);
```

##### 渐变重复

```css
background: repeating-linear-gradient(90deg, blue 0, blue 25px, 25px, red 50px); 
background: repeating-radial-gradient(100px 100px, red 0%, yellow 20%); 
```

### 数据样式

#### 表格

##### 定制表格

除了使用 `table` 标签绘制表格外，也可以使用样式绘制。表格不能设置外边距。

| 样式规则           | 说明         |
| ------------------ | ------------ |
| table              | 对应 table   |
| table-caption      | 对应 caption |
| table-row          | 对表 tr      |
| table-cell         | 对应表td     |
| table-row-group    | 对应 tbody   |
| table-header-group | 对应 thead   |
| table-footer-group | 对应 tfoot   |

```html
  <style>
        * {
            padding: 0;
            margin: 0;
        }

        .table {
            display: table;
            text-align: center;
        }

        .table nav {
            display: table-caption;
        }

        .table section ul {
            display: table-row;
        }

        .table section ul li {
            display: table-cell;
            padding: 10px;
        }

        .table section:nth-of-type(1) {
            display: table-header-group;
        }

        .table section:nth-of-type(2) {
            display: table-row-group;
            border: solid 1px #ddd;
        }

        .table section:nth-of-type(3) {
            display: table-footer-group;
            background: #f3f3f3;
        }
    </style>
    <article class="table">
        <nav>后盾人在线教程</nav>
        <section>
            <ul>
                <li>标题</li>
                <li>说明</li>
            </ul>
        </section>
        <section>
            <ul>
                <li>后盾人</li>
                <li>houdunren.com</li>
            </ul>
            <ul>
                <li>开源系统</li>
                <li>hdcms.com</li>
            </ul>
        </section>
        <section>
            <ul>
                <li>不断更新视频</li>
                <li>努力加油</li>
            </ul>
        </section>
    </article>
```

##### 表格标题

可以通过``caption-side``设置标题的位置，值为``top | bottom``(top 表格的上方，bottom 表格的下方)

##### 内容对齐

水平对齐``text-align``

垂直对齐``vertical-align``

##### 表格间距

设置单元格间距

```css
table{
    border-spacing:50px 10px;
}
```

##### 边框合并

默认表格边框间是有间距的

```css
table{
    border-collapse:collapse;
}
```

##### 隐藏空的单元格

```css
table{
    empty-cells:hide;
}
```

#### 列表

##### 列表符号

使用 `list-style-type` 来设置列表样式，规则是继承的，所以在`ul` 标签上设置即可。

| 值                   | 描述                                                         |
| :------------------- | :----------------------------------------------------------- |
| none                 | 无标记。                                                     |
| disc                 | 默认。标记是实心圆。                                         |
| circle               | 标记是空心圆。                                               |
| square               | 标记是实心方块。                                             |
| decimal              | 标记是数字。                                                 |
| decimal-leading-zero | 0 开头的数字标记。(01, 02, 03, 等。)                         |
| lower-roman          | 小写罗马数字(i, ii, iii, iv, v, 等。)                        |
| upper-roman          | 大写罗马数字(I, II, III, IV, V, 等。)                        |
| lower-alpha          | 小写英文字母 The marker is lower-alpha (a, b, c, d, e, 等。) |
| upper-alpha          | 大写英文字母 The marker is upper-alpha (A, B, C, D, E, 等。) |
| lower-greek          | 小写希腊字母(alpha, beta, gamma, 等。)                       |
| lower-latin          | 小写拉丁字母(a, b, c, d, e, 等。)                            |
| upper-latin          | 大写拉丁字母(A, B, C, D, E, 等。)                            |
| hebrew               | 传统的希伯来编号方式                                         |
| armenian             | 传统的亚美尼亚编号方式                                       |
| georgian             | 传统的乔治亚编号方式(an, ban, gan, 等。)                     |
| cjk-ideographic      | 简单的表意数字                                               |
| hiragana             | 标记是：a, i, u, e, o, ka, ki, 等。（日文片假名）            |
| katakana             | 标记是：A, I, U, E, O, KA, KI, 等。（日文片假名）            |
| hiragana-iroha       | 标记是：i, ro, ha, ni, ho, he, to, 等。（日文片假名）        |
| katakana-iroha       | 标记是：I, RO, HA, NI, HO, HE, TO, 等。（日文片假名）        |

##### 符号的位置

控制符号显示在内容的外面还是内部

| 选项    | 说明 |
| ------- | ---- |
| inside  | 内部 |
| outside | 外部 |

```css
ul {
   list-style-position:inside; 
}
```

##### 指示器变背景图片

![](E:\静态页面\css\images\屏幕截图 2023-01-03 102338.png)

```text
ul li {
  background: url(xj-small.png) no-repeat 0 6px;
  background-size: 10px 10px;
  list-style-position: inside;
  list-style: none;
  text-indent: 15px;
}
```

##### 多图背景

![](E:\静态页面\css\images\屏幕截图 2023-01-03 102426.png)

```css
  ul {
            list-style-type: none;
        }

  ul li {
            background-image: url(xj-small.png), url(houdunren.jpg);
            background-repeat: no-repeat, repeat;
            background-size: 10px 10px, 100%;
            background-position: 5px 7px, 0 0;
            text-indent: 20px;
            border-bottom: solid 1px #ddd;
            margin-bottom: 10px;
            padding-bottom: 5px;
        }
```

#### 追加内容

##### 基本使用

使用伪类``::before``向前添加内容,``::after``向后添加内容

##### 提取属性

使用属性值添加内容，可以使用标准属性与自定义属性

```css
<style>
  a::after {
    content: attr(href);
  }
</style>

<a href="houdunren.com">后盾人</a>
```

通过属性值添加标签提示

```css
a {
    position: relative;
}

a:hover {
    &::before {
        content: "URL: "attr(data-url);
        background: #555;
        color: white;
        position: absolute;
        top: 20px;
        padding: 3px 10px;
        border-radius: 10px;
    }
}
```

##### 自定义简单表单

```css
<style>
    body {
        padding: 80px;
    }

    .field {
        position: relative;
    }

    input {
        border: none;
        outline: none;
    }

    .field::after {
        content: '';
        background: linear-gradient(to right, white, red, green, blue, white);
        height: 30px;
        position: relative;
        width: 100%;
        height: 1px;
        display: block;
        left: 0px;
        right: 0px;
    }

    .field:hover::before {
        content: attr(data-placeholder);
        position: absolute;
        top: -20px;
        left: 0px;
        color: #555;
        font-size: 12px;
    }
</style>

...
<div class="field" data-placeholder="请输入少于100字的标题">
    <input type="text" id="name">
</div>
```

### 浮动布局

浮动只会对后面的元素造成影响

元素浮动后会变为块元素包括行元素如 `span`，所以浮动后的元素可以设置宽高

##### 清除浮动

**CLEAR**

| 选项  | 说明               |
| ----- | ------------------ |
| left  | 左边远离浮动元素   |
| right | 右连远离浮动元素   |
| both  | 左右都远离浮动元素 |

在父元素内部的最后面添加一个没有高度的子元素，并使用``clear:both``

```css
<style>
	.clearfix {
      clear: both;
      height: 0;
  }

  div {
      width: 200px;
      height: 200px;
      margin-bottom: 10px;
  }


  div.green {
      border: solid 2px green;
      float: left;
  }

  div.red {
      border: solid 2px red;
      height: 200px;
      float: left;
  }

  div.blue {
      background: blue;
  }
</style>

<article>
        <div class="green"></div>
        <div class="red"></div>
        <div class="clearfix"></div>
</article>
<div class="blue"></div>
```

**AFTER**

使用``::after``伪类为父元素添加后标签，实现清楚浮动的影响

```css
article::after {
    content:"";
    display:block;
    clear:both;
}
```

**OVERFLOW**

子元素使用浮动后将不占用空间，这时父元素高度将为零。通过添加父元素并设置``overflow``属性可以清除浮动，将会使父元素产生``BFC``机制，即父元素的高度计算会包括浮动元素的高度

```css
article{
    overflow:hidden;
}
```

#### 形状浮动

通过形状浮动可以让内容围绕图片，类似于我们在 word 中的环绕排版。要求图片是有透明度的 PNG 格式。

##### 距离控制

``shape-outside``

| 选项        | 说明       |
| ----------- | ---------- |
| margin-box  | 外边距环绕 |
| padding-box | 内边距环绕 |
| border-box  | 边线环绕   |
| content-box | 内容环绕   |

```css
 shape-outside: margin-box;
```



##### 环绕模式

| 选项    | 说明     |
| ------- | -------- |
| circle  | 圆形环绕 |
| ellipse | 椭圆环绕 |
| url     | 图片环绕 |
| polygan | 多边环绕 |

```css
img {
    padding: 20px;
    float: left;
    shape-outside: circle(50%) padding-box;
}
```



##### 显示区域

``clip-path``

| 选项    | 说明   |
| ------- | ------ |
| circle  | 圆形   |
| ellipse | 椭圆   |
| polygon | 多边形 |



### 定位布局

#### 定位类型

| 选项     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| static   | 默认形为，参考文档流                                         |
| relative | 相对定位                                                     |
| absolute | 绝对定位(相对定位是相对于元素原来的位置控制，当元素发生位置偏移时，原位置留白。) |
| fixed    | 固定定位                                                     |
| sticky   | 粘性定位                                                     |



### 弹性布局

#### 弹性盒子

容器盒子里面包含着容器元素，使用 `声明块级弹性盒子display:flex` 或 `声明内联级弹性盒子display:inline-flex` 声明为弹性盒子。

##### flex-direction

用于控制盒子元素排列的方向

| 值             | 描述                           |
| :------------- | :----------------------------- |
| row            | 从左到右水平排列元素（默认值） |
| row-reverse    | 从右向左排列元素               |
| column         | 从上到下垂直排列元素           |
| column-reverse | 从下到上垂直排列元素           |

##### flex-wrap

flex-wrap属性规定flex容器是单行或者多行，同时横轴的方向决定了新行堆叠的方向

| 选项         | 说明                                             |
| :----------- | :----------------------------------------------- |
| nowrap       | 元素不拆行或不拆列（默认值）                     |
| wrap         | 容器元素在必要的时候拆行或拆列。                 |
| wrap-reverse | 容器元素在必要的时候拆行或拆列，但是以相反的顺序 |

##### flex-flow

``flex-flow`` 是 ``flex-direction`` 与 ``flex-wrap`` 的组合简写模式

从右向左排列，换行向上拆分行

```css
flex-flow: row-reverse wrap-reverse;
```

##### justify-content

用于控制元素再**主轴**上的排列方式

| 选项          | 说明                                                         |
| :------------ | :----------------------------------------------------------- |
| flex-start    | 元素紧靠主轴起点                                             |
| flex-end      | 元素紧靠主轴终点                                             |
| center        | 元素从弹性容器中心开始                                       |
| space-between | 第一个元素靠起点，最后一个元素靠终点，余下元素平均分配空间   |
| space-around  | 每个元素两侧的间隔相等。所以，元素之间的间隔比元素与容器的边距的间隔大一倍 |
| space-evenly  | 元素间距离平均分配                                           |

#### align-items与align-content

元素在交叉轴上有行的概念，理解这个概念会对理解 align-items 与 align-content 有更好的帮助。

- align-item 是控制**元素**在行上的排列
- align-content 是**控制行**在交差轴上的排列,**只适用多行并且有高度**的flex容器（也就是flex容器中的子项不止一行时该属性才有效果），它的作用是当flex容器在交叉轴上**有多余的空间**时，将子项作为一个整体（属性值为：flex-start、flex-end、center时）进行对齐。

##### align-items

| 选项       | 说明                           |
| :--------- | :----------------------------- |
| stretch    | 元素被拉伸以适应容器（默认值） |
| center     | 元素位于容器的中心             |
| flex-start | 元素位于容器的交叉轴开头       |
| flex-end   | 元素位于容器的交叉轴结尾       |

##### align-content

只适用于多行显示的弹性容器，用于控制行（而不是元素）在交叉轴上的排列方式。

| 选项          | 说明                                                         |
| :------------ | :----------------------------------------------------------- |
| stretch       | 将空间平均分配给元素                                         |
| flex-start    | 元素紧靠主轴起点                                             |
| flex-end      | 元素紧靠主轴终点                                             |
| center        | 元素从弹性容器中心开始                                       |
| space-between | 第一个元素靠起点，最后一个元素靠终点，余下元素平均分配空间   |
| space-around  | 每个元素两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍 |
| space-evenly  | 元素间距离平均分配                                           |

##### 案例

**单行，无论flex容器是否有高度，align-content都不会生效**

```html
<style>
    * {
        padding: 0;
        margin: 0;
        padding: 10px;
        margin: 5px;
    }

    .flex {
        width: 600px;
        height: 500px;
        margin: 10px;
        text-align: center;
        border: 2px solid #9a9a9a;
        display: flex;
        /* 默认的flex-direction为row，则交叉轴方向为column，即垂直方向*/
        align-content: center;
    }

    .flex div {
        width: 100px;
        margin: 5px;
    }

    .i1 {
        background-color: #ffb685;
        height: 130px;
    }

    .i2 {
        background-color: #fff7b1;
        height: 50px;
        width: 120px;
    }

    .i3 {
        background-color: #b1ffc8;
        height: 100px;
    }

    .i4 {
        background-color: #b1ccff;
        height: 60px;
    }
</style>

<body>
    <div class='flex'>
        <div class='i1'>1</div>
        <div class='i2'>2</div>
        <div class='i3'>3</div>
        <div class='i4'>4</div>
    </div>
</body>
```

**多行，如果flex容器没有设置高度，align-content也不会生效，因为没有剩余的空间去分配，只有在有高度的时候才会生效**

```html
<style>
    * {
        padding: 0;
        margin: 0;
        padding: 10px;
        margin: 5px;
    }

    .flex {
        width: 400px;
        height: 500px;
        margin: 10px;
        text-align: center;
        border: 2px solid #9a9a9a;
        display: flex;
        flex-wrap: wrap;
        /* 默认的flex-direction为row，则交叉轴方向为column，即垂直方向*/
        /* align-items 控制每一行的元素在行上的排列方式 */
        align-items: center;
        /* align-content 控制每一行的排列方式 */
        align-content: space-between;
    }

    .flex div {
        width: 100px;
        margin: 5px;
    }

    .i1 {
        background-color: #ffb685;
        height: 130px;
    }

    .i2 {
        background-color: #fff7b1;
        height: 50px;
        width: 120px;
    }

    .i3 {
        background-color: #b1ffc8;
        height: 100px;
    }

    .i4 {
        background-color: #b1ccff;
        height: 60px;
    }

    .i5 {
        background-color: #ffb1ee;
        height: 30px;
    }
</style>

<body>
    <div class='flex'>
        <div class='i1'>1</div>
        <div class='i2'>2</div>
        <div class='i3'>3</div>
        <div class='i4'>4</div>
        <div class='i5'>5</div>
    </div>
</body>
```

##### 总结

* align-items属性是针对单独的每一个flex子项起作用，它的基本单位是每一个子项，在所有情况下都有效果（当然要看具体的属性值）。
* align-content属性是将flex子项作为一个整体起作用，它的基本单位是子项构成的行，只在两种情况下有效果：①子项多行且flex容器高度固定 ②子项单行，flex容器高度固定且设置了flex-wrap:wrap;

#### 弹性元素

- 不能使用 float 与 clear 规则
- 弹性元素均为块元素
- 绝对定位的弹性元素不参与弹性布局

#### align-self

用于控制单个元素在交叉轴上的排列方式

| 选项       | 说明                   |
| :--------- | :--------------------- |
| stretch    | 将空间平均分配给元素   |
| flex-start | 元素紧靠主轴起点       |
| flex-end   | 元素紧靠主轴终点       |
| center     | 元素从弹性容器中心开始 |

#### flex与flex-grow与flex-shrink

flex 是 flex-grow、flex-shrink 、flex-basis 缩写组合。

flex: 1; 的值是 flex-grow: 1; flex-shrink: 1; flex-basis: 0%;

这是一个宽700px的弹性盒子，其中红黄蓝每个子元素的宽度都为100px。我们分别使用[flex](https://so.csdn.net/so/search?q=flex&spm=1001.2101.3001.7020)-grow和flex对子元素进行放大。

![](E:\静态页面\css\images\20200524200205922.png)

##### flex放大

![](E:\静态页面\css\images\2020052420005198.png)

使用flex
step1：计算剩余空间，剩余空间为弹性盒子剩余宽度与进行flex的子元素的宽度之和。
例中的剩余空间为：
进行flex的子元素宽度分别是100,100，所以剩余空间为600px  =  700px  -100px(黄)  -100px(蓝)。
step2: 按照进行flex属性值，数字的比例进行分配空间。黄色和蓝色的比例为1:2。因此其宽度分别为200px,400px。

##### 使用flex-grow放大

![](E:\静态页面\css\images\20200524201029521.png)

使用flex-grow
step1：计算剩余空间，剩余空间为弹性盒子剩余宽度
例中的剩余空间为：
剩余宽度为400px = 700px  -100px  -100px  -100px
step2: 按照进行flex属性值，数字的比例进行分配空间。黄色和蓝色的比例为1:2。因此其宽度分别为133.34px ,266.66px。
step3：元素自身的宽度加上分配到的剩余空间就是放大后的宽度。因此其最终显示出来宽度分别为233.34px ,366.66px。

##### flex-shrink

该属性定义空间不够时各个元素如何收缩，默认值为1；

flex-shrink 定义的仅仅只是元素宽度变小的一个权重分量，每个元素具体收缩多少，还有另一个重要的因素，即他本身的宽度。

例子：

第一种情况：flex-shrink之和大于1：

父元素 500px。三个子元素分别设置为 150px，200px，300px。

三个子元素的 flex-shrink 的值分别为 1，2，3。

1. 先计算溢出的部分	500px-150px-200px-300px= -150px
2. 计算总权重           1 x 150px+2 x 200px + 3 x 300px=1450px
3. 计算分别收缩        
   1. -150px(溢出)  x  1(flex-shrink)  x  150px(width)  /  1450px(总权重)=  -15.5
   2. -150px(溢出)  x  2(flex-shrink)  x  200px(width)  /  1450px(总权重)=  -41.4
   3. -150px(溢出)  x  3(flex-shrink)  x  300px(width)  /  1450px(总权重)=  -93.1
4. 最终宽度
   1. 150 - 15.5 = 134.5
   2. 200 - 41.4 = 158.6
   3. 300 - 93.1 = 206.9



当flex-shrink之和小于1

此时，并不会收缩所有的空间，而只会收缩 flex-shrink 之和相对于 1 的比例的空间。

还是上面的例子，但是 flex-shrink 分别改为 0.1，0.2，0.3。

于是总权重为 145（正好缩小 10 倍，略去计算公式）。

三个元素收缩总和并不是 150px，而是只会收缩 150px 的 (0.1 + 0.2 + 0.3) / 1 即 60% 的空间：90px。

每个元素收缩的空间为：

- 90 * 0.1(flex-shrink) * 150(width) / 145 = 9.31
- 90 * 0.2(flex-shrink) * 200(width) / 145 = 24.83
- 90 * 0.3(flex-shrink) * 300(width) / 145 = 55.86

三个元素的最终宽度分别为：

- 150 - 9.31 = 140.69
- 200 - 24.83 = 175.17
- 300 - 55.86 = 244.14



#### flex-basis

flex-basis 属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。

可以是长度单位，也可以是百分比。`flex-basis`的优先级高于`width、height`属性。



#### order

用于控制弹性元素的位置，默认为 `order:0` 数值越小越在前面，可以负数或整数。

```html
<style>
    * {
        padding: 0;
        margin: 0;
    }

    body {
        padding-left: 50px;
        padding-top: 15px;
    }

    article {
        border: solid 5px silver;
        width: 400px;
        height: 400px;
        padding: 10px;
        display: flex;
        flex-direction: column;

    }

    article section {
        order: 1;
        flex: 1 0 100px;
        padding: 10px;
        background: blueviolet;
        background-clip: content-box;
        display: flex;
        flex-direction: column;
        text-align: center;
        color: white;
    }

    article section div {
        flex: 1;
    }

    article section div {
        display: flex;
        flex-direction: column;
        justify-content: center;
    }

    article section span {
        flex: 0;
        background: #000;
        padding: 20px;
        cursor: pointer;
    }
</style>

<article>
    <section>
        <div>houdunren.com</div>
        <span onclick="up(this)">up</span>
    </section>
    <section>
        <div>hdcms.com</div>
        <span onclick="up(this)">up</span>
    </section>
</article>

<script>
    function up(el) {
        el.parentElement.style.order = getOrder(el.parentElement) * 1 - 1;
        console.log(getOrder(el.parentElement))
    }

    function getOrder(el) {
        return getComputedStyle(el, null).order;
    }
</script>
```

#### 绝对定位

绝对定位的弹性元素不参与弹性布局

#### 自动空间

在弹性布局中对元素使用`margin-right:auto` 等形式可以自动撑满空间。下例为第一个 ul 设置 `margin-right:auto` 表示右侧空间自动撑满，第二个 ul 靠近父元素右边界。



### 栅格系统

#### 声明容器

##### 块级容器

``display:grid``

##### 行级容器

``display:inline-grid``

#### 划分行列

栅格有点类似表格，也分行和列，使用``grid-template-columns``规则可划分列数，使用``grid-template-rows``划分行数

两行两列，当容器宽度过大时将留白

```html
article{
width:300px;
height:200px;
display:grid;
grid-template-rows:100px 100px;
grid-template-colums:100px 100px;
}
```

##### 也可以使用百分比单位

```html
{
display:grid;
grid-template-rows:50% 50%;
grid-template-colums:25% 25% 25%;
}
```

##### 重复设置

使用关键词``repeat``统一设置值，第一个参数为重复数量，第二个参数为重复值

```html
   		width: 300px;
        height: 200px;
        border: solid 5px silver;
        display: grid;
        /* 改变的是高度 三行，每行30%的高*/
        grid-template-rows: repeat(3, 30%);
        /* 改变的是宽度  两列，每列100px宽*/
        grid-template-columns: repeat(2, 100px);
```

可以设置多个值来定义重复，

```html
 
grid-template-rows: repeat(2, 50%);
/* 重复两次，每一次重复包含两列，一列50px宽，另一列100px宽 */
grid-template-columns: repeat(2, 50px 100px);
```

##### 自动填充

```html
        grid-template-rows: repeat(auto-fill, 50px);
        grid-template-columns: repeat(auto-fill, 150px);
```

##### 比例划分

```html
/* 高度1:2 */        
grid-template-rows: 1fr 2fr;
/* 宽度1:2:3 */
grid-template-columns: 1fr 2fr 3fr;
```

##### 比例划分与重复定义

```html
grid-template-rows:repeat(2,1fr);
grid-template-columns:repeat(2,1fr 2fr);
```

##### 自动空间

中间一列自动分配空间

```html
grid-template-rows:repeat(2,50%)；
grid-template-columns:10% auto 20%;
```

##### grid-template-areas

一般与``grid-area``组合使用

```html
<section id="page">
  <header>Header</header>
  <nav>Navigation</nav>
  <main>Main area</main>
  <footer>Footer</footer>
</section>

<style>
#page {
  display: grid; /* 1.设置 display 为 grid */
  width: 100%;
  height: 250px;
  grid-template-areas: "head head"
                       "nav  main"
                       "nav  foot"; /* 2.区域划分 当前为 三行 两列 */
  grid-template-rows: 50px 1fr 30px; /* 3.各区域 宽高设置 */
  grid-template-columns: 150px 1fr;
}

#page > header {
  grid-area: head; /* 4. 指定当前元素所在的区域位置，从 grid-template-areas 选取值 */
  background-color: #8ca0ff;
}

#page > nav {
  grid-area: nav;
  background-color: #ffa08c;
}

#page > main {
  grid-area: main;
  background-color: #ffff64;
}

#page > footer {
  grid-area: foot;
  background-color: #8cffa0;
}
</style>
```

##### 组合定义

``grid-template``是``grid-template-rows``、``grid-template-columns``、``grid-template-areas``三个属性的简写

```html
/* 两行三列 */
 grid-template: repeat(2, 100px) / repeat(3, 100px);
```

##### minmax划分

假设每一行的栅格元素单元格总宽度是`400px`，栅格容器的宽度却只有`300px`，造成了列溢出，如果设置`minmax`则可以让每个单元格进行适当调整

```css
　grid-template: repeat(2,100px)/repeat(4,minmax(50px,100px));
```

#### 间距定义

##### 行间距

栅格容器中每一个元素，现在看是紧密挨在一起的。我们可以对栅格元素本身进行`margin`或者`padding`来将彼此之前撑开留出间隙，也可以使用栅格容器提供的方法。

```css
row-gap:30px;
```

##### 列间距

```css
column-gap:30px;
```

##### 组合定义

使用`gap`规则可以一次定义行、列间距

```css
/* 行间距20px 列间距10px */
gap:20px 10px;
```

#### 栅格线命名

![](E:\静态页面\css\images\屏幕截图 2023-01-12 160333.png)

##### 独立命名

![](E:\静态页面\css\images\屏幕截图 2023-01-12 160138.png)

```html
<style>
    * {
        padding: 0;
        margin: 0;
    }

    body {
        padding: 200px;
    }

    article {
        width: 300px;
        height: 300px;
        border: solid 5px silver;
        display: grid;
        /* 栅格线命名 */
        grid-template-rows: [r1-start] 100px [r1-end r2-start] 100px [r2-end r3-start] 100px [r3-end];
        grid-template-columns: [c1-start] 100px [c1-end c2-start] 100px [c2-end c3-start] 100px [c3-end];
    }

    div {
        background-color: blueviolet;
        background-clip: content-box;
        padding: 10px;

        /* 在中间 */
        grid-row-start: r2-start;
        grid-row-end: r2-end;
        grid-column-start: c2-start;
        grid-column-end: c2-end;
    }
</style>

<body>
    <article>
        <div></div>
    </article>
</body>
```

##### 自动命名

对于重复设置的栅格容器系统会为它自动命名，使用时使用 `r1、c2` 的方式定位栅格。

> 　　r代表行
>
> 　　c代表列

　　重复设置，命名前缀：

> 　　grid-template-rows: repeat(3, [r-start] 100px [r-end]);
>
> 　　grid-template-columns: repeat(3, [c-start] 100px [c-end]);

　　在使用自动命名的栅格线进行定位时，应该按照如下格式：

> 　　grid-row-start: r-start 2;
>
> 　　grid-column-start: c-start 2;
>
> 　　grid-row-end: r-start 2;
>
> 　　grid-column-end: c-end 2;



#### 元素定位

| 选项              | 说明         |
| ----------------- | ------------ |
| grid-row-start    | 行开始栅格线 |
| grid-row-end      | 行结束栅格线 |
| grid-column-start | 列开始栅格线 |
| grid-column-end   | 列结束栅格线 |

##### 栅格线位置定位

规定行开始线位置与行结束线位置以及列开始线位置和列结束线位置。

```css
grid-template-rows: repeat(3, 100px);
grid-template-columns: repeat(3, 100px);

grid-row-start: 2;
grid-column-start: 2;
grid-row-end: 2;
grid-column-end: 2;
```

##### 栅格线自定义名称定位

```css
grid-template-rows: [r1-start] 100px [r1-end r2-start] 100px [r2-end r3-start] 100px [r3-end];
grid-template-columns: [c1-start] 100px [c1-end c2-start] 100px [c2-end c3-start] 100px [c3-end];

grid-row-start: r2-start;
grid-column-start: c1-end;
grid-row-end: r2-end;
grid-column-end: c3-start;
```

##### 栅格线自动名称定位

```css
grid-template-rows: repeat(3, [r-start] 100px [r-end]);
grid-template-columns: repeat(3, [c-start] 100px [c-end]);

grid-row-start: r-start 2;
grid-column-start: c-start 2;
grid-row-end: r-start 2;
grid-column-end: c-end 2;
```

##### 偏移量定位

我们只需要指定行线的开始位置以及列线的开始位置即可，关于结束为止也是数数，使用`span`来数，往后走一根线还是两根线。

```html
grid-template-rows: repeat(3, 100px);
grid-template-columns: repeat(3, 100px);

/* 全占 */
grid-row-start: 1;
grid-column-start: 1;
/* 代表从行线开始位置向后数3根线 */
grid-row-end: span 3;
/* 代表从列线开始位置向后数3根线 */
grid-column-end: span 3;
```

##### 简写模式

**`grid-row`** 属性是一种 [`grid-row-start` 和 [`grid-row-end` 的缩写形式

**`grid-column`** 属性是一种 [`grid-column-start` 和 [`grid-column-end` 的缩写形式

```css
　　grid-row: 3/span 3;

　　grid-column: 2/span 3;
```

##### grid-area 极简模式

　　`grid-area`是对上面的简写模式`grid-row`以及`grid-column`的再次简写，它的语法结构如下：

```css
　　行线开始/列线开始/行线结束/列线结束

　　grid-area: 2/1/span 1/span 3;
```

#### 栅格流动

在容器中设置`grid-auto-flow` 属性可以改变单元格排列方式。

| 选项   | 说明                                   |
| ------ | -------------------------------------- |
| column | 按列排序                               |
| row    | 按行排列                               |
| dense  | 元素使用前面空余栅格（下面有示例说明） |

##### 基本使用

按列排列

```js
<style>
  article {
      width: 400px;
      height: 400px;
      display: grid;
      grid-template-rows: repeat(2, 1fr);
      grid-template-columns: repeat(2, 1fr);
      border: solid 5px silver;
      grid-auto-flow: column;
  }

  div {
      background: blueviolet;
      background-clip: content-box;
      padding: 10px;
      font-size: 35px;
      color: white;
  }
</style>
...

<article>
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <div>4</div>
</article>
```

##### 强制填充

当元素在栅格系统中放不下的时候，会发生换行，使用``grid-auto-flow:row dense``可以让元素强制不换行

```html
<style>
    * {
        padding: 0;
        margin: 0;
    }

    body {
        padding-left: 200px;
        padding-top: 200px;
    }

    article {
        width: 600px;
        height: 600px;
        display: grid;
        grid-template-rows: repeat(3, 200px);
        grid-template-columns: repeat(3, 200px);
        border: solid 5px silver;
        grid-auto-flow: row dense;
    }

    div {
        background: blueviolet;
        background-clip: content-box;
        padding: 10px;
        font-size: 35px;
        color: white;
    }

    article div:nth-child(1) {
        grid-column: 1/span 2;
        background: #000;
    }

    article div:nth-child(2) {
        grid-column: 2/span 1;
    }
</style>
...

<article>
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <div>4</div>
</article>
```

#### 对齐管理

可以通过属性方便的定义栅格或元素的对齐方式

| 选项            | 说明                                             | 对象     |
| --------------- | ------------------------------------------------ | -------- |
| justify-content | 所有栅格在容器中的水平对齐方式，容器有额外空间时 | 栅格容器 |
| align-content   | 所有栅格在容器中的垂直对齐方式，容器有额外空间时 | 栅格容器 |
| align-items     | 栅格内所有元素的垂直排列方式                     | 栅格容器 |
| justify-items   | 栅格内所有元素的横向排列方式                     | 栅格容器 |
| align-self      | 元素在栅格中垂直对齐方式                         | 栅格元素 |
| justify-self    | 元素在栅格中水平对齐方式                         | 栅格元素 |

##### 栅格对齐

justify-content 与 align-content 用于控制栅格的对齐方式，比如在栅格的尺寸小于容器的心里时，控制栅格的布局方式。

属性的值如下

| 值            | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| start         | 容器左边                                                     |
| end           | 容器右边                                                     |
| center        | 容器中间                                                     |
| stretch       | 撑满容器                                                     |
| space-between | 第一个栅格靠左边，最后一个栅格靠右边，余下元素平均分配空间   |
| space-around  | 每个元素两侧的间隔相等。所以，栅格之间的间隔比栅格与容器边距的间隔大一倍 |
| space-evenly  | 栅格间距离完全平均分配                                       |

![](D:\后盾人\css\images\image-20190829231259807.2b1db179.png)

```html
    article {
        border: solid 5px silver;
        width: 600px;
        height: 600px;
        display: grid;
        grid-template-columns: 200px 200px;
        grid-template-rows: 200px 200px;
        justify-content: space-between;
        align-content: space-evenly;
        align-items: end;
        justify-items: center;


    }

    div {
        background: blueviolet;
        background-clip: content-box;
        border: 5px dotted #ccc;
        padding: 20px;
        font-size: 35px;
        color: white;
    }

 <article>
        <div>1</div>
        <div>2</div>
        <div>3</div>
        <div>4</div>
</article>
```

##### 元素对齐

justify-items 与 align-items 用于控制所有栅格内元素的对齐方式

| 值      | 说明               |
| ------- | ------------------ |
| start   | 元素对齐栅格的左边 |
| end     | 元素对齐栅格的右边 |
| center  | 元素对齐栅格的中间 |
| stretch | 水平撑满栅格       |

```html
    article {
        border: solid 5px silver;
        width: 600px;
        height: 600px;
        display: grid;
        grid-template-columns: 200px 200px;
        grid-template-rows: 200px 200px;
        justify-content: space-between;
        align-content: space-evenly;
        align-items: end;
        justify-items: center;
    }
```

##### 元素的独立控制

justify-self 与 align-self 控制单个栅格内元素的对齐方式，属性值与 justify-items 和 align-items 是一致的。

会覆盖justify-items 和 align-items 的效果

```html
div:first-child {
  justify-self: end;
  align-self: center;
}

div:nth-child(4) {
  justify-self: start;
  align-self: center;
}
```

#### 组合简写

#### place-content

用于控制栅格的对齐方式，语法如下：

```text
place-content: <align-content> <justify-content>
```

#### place-items

控制所有元素的对齐方式，语法结构如下：

```text
place-items: <align-items> <justify-items>
```

#### place-self

控制单个元素的对齐方式

```text
place-self: <align-self> <justify-self>
```

####  自动排列

当栅格无法放置内容时，系统会自动添加栅格用于放置溢出的元素，我们需要使用以下属性控制自动添加栅格的尺寸。

##### 属性说明

| 选项              | 说明                                                   | 对象 |
| ----------------- | ------------------------------------------------------ | ---- |
| grid-auto-rows    | 控制自动增加的栅格行的尺寸，grid-auto-flow:row; 为默认 | 容器 |
| grid-auto-columns | 控制自动增加的栅格列的尺寸，grid-auto-flow: column;    | 容器 |

##### 自动栅格行

下面定义了 2X2 的栅格，但有多个元素，系统将自动创建栅格用于放置额外元素。我们使用 grid-auto-rows 来控制增加栅格的行高。

![image-20201017142201592](https://doc.houdunren.com/assets/img/image-20201017142201592.6f7e0324.png)

```text
<style>
  main {
    display: grid;
    grid-template-rows: repeat(2, 50px);
    grid-template-columns: repeat(2, 1fr);
    grid-auto-rows: 50px;
    grid-auto-columns: 200px;
  }
  div {
    background: blueviolet;
    background-clip: content-box;
    border: solid 1px #ddd;
    color: white;
  }
</style>
<main>
  <div href="">我的音乐</div>
  <div href="">西方音乐</div>
  <div href="">北方音乐</div>
  <div href="">后盾人</div>
  <div href="">向军老师</div>
  <div href="">训练营</div>
</main>
```

##### 自动行列

下面创建了 2X2 栅格，我们将第 2 个 DIV 设置的格栅已经超过了四个栅格，所以系统会自动创建栅格。

![image-20201017142907082](https://doc.houdunren.com/assets/img/image-20201017142907082.74d40353.png)

```text
<style>
  main {
    display: grid;
    grid-template-rows: repeat(2, 50px);
    grid-template-columns: repeat(2, 1fr);
    grid-auto-columns: 10vw;
    grid-auto-rows: 10vh;
  }
  div {
    background: blueviolet;
    background-clip: content-box;
    border: solid 1px #ddd;
    color: white;
  }
  div:nth-child(2) {
    grid-area: 5/5/5/5;
  }
</style>
<main>
  <div href="">后盾人</div>
  <div href="">向军老师</div>
</main>
```

#### 终级简写

grid 是简写属性，可以用来设置：

- 显式网格属性 grid-template-rows、grid-template-columns 和 grid-template-areas，
- 隐式网格属性 grid-auto-rows、grid-auto-columns 和 grid-auto-flow，
- 间距属性 grid-column-gap 和 grid-row-gap

使用语法:

```text
<'grid-template'> | <'grid-template-rows'> / [ auto-flow && dense? ] <'grid-auto-columns'>? | [ auto-flow && dense? ] <'grid-auto-rows'>? / <'grid-template-columns'>
```

##### 行列划分

下面使用 grid 布局内容，将 body 容器的栅格居中排列，将 main 容器内的栅格内的元素居中排列。

![image-20201017171935040](https://doc.houdunren.com/assets/img/image-20201017171935040.c8734550.png)

```text
<style>
  body {
    display: grid;
    place-content: center center;
    width: 100vw;
    height: 100vh;
  }
  main {
    display: grid;
    grid: 10vh / repeat(4, 1fr);
    place-items: center center;
  }
  div {
    background-clip: content-box;
    border: solid 1px #ddd;
    color: white;
    padding: 10px;
    box-sizing: border-box;
  }
  div:nth-child(1) {
    background-color: #3498db;
  }
  div:nth-child(2) {
    background-color: #f1c40f;
  }
  div:nth-child(3) {
    background-color: #2ecc71;
  }
  div:nth-child(4) {
    background-color: #9b59b6;
  }
</style>
<main>
  <div href="">后盾人</div>
  <div href="">向军老师</div>
  <div href="">HDCMS</div>
  <div href="">后盾人</div>
</main>
```

##### 定义区域

使用 grid 也可以定义栅格区域

![image-20201017172625960](https://doc.houdunren.com/assets/img/image-20201017172625960.1082a8f5.png)

```text
<style>
  main {
    width: 100vw;
    height: 100vh;
    display: grid;
    grid:
      'header header' 50px
      'nav main' auto
      'footer footer' 60px/100px auto;
  }
  div {
    border: solid 1px #ddd;
    color: white;
    padding: 10px;
    box-sizing: border-box;
  }
  div:nth-child(1) {
    background-color: #3498db;
    grid-area: header;
  }
  div:nth-child(2) {
    background-color: #f1c40f;
    grid-area: nav;
  }
  div:nth-child(3) {
    background-color: #2ecc71;
    grid-area: main;
  }
  div:nth-child(4) {
    background-color: #9b59b6;
    grid-area: footer;
  }
</style>
<main>
  <div href="">后盾人</div>
  <div href="">向军老师</div>
  <div href="">HDCMS</div>
  <div href="">后盾人</div>
</main>
```



## 变形动画

### 基础知识

#### 坐标系统

- X 轴是水平轴
- Y 轴是垂直轴
- Z 轴是纵深轴

#### 变形操作

使用 `transform` 规则控制元素的变形操作，包括控制移动、旋转、倾斜、3D 转换等，下面会详细介绍每一个知识点。

下面是 CSS 提供的变形动作。

| 选项                          | 说明                                  |
| ----------------------------- | ------------------------------------- |
| none                          | 定义不进行转换。                      |
| translate(*x*,*y*)            | 定义 2D 转换。                        |
| translate3d(*x*,*y*,*z*)      | 定义 3D 转换。                        |
| translateX(*x*)               | 定义转换，只是用 X 轴的值。           |
| translateY(*y*)               | 定义转换，只是用 Y 轴的值。           |
| translateZ(*z*)               | 定义 3D 转换，只是用 Z 轴的值。       |
| scale(*x*,*y*)                | 定义 2D 缩放转换。                    |
| scale3d(*x*,*y*,*z*)          | 定义 3D 缩放转换。                    |
| scaleX(*x*)                   | 通过设置 X 轴的值来定义缩放转换。     |
| scaleY(*y*)                   | 通过设置 Y 轴的值来定义缩放转换。     |
| scaleZ(*z*)                   | 通过设置 Z 轴的值来定义 3D 缩放转换。 |
| rotate(*angle*)               | 定义 2D 旋转，在参数中规定角度。      |
| rotate3d(*x*,*y*,*z*,*angle*) | 定义 3D 旋转。                        |
| rotateX(*angle*)              | 定义沿着 X 轴的 3D 旋转。             |
| rotateY(*angle*)              | 定义沿着 Y 轴的 3D 旋转。             |
| rotateZ(*angle*)              | 定义沿着 Z 轴的 3D 旋转。             |
| skew(*x-angle*,*y-angle*)     | 定义沿着 X 和 Y 轴的 2D 倾斜转换。    |
| skewX(*angle*)                | 定义沿着 X 轴的 2D 倾斜转换。         |
| skewY(*angle*)                | 定义沿着 Y 轴的 2D 倾斜转换。         |
| perspective(*n*)              | 为 3D 转换元素定义透视视图。          |

### 移动元素

- 沿 X 轴移动时正值向右移动、负值向左移动
- 沿 Y 轴移动时正值向下移动、负值向上移动
- 如果使用百分数将控制元素的原尺寸计算百分比然后移动
- 可同时设置多个值，解析器会从左向右依次执行
- 变形是在原基础上更改，即第二次设置值时不是在第一次值上变化

























## 响应式布局

### 媒体查询

```css
针对屏幕设备
<style media="screen"></style>
<link rel="stylesheet" href="css/screen.css" media="screen" />
针对打印机设备
<style media="print"></style>
<link rel="stylesheet" href="css/print.css" media="print" />
针对全部设备
<style></style>
<link rel="stylesheet" href="css/common.css" media="all" />
```

#### 使用@import简化页面多文件引入

```css
@import url(common.css) all;  all可以不加
@import url(screen.css) screen;
@import url(print.css) print;
```

#### 样式表中使用@media条件判断

```css
<style media="screen and (min-width:768px) and (max-width:1000px)"> (768px<=width<=1000px) </style>
<style media="screen and (max-width:768)">
768这1象素归样式
</style> 
```

#### 逻辑或（，）

横屏(orientation:landscape)或者>=768px应用该样式

```css
<style media="screen and (orientation:landscape),screen and (min-width:768px)"></style>
```

#### 逻辑非(not)

后面的条件如果满足了就不使用该样式

```css
<style>
@media not screen and (min-width:500px) and (max-width:768px){
    
}
</style>
```

#### 使用only排除低端浏览器

```css
<style>
@media only screen and (min-width:500px){}
</style>
```



