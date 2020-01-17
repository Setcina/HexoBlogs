---
title: '01-display:flex'
date: 2020-01-17 11:10:31
tags:
  CSS
---

2009年，W3C 提出了一种新的方案----Flex 布局，可以简便、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，这意味着，现在就能很安全地使用这项功能。

Chrome | Opera | Firefox | safari | IE | 
---|---|---|---|---|
12+ |  12.1+ | 22+ | 6.1+ | 10+ |


### Flex布局是什么
Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。任何一个容器都可以指定为 Flex 布局。
``` css
.box{
  display: flex;
  display: inline-flex;
}
```

### 基本概念
采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。

项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。


### 容器的属性
以下6个属性设置在容器上。
``` js
* flex-direction
* flex-wrap
* flex-flow
* justify-content
* align-items
* align-content
```

#### flex-direction属性
flex-direction属性决定主轴的方向（即项目的排列方向）。
``` js
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}

row: 主轴为水平方向，起点在左端。
row-reverse：主轴为水平方向，起点在右端。
column：主轴为垂直方向，起点在上沿。
column-reverse：主轴为垂直方向，起点在下沿。
```


#### flex-wrap属性
默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。
``` js
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
nowrap（默认）：不换行。
wrap：换行，第一行在上方。
wrap-reverse：换行，第一行在下方。
```

#### flex-flow
flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}

#### justify-content属性
justify-content属性定义了项目在主轴上的对齐方式。

``` css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}

flex-start（默认值）：左对齐
flex-end：右对齐
center： 居中
space-between：两端对齐，项目之间的间隔都相等。
space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
```

#### align-items属性
align-items属性定义项目在交叉轴上如何对齐。
``` css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
flex-start：交叉轴的起点对齐。
flex-end：交叉轴的终点对齐。
center：交叉轴的中点对齐。
baseline: 项目的第一行文字的基线对齐。
stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
```

#### align-content属性
align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
``` css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}

flex-start：与交叉轴的起点对齐。
flex-end：与交叉轴的终点对齐。
center：与交叉轴的中点对齐。
space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
stretch（默认值）：轴线占满整个交叉轴。
```