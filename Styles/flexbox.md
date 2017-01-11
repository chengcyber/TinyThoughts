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

# Resource

[Understanding Flex: Everything you need to know](https://medium.freecodecamp.com/understanding-flexbox-everything-you-need-to-know-b4013d4dc9af#.27z6tmmel)


