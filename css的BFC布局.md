---
title: css BFC 模式
---
参考博客：https://www.cnblogs.com/chen-cong/p/7862832.html
# BFC模式 块级格式化上下文

创建了BFC元素就是一个独立的盒子，只有Block-level box 可以参加创建BFC，它规定了内部的Block-level box 如何布局，并且这个盒子的布局和外界互不影响。

# 形成BFC的条件
      1、浮动元素，float 除 none 以外的值； 
      2、定位元素，position（absolute，fixed）； 
      3、display 为以下其中之一的值 inline-block，table-cell，table-caption； 
      4、overflow 除了 visible 以外的值（hidden，auto，scroll）；

# BFC的特性

      1.内部的Box会在垂直方向上一个接一个的放置。
      2.垂直方向上的距离由margin决定。
      3.bfc的区域不会与float的元素区域重叠。
      4.计算bfc的高度时，浮动元素也参与计算。
      5.bfc就是页面上的一个独立容器，容器里面的子元素不会影响外面元素。

# BFC实例

#### BFC中的盒子对齐
特性的第一条是：内部的Box会在垂直方向上一个接一个的放置。
浮动的元素也是这样，box3浮动，他依然接着上一个盒子垂直排列。并且所有的盒子都左对齐。
```
    <div class="container">
        <div class="box1"></div>
        <div class="box2"></div>
        <div class="box3"></div>
        <div class="box4"></div>
    </div>
    css
    div {
            height: 20px;
        }
        
        .container {
            position: absolute;  /* 创建一个BFC环境*/
            height: auto;
            background-color: #eee;
        }
        
        .box1 {
            width: 400px;
            background-color: red;
        }
        
        .box2 {
            width: 300px;
            background-color: green;
        }
        
        .box3 {
            width: 100px;
            background-color: yellow;
            float: left;
        }
        
        .box4 {
            width: 200px;
            height: 30px;
            background-color: purple;
        }
```
#### 边距折叠
```
    <div class="container">
        <div class="box"></div>
        <div class="box"></div>
    </div>
    css代码
    .container {
            overflow: hidden;
            width: 100px;
            height: 100px;
            background-color: red;
        }
        
        .box1 {
            height: 20px;
            margin: 10px 0;
            background-color: green;
        }
        
        .box2 {
            height: 20px;
            margin: 20px 0;
            background-color: green;
        }

```
#### 不被浮动元素覆盖 
```
    <div class="column"></div>
    <div class="column"></div>
    css代码
    .column:nth-of-type(1) {
        float: left;
        width: 200px;
        height: 300px;
        margin-right: 10px;
        background-color: red;
    }
    
    .column:nth-of-type(2) {
        overflow: hidden;/*创建bfc */
        height: 300px;
        background-color: purple;
    }

```
#### 还有三栏布局
```
   <div class="contain">
        <div class="column"></div>
        <div class="column"></div>
        <div class="column"></div>
    </div>
    .column:nth-of-type(1),
        .column:nth-of-type(2) {
            float: left;
            width: 100px;
            height: 300px;
            background-color: green;
        }
        
        .column:nth-of-type(2) {
            float: right;
        }
        
        .column:nth-of-type(3) {
            overflow: hidden;  /*创建bfc*/
            height: 300px;
            background-color: red;
        }
```
#### 防止字体环绕
```
    <div class="left"></div>
    <p>你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好
       你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好
       你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好你好
    </p>
    .left {
        float: left;
        width: 100px;
        height: 100px;
        background-color: yellow;
    }
    
    p {
        background-color: green;
        overflow: hidden;
    }
```
## 扩展
*实现三格布局的五种方式，各优缺点，去掉高度考虑纵向的时候，还有那种方式适应，兼容性如何
1、float布局
2、绝对定位布局
3、flex布局
4、table布局
5、网格布局
## 扩展2
flex布局与网格布局的区别