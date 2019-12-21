---
title: css3使用vw和vh视口单位实现自适应
date: 2019-01-17 07:13:15
tags:
    - 移动Web
---
### 前言

移动端网页和pc端很大不同的便是现在移动端窗口分辨率繁多，所以，移动端适配，一直是前端工作上的一个痛点。但既然是痛点，那就要解决它，为此我们先来了解一写基本的概念之后，我们再进入这次移动端自适应的主题。

#### 什么是rem？

来自于鹅厂ISUX团队的解释如下： rem（font size of the root element）是指相对于根元素的字体大小的单位。简单的说它就是一个相对单位。看到rem大家一定会想起em单位，em（font size of the element）是指相对于父元素的字体大小的单位。它们之间其实很相似，只不过一个计算的规则是依赖根元素一个是依赖父元素计算。

所以这里总结一句，所谓依赖根元素来计算的方式，就是先给予html元素一个font-size，然后我们所有的rem就根据这个font-size来计算

``` html

html{ font-size:16px;}

```

那么我们这里的1rem就应该这么来计算：1x16=16px=1rem；

#### 什么是视口

在业界，极为推崇的一种理论是 Peter-Paul Koch 提出的关于视口的解释——在桌面端，视口指的是在桌面端，指的是浏览器的可视区域；而在移动端较为复杂，它涉及到三个视口：分别是 Layout Viewport（布局视口）、 Visual Viewport（视觉视口）、Ideal Viewport。

而视口单位中的“视口”，
* > 在桌面端，毫无疑问指的就是浏览器的可视区域；
* > 移动端，它指的则是三个 Viewport 中的 Layout Viewport 。

根据CSS3规范，视口单位主要包括以下4个：

* vw : 1vw 等于视口宽度的1%

* vh : 1vh 等于视口高度的1%

* vmin : 选取 vw 和 vh 中最小的那个

* vmax : 选取 vw 和 vh 中最大的那个

视口单位区别于%单位，视口单位是依赖于视口的尺寸，根据视口尺寸的百分比来定义的；而%单位则是依赖于元素的祖先元素。

好了，我们的基本概念就到此结束，进入我们的正题

响应式布局的实现依靠媒体查询（ Media Queries ）来实现，选取主流设备宽度尺寸作为断点针对性写额外的样式进行适配，但这样做会比较麻烦，只能在选取的几个主流设备尺寸下呈现完美适配。

即使是通过 rem 单位来实现适配，也是需要内嵌一段脚本去动态计算根元素大小。（最出名的是手淘的flexible.js移动端自适应方案）

而近年来，随着移动端对视口单位的支持越来越成熟、广泛，使得我们可以尝试css3使用vw和vh视口单位去真正地适配所有设备尺寸。


#### 主题一：flexible.js

早期移动端开发，对于终端设备适配问题只属于Android系列，只不过很多设计师常常忽略Android适配问题，只出一套iOS平台设计稿。但随着iPhone6，iPhone6+的出现，从此终端适配问题不再是Android系列了，也从这个时候让移动端适配全面进入到“杂屏”时代。

手淘经过多年的摸索和实战，总结了一套移动端适配的方案——flexible方案。（只做简单介绍，下面有文档地址）

``` js

npm i -save amfe-flexible //源码

```

``` html
<!DOCTYPE html> 
<html lang="en"> 
    <head> 
        <meta charset="utf-8"> 
        <meta content="yes" name="apple-mobile-web-app-capable"> 
        <meta content="yes" name="apple-touch-fullscreen"> 
        <meta content="telephone=no,email=no" name="format-detection"> 
        <script src="http://g.tbcdn.cn/mtb/lib-flexible/0.3.4/??flexible_css.js,flexible.js"></script> //cnd
        <link rel="apple-touch-icon" href="favicon.png"> 
        <link rel="Shortcut Icon" href="favicon.png" type="image/x-icon"> 
        <title>flexible.js</title>
    </head>
<body>

```

flexible.js是利用了rem单位相对根元素html的font-size来做计算，而我们需要做的就是根据不同的屏幕算出html的font-size，而页面内的大小单位都根据rem来写，从而实现了自适应。

虽然用法简单，在这里我对flexible.js就不说太多了，因为这个方案已经被vw以及vh单位替代。其实，这个方案缺点在于，每一次页面都要先加载一段js去算视口大小以及drp值，在加载页面的时候，会比较明显的感觉到页面大小一瞬间的变化，所以这本身不是最好的适配方案，在此我给出关于flexible如何配合样式使用的github官方文档地址，大家有兴趣可以去看看

