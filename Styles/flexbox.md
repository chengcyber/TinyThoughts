#  Flexbox

## 什么是 Flexbox ？

	Flexbox能够一种便捷的方式来布局，对齐，分散元素间的空隙，即使在不知道视图大小，或者页面含有未知的、动态元素。





## 开启 Flexbox 旅程

  第一步，我们需要一个 flex-container，用来作为 flexbox 的元素容器，容器中的子元素视作 flex-item

举例来说：

```html
    <ul> <!-- parent element -->
      <li></li> <!-- first child element -->
      <li></li> <!-- first child element -->
      <li></li> <!-- first child element -->
    </ul>
```
需要先将父元素 ul 设置为 flex-container

`display: flex` 或者 `display: inline-flex`

```
/* Make parent element a flex container */
ul {
    display: flex; /* or inline-flex */
}
```
为了方便观察，为 li 元素添加如下样式


```
li {
  width: 100px;
  height: 100px;
  background-color: #8cacea;
  margin: 8px;
}
```
得到下面的效果，表示 flexbox 已经被使用

![Flex actived](http://ojmbu5nqv.bkt.clouddn.com/0-XthcMWtWfUIEmsgR.png)

- flex-container: The parent element you’ve set `display: flex` on.
- flex-items: The children elements within a Flex container.

![](http://ojmbu5nqv.bkt.clouddn.com/flex_twokeyword.png)

## Flex container 成员
- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

### 1.Flex-direction

```
/*where ul represents a flex container*/
ul {
  flex-direction: row || column || row-reverse || column-reverse;
  }
/* default row */
```
通俗地说，flex-direction 的作用是控制元素垂直/水平排列。但是在 flexbox 的世界观里，应该称作 主坐标轴(main-axis) 和 纵坐标轴(cross-axis)。

![Default main and cross axis](http://ojmbu5nqv.bkt.clouddn.com/flex_main_axis.png)

flex-direction 的默认值为 row, 即行被设置为 main axis，就是我们所说的水平排列

如果 flex-direction 设置为 column 呢
![](http://ojmbu5nqv.bkt.clouddn.com/0-bID-FjxGy9wVwanl.png)

这样就将 flex-items 水平排列

### 2.Flex-wrap

```
//where ul represents a flex container
ul {
  flex-wrap: wrap || no-wrap || wrap-reverse;
  }
/* default no-wrap */
```
要说 `flex-wrap` 的作用，我们可以在第一个例子中，多添加几个 `li` 元素来观察。

```
/* 10 li elements*/
<ul> <!--parent element-->
  <li></li> <!--first child element-->
  <li></li> <!--second child element-->
  <li></li> <!--third child element-->
  ...
  <li></li>
  <li></li>
  <li></li>
</ul>
```
![after add to 10 items](http://ojmbu5nqv.bkt.clouddn.com/flex_wrap_10item.png)
可以看到元素挤压了自身来容纳新增加的元素。
现在，我们来设置 `flex-wrap`

```
ul {
    flex-wrap: no-wrap; 
    /*Keep on taking more flex items without breaking (wrapping)*/
}
```
![flex-wrap initiated](http://ojmbu5nqv.bkt.clouddn.com/flex-wrap-initiated.png)
通过将 `flex-wrap` 设置为 `no-wrap`, 元素就能自动换行了

再来试试看 `no-wrap-reverse`
![flex-items wrap in reverse](http://ojmbu5nqv.bkt.clouddn.com/flex-items%20wrap%20in%20reverse.png)

### 3.Flex-flow

```
ul {
    flex-flow: row wrap; /*direction "row" and yes, please wrap the items.*/
}
```
`flex-flow` 是 `flex-direction` 和 `flex-wrap` 的缩写形式，就像 `border: 1px solid red`

![flex-flow broken down in bits](http://ojmbu5nqv.bkt.clouddn.com/flex-flow%20broken%20down%20in%20bits.png)

只要是 `flex-direction` 和 `flex-wrap` 的可能值都可以被使用。

### 4.Justify-content

```
ul {
    justify-content: flex-start || flex-end || center || space-between || space-around
}
/* default flex-start */
```
`justify-content` 是用来控制 `main-axis` 的对齐方式

#### 4.1 Flex-start
默认值，将 `flex-items` 都聚集在 `main-axis` 开头

```
ul {
    justify-content: flex-start;
  }
```
![justify-content: flex-start (default behavior)](http://ojmbu5nqv.bkt.clouddn.com/flex-justify-content.png)

#### 4.2 Flex-end
将 `flex-items` 聚集在 `main-axis` 结尾

```
ul {
    justify-content: flex-end;
  }
```
![justify-content: flex-end](http://ojmbu5nqv.bkt.clouddn.com/flex-justify-content-flex-end.png)

#### 4.3 Center
`flex-items` 居中显示

```
ul {
    justify-content: center;
  }
```
![justify-content: center
](http://ojmbu5nqv.bkt.clouddn.com/flex-justify-content-center.png)

#### 4.4 Space-between
`flex-items` 分散对齐

```
ul {
    justify-content: space-between;
  }
```
![justify-content: space-between](http://ojmbu5nqv.bkt.clouddn.com/flex-jusitfy-content-space-between.png)

注意元素之间空格间距是相同的
![](http://ojmbu5nqv.bkt.clouddn.com/flex-justify-content-space-between2.png)

#### 4.5 Space-around
`flex-items` 会被等距的空格围绕

```
ul {
    justify-content: space-around;
  }
```
![justify-content: space-around](http://ojmbu5nqv.bkt.clouddn.com/flex-justify-content-space-around.png)

等距的空格间距围绕
![](http://ojmbu5nqv.bkt.clouddn.com/flex-justify-content-space-around2.png)

### 5. Align-items
`align-items` 用来控制 `cross-axis` 的对齐方式，可以理解成 另一列的 `justify-content`，但是可用值是不同的。

```
/*ul represents any flex container*/
ul {
    align-items: flex-start || flex-end || center || stretch || baseline
}
/* default stretch */
```
#### 5.1 Stretch
默认值，元素会按照最高的元素高度填充

![align-items: stretch](http://ojmbu5nqv.bkt.clouddn.com/flex-align-items-stretch.png)

#### 5.2 Flex-start
顶端对齐
![align-items: flex-start](http://ojmbu5nqv.bkt.clouddn.com/flex-align-items-flex-start.png)

#### 5.3 Flex-end
底端对齐
![align-items: flex-end](http://ojmbu5nqv.bkt.clouddn.com/flex-align-items-flex-end.png)

#### 5.4 Center
居中对齐
![align-items: center](http://ojmbu5nqv.bkt.clouddn.com/flex-align-items-center.png)

#### 5.5 Baseline
基于基准线对齐
![align-items: baseline](http://ojmbu5nqv.bkt.clouddn.com/flex-align-item-baseline.png)

`baseline` 和 `flex-start` 十分相似，但有略微的不同。
什么是基准线对齐，请看下图
![](http://ojmbu5nqv.bkt.clouddn.com/flex-align-items-baseline1.png)

### 6. Align-content
`align-content` 是在 `flex-items` 被 `wrap` 后的内容对齐。相当于多行模式的 `align-items`，可用值排除`baseline`


```
/*ul represents any flex container*/
ul {
    align-content: flex-start || flex-end || center || stretch
}
/* default stretch */
```

#### 6.1 Stretch
元素按照最高的元素高度在 `cross-axis` 对齐
![](http://ojmbu5nqv.bkt.clouddn.com/flex-align-content-stretch.png)

#### 6.2 Flex-start
顶端对齐
![](http://ojmbu5nqv.bkt.clouddn.com/flex-align-content-flex-start.png)

#### 6.3 Flex-end
底端对齐
![](http://ojmbu5nqv.bkt.clouddn.com/flex-align-content-flex-end.png)

#### 6.4 Center
居中对齐
![](http://ojmbu5nqv.bkt.clouddn.com/flex-align-content-center.png)


# Flex-item 成员
- order
- flex-grow
- flex-shrink
- flex
- flex-basis

## 1. Order
`order` 顾名思义就是给元素排序，一般地，如果我们的列表是这样的


```
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>                          
</ul>
```
![Default Order](http://ojmbu5nqv.bkt.clouddn.com/flex-order-defualt.png)

flexbox 默认的 `order` 为 `0` ，按递增排序。

当我们把第一个元素的 `order` 设置为 `1`。

```
/*select first li element within the ul */
    li:nth-child(1) {
        order: 1; /*give it a value higher than 0*/
    }
```

![set first child order to 1](http://ojmbu5nqv.bkt.clouddn.com/flexbox-order-1.png)
由于2，3，4的 `order` 都是 `0`， 所以第一个元素就被排列到了最后。

## 2.Flex-grow and Flex-shrink
在使用 `flexbox` 时，最妙的就是利用他的弹性(flexible)，而 `flex-grow` 和 `flex-shrink` 正是控制元素弹性的成员。

- `flex-grow` 控制元素是否展开占据所有空隙
- `flex-shirnk` 控制元素是否在没有更多空间时，进行缩减。

### 可用值
 0 或 任意正整数 `0 || positive number`

### 默认值
`flex-grow` 默认为`0`，不进行展开    
`flex-shrink` 默认为`1`，自动缩减

来看一下例子
![Simple flex-item](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-grow-0.png)
由于 `flex-grow` 默认为 `0`， 元素就不会进行展开，占用整个一行

![flex-item fill the available space](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-grow-1.png)
在 `flex-grow` 设置为 `1`后， 元素就展开占据了整个一行

至于， `flex-shrink` 默认为 `1`，在文章开头例子中，添加10个元素时就已经观察过。

### 任意整数

现在我们学会开关元素弹性了，现在来看一下为什么可用值还可以是其他的正整数。
例如，我们现在有两个元素

```
<ul>
    <li>I am one</li>
    <li>I am two</li>                         
</ul>
```
设置他们的 `flex-grow` 分别为 `2` 和 `1`

```
li:nth-child(1) {
    flex-grow: 2;
}

li:nth-child(2) {
    flex-grow: 1;
}
```
![flex-grow ratio](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-grow-postive-number.png)
显然，我们可以通过增加 `flex-grow` 的值，来增加元素占行比

由此可见，    
各元素的占行比 = 各元素的 `flex-grow` 值 / 所有元素的 `flex-grow` 总和

## 3. Flex-basis
领略过了 `flexbox` 的弹性性能后，我们来看看 `flex-basis`。 `flex-basis` 指定了初始尺寸，即在被 `flex-grow` `flex-shrink` 改变之前的元素大小

```
li {
    flex-basis: auto || percentages || ems || rems || pixels
    /* default auto */
}
```

注意，`flex-basis: 0;` 和 `flex-basis: 0px;` 是有区别的，前者相当于不控制元素尺寸，后者是精确的设置为 `0px`

来看一个简单的例子：

```
<ul>
    <li>I am a simple list AND I am a simple list</li>
</ul>
```
![flex-basis auto](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-basis-default.png)
这是在 `flex-basis` 默认为 `auto` 时的样子。

当我们设置为某个固定尺寸时

```
li {
    flex-basis: 150px;
}
```
![flex-basis with a constrained width](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-basis-150px.png)

### 4. Flex
`flex` 是 `flex-grow` + `flex-shrink` + `flex-basis` 的简写形式，就如同于 `flex-flow` `border` 之类的成员。

![](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-shorthand.png)

#### 4.1 flex: 0 1 auto - default 

```
/*again, the "li" represents any flex-item*/
li {
  flex: 0 1 auto;
  /* same as flex: default */
}
```
![flex: 0 1 auto](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-default.png)
此时，元素不会展开，会缩减，根据自身内容调整大小

#### 4.2 flex: 0 0 auto - none


```
/*again, the "li" represents any list-item*/
li {
  flex: 0 0 auto;
  /* same as flex: none */
}
```

![flex: 0 0 auto](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-none.png)
此时，元素不会展开，不会缩减，而且根据自身内容调整大小

![NOT shrink when resizing browser](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-none-resizing.png)

因为元素不会缩减，所以在缩小浏览器大小后也不进行缩减

#### 4.3 flex: 1 1 auto 
```
/*again, the "li" represents any list-item*/
li {
  flex: 1 1 auto;
  /* same as flex: auto */
}
```
![flex: 1 1 auto](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-auto.png)
元素自动展开，自动缩减，根据内容调整大小

### 5. Align-self
区别于 `align-items` 设置在 `flex-container` 元素上，控制所有子元素的对齐方式， `align-self` 则是设置在 单个元素 上，只控制单个元素的对齐方式


```
/*target first list item*/
li:first-of-type {
    align-self: auto || flex-start || flex-end || center || baseline || stretch
}
```
这些可用值应该已经很熟悉了吧

#### 5.1 Flex-end
单个元素在 `cross-axis` 底部对齐
![targeted flex item at the end of the cross axis](http://ojmbu5nqv.bkt.clouddn.com/flex-align-self-flex-end.png)

#### 5.2 Center
单个元素在 `cross-axis` 居中对齐
![targeted flex item at the center of the cross axis](http://ojmbu5nqv.bkt.clouddn.com/flex-align-self-center.png)

#### 5.3 Stretch
单个元素填充整个 `cross-axis`
![targeted flex item stretched along the cross axis](http://ojmbu5nqv.bkt.clouddn.com/flex-align-self-stretch.png)

#### 5.4 Baseline
单个元素 基准线对齐
![targeted flex item aligned along the baseline](http://ojmbu5nqv.bkt.clouddn.com/flex-align-self-baseline.png)

#### 5.5 auto
`auto` 设置当前元素的 `align-self` 继承自父元素的 `align-items` 值，如果这个元素没有父元素，就设置为 `stretch`


```
ul {
    display: flex;
    border: 1px solid red;
    padding: 0;
    list-style: none;
    justify-content: space-between;
    align-items: flex-start; /*affects all flex-items*/
    min-height: 50%;
    background-color: #e8e8e9;
}
li {
  width: 100px;
  background-color: #8cacea;
  margin: 8px;
  font-size: 2rem;
}
```
这个例子中，父元素 `align-items: flex-start`。所以设置子元素 `align-self: auto` 相当于 `align-self: flex-start`

![targeted flex item aligned along the start of the cross axis](http://ojmbu5nqv.bkt.clouddn.com/flex-align-self-auto.png)

# 绝对弹性元素 和 相对弹性元素

先来看一个例子，直观感受一下。

```
<ul>
    <li>
        This is just some random text  to buttress the point being explained.
    Some more random text to buttress the point being explained.
    </li>
    <li>This is just a shorter random text.</li>
</ul>
```

设置为相对弹性元素

```
ul {
    display: flex; /*flexbox activated*/
}
li {
    flex: auto; /*remember this is same as flex: 1 1 auto;*/
    border: 2px solid red;
    margin: 2em;
}
```
![relative flex-item](http://ojmbu5nqv.bkt.clouddn.com/flex-relative.png)

设置为绝对弹性元素


```
li {
    flex: 1 ; /*same as flex: 1 1 0*/
}
```

![absolute flex-item](http://ojmbu5nqv.bkt.clouddn.com/flex-absolute.png)


Q: 在 `flex-box` 中， 怎么区分绝对和相对呢？    
A:    

- 相对，元素会根据自己的内容来控制尺寸。  
- 绝对，元素只会根据`flex`的设置排列对齐，无关自身的内容大小

Q: 怎么得到绝对/相对弹性元素呢？    
A：

- 相对：`flex-basis: auto`
- 绝对：`flex-basis: 0`

    
    
# Auto-margin Alignment
由于 `margin` 暂时找不出合适的中文，就直接使用英文名。 
   
当对 `flex-items` 设置 `margin: auto` 时，可能会发生一些奇怪的现象。由于 `margin: auto` 会占用所有为使用的空隙。我们可以借用这个特性来完成一些特殊的排列。


```
<ul>
    <li>Branding</li>
    <li>Home</li>
    <li>Services</li>
    <li>About</li>
    <li>Contact</li>
</ul>
ul {
    display: flex;
}
li {
    flex: 0 0 auto;
}
```

![Simple navigation bar](http://ojmbu5nqv.bkt.clouddn.com/flex-simple-navbar.png)

这是一个简单的弹性盒子，元素不会展开，不会缩减，根据自身内容调整大小。

但是可以看到，行尾还留有许多空隙
![extra spaces](http://ojmbu5nqv.bkt.clouddn.com/flex-automargin-extra.png)

现在，我们对第一个元素设置 `margin: auto`

```
li:nth-child(1) {
    margin-right: auto; /*applied only to the right*/
}
```
![margin: auto applied to branding](http://ojmbu5nqv.bkt.clouddn.com/flex-automargin-branding.png)

这之间发生了什么呢？

![](http://ojmbu5nqv.bkt.clouddn.com/flex-automargin-before-after.png)

那如果想要左右都能进行 auto-margin 呢

```
/*you may use the margin shorthand to set both sides if you wish*/
li:nth-child(1) {
    margin-left: auto;
    margin-right: auto
}
```

![margin:auto applied on both sides of the “branding”](http://ojmbu5nqv.bkt.clouddn.com/flex-automargin-bothside.png)

以上，我们就通过了设置 `margin` 来实现特殊的布局。需要注意的是，一旦设置了元素的 `margin` ，`justify-content`成员就会失效


```
ul {
    justify-content: flex-end;
}
```
即使我们这样添加 `justify-content` 也不会影响 `margin` 设置的布局

# 实用案例
下面的3个案例，可以用来试试手，实现看看。

## (i) Bootstrap Navigation
![](http://ojmbu5nqv.bkt.clouddn.com/flex-bootstrap-nav.png)

## (ii) AirBnB desktop Navigation
![](http://ojmbu5nqv.bkt.clouddn.com/flex-airbnb-nav.png)

## (iii) Twitter desktop Navigation
![](http://ojmbu5nqv.bkt.clouddn.com/flex-twitter.png)

## 解析 flex-direction
在 `flex-direciton` 的章节中，我们提到过 `main-axis` 和 `cross-axis` 这两个坐标轴概念。 为什么我们要用坐标轴概念，而摒弃水平/垂直排列呢？答案就在 `flex-direction` 上。

当 `flex-direction: row` 的时候，我们都知道从左向右排列，正确地说应该称之为 默认的 `main-axis` 和 `cross-axis` 位置。
![default main and cross axis](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-direction-row.png) 

当我们 `flex-direction: column` 时，到底发生了什么？

正确的说法是，更改了坐标轴的位置

![changed main and cross axis](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-direction-column.png)

在正确理解坐标轴概念之后，我们就可以知道 `flex-direction: column`时，排列和对齐的方向就随着 `main-axis` `cross-axis` 的改变而改变了

来看个例子

```
<ul>
   <li></li>
   <li></li>
   <li></li>
</ul>
ul {
    display: flex;
}
```

![](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-direction-demo-row.png)

```
ul {
    display: flex;
    flex-direction: column;
}
```

![](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-direction-column-demo.png)


```
li {
    flex-basis: 100px;
}
```

![](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-direciton-column-demo2.png)

原本在 `row` 时控制宽度， 现在变成了控制高度。

在看看普通的 `width` 成员

```
li {
    width: 200px;
}
```

![](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-direction-column-width.png)

这样，你渐渐明白了 `main-axis` 的作用， `flex-basis` 是控制在 `main-axis` 上的大小的

再试试看，根据 `cross-axis` 对齐的 `algin-items` 成员

```
li {
    align-items: center;
}
```

![](http://ojmbu5nqv.bkt.clouddn.com/flex-flex-direction-column-align-items.png)

很清楚的看到，`corss-axis` 现在表示水平位置，所以 `align-items` 使得元素在水平位置居中。



# 参考链接 Resource

[Understanding Flex: Everything you need to know](https://medium.freecodecamp.com/understanding-flexbox-everything-you-need-to-know-b4013d4dc9af#.27z6tmmel)

[flexbox 解决方案](https://hufan-akari.github.io/solved-by-flexbox/)

[Flexbugs and gotchas for non-compliant browsers](https://github.com/philipwalton/flexbugs)

# 结语
使用 flexbox 模型，可以用更干净的css，更直接的解决方案来完成页面布局、排列