https://github.com/amfe/article/issues/17


#### 主题二：vw和vh搭配rem实现自适应

上面说到第一个方案flexible.js的弊端，所以，在vw和vh越来越兼容目前市面上的移动端设备的时候（ios 8 以上以及 Android 4.4 以上获得支持，并且在微信 x5 内核中也得到完美的全面支持），我们目前可以通过视口单位实现适配的页面，是既能解决响应式断层问题，又能解决脚本依赖的问题的。

实现方案：
* >给根元素大小设置随着视口变化而变化的 vw 单位，这样就可以实现动态改变其大小。
* >限制根元素字体大小的最大最小值，配合 body 加上最大宽度和最小宽度

1.首先我们创建一个index.html
``` html
<!DOCTYPE HTML>
<html>

    <head>
    <meta charset="utf-8" />
    <meta content="yes" name="apple-mobile-web-app-capable" />
    <meta content="yes" name="apple-touch-fullscreen" />
    <meta content="telephone=no,email=no" name="format-detection" />
    <!-- format-detection格式检测 -->
    <!-- telephone=no禁止了把数字转化为拨号链接 -->
    <!-- email=no禁止作为邮箱地址 -->
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <!-- width = device-width：宽度等于当前设备的宽度 -->
    <!-- initial-scale：初始的缩放比例（默认设置为1.0）-->
    <!-- maximum-scale：允许用户缩放到的最大比例（默认设置为1.0）-->
    <title>css3利用vw和rem实现移动端自适应</title>
    </head>

    <body>
        <div class="nav">
            <nav>
                <a href="#">1</a>
                <a href="#">2</a>
                <a href="#">3</a>
                <a href="#">4</a>
                <a href="#">5</a>
            </nav>
        </div>
    </body>

</html> 
```

2.然后我们创建一个scss文件(scss文件的编译我就不多说了，因为这个应该大部分前端工程师都是会的，如果不了解sass，可以移步到sass官网了解一下：https://www.sass.hk/)
``` css
/* rem 单位换算：定为 75px 只是方便运算，750px-75px、640-64px、1080px-108px，如此类推 */
/* iPhone 6尺寸的根元素大小基准值 */
$vm_fontsize: 75; 

@function rem($px) {
     @return ($px / $vm_fontsize ) * 0.5rem;
}
/* 根元素大小使用 vw 单位 */
$vm_design: 750;
html {
    font-size: ($vm_fontsize / ($vm_design / 2)) * 100vw; 
    /* 同时，通过Media Queries 限制根元素最大最小值 */
    @media screen and (max-width: 320px) {
        font-size: 64px;    
    }
    @media screen and (min-width: 750px) {
        font-size: 150px;
    }
}
/*  body 也增加最大最小宽度限制，避免默认100%宽度的 block 元素跟随 body 而过大过小 */
body {
    max-width: 750px;
    min-width: 320px;
    margin: 0 auto;
}

.nav{ 
  width: 100%;
  height: 100%;
  background: #ccc;
  nav{  
    padding: rem(20) 0;
    width: 100%;
    height: rem(120);
    background: #d5fafc;
    a{  
      display: block; 
      float: left;
      width: rem(120);
      height: rem(120);
      background: red;
      margin: 0 rem(15);
      font-size: rem(20);
      line-height: rem(120);
      text-align: center;
    }
  }
}
```

采用vw来做适配处理并不是只有好处没有任何缺点。有一些细节之处还是存在一定的缺陷的。比如当容器使用vw单位，margin采用px单位时，很容易造成整体宽度超过100vw，从而影响布局效果。对于类似这样的现象，我们可以采用相关的技术进行规避。比如将margin换成padding，并且配合box-sizing。只不过这不是最佳方案，随着将来浏览器或者应用自身的Webview对calc()函数的支持之后，碰到vw和px混合使用的时候，可以结合calc()函数一起使用，这样就可以完美的解决。

另外一点，px转换成vw单位，多少还会存在一定的像素差，毕竟很多时候无法完全整除。

到目前为止，我发现的两个不足之处。或许在后面的使用当中，还会碰到一些其他不为人之的坑。事实也是如此，不管任何方案，踩得坑越多，该方案也越来越强大。希望喜欢这个适配方案的同学和我一起踩坑，让其更为完善。
https://www.w3cplus.com/css/vw-for-layout.html