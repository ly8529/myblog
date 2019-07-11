---
title: 学习 css grid 布局
---
参考博客： 阮一峰 http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html
# 网格布局

flex布局相当于一维布局，grid相当于二维布局

# 容器和项目
    容器指采用网格布局的区域，可以理解为父元素，项目指父元素内的顶层子元素

# 容器的属性
#### 1、display属性，可设置的值为grid，inline-grid

    注意，设为网格布局以后，容器子元素（项目）的float、display: inline-block、display: table-cell、vertical-align和column-*等设置都将失效。
#### 2、grid-template-columns 属性，grid-template-rows 属性
    grid-template-columns 属性定义每一列的列宽，
    grid-template-rows 属性定义每一行的行高
```
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
```
或者是用百分比
```
.container {
  display: grid;
  grid-template-columns: 33.33% 33.33% 33.33%;
  grid-template-rows: 33.33% 33.33% 33.33%;
}
```
##### repeat()
1、repeat函数可以简化重复的值
```
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
```
repeat()接受两个参数，第一个参数是重复的次数（上例是3），第二个参数是所要重复的值。
repeat()重复某种模式也是可以的。

```

grid-template-columns: repeat(2, 100px 20px 80px);
```
2、auto-fill自动填充

当容器大小不确定，单元格大小是固定时使用，每一行会自动容纳尽可能多的单元格。

```
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}

```
上述代码表示每列宽度100px，然后自动填充，直到容器不能放置更多的列

3、fr关键字（fraction片段）
```
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```
上述代码表示两个相同宽度的列

```
.container {
  display: grid;
  grid-template-columns: 150px 1fr 2fr;
}
```

上述代码表示 第一列宽度为150px ，第二列宽度是第三列宽度的一半。

4、minmax函数

minmax函数接收两个参数，分别为最小值和最大值

```
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```
上述代码minmax(100px, 1fr)表示列宽不小于 100px，不大于1fr

5、auto关键字

auto 关键字表示由浏览器自己决定长度
```
grid-template-columns: 100px auto 100px;
```
上述代码中，第二列的宽度，基本上等于该列单元格的最大宽度，除非单元格内容设置了min-width，且这个值大于最大宽度。

6、网线格的名称

grid-template-columns属性和grid-template-rows属性里面，还可以使用方括号，指定每一根网格线的名字，方便以后的引用
```
.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```
上述代码指定网格布局为3行 x 3列，因此有4根垂直网格线和4根水平网格线。方括号里面依次是这八根线的名字。
网格布局允许同一根线有多个名字，比如[fifth-line row-5]。

7、布局实例
grid-template-columns属性对于网页布局非常有用。两栏式布局只需要一行代码。

```
.wrapper {
  display: grid;
  grid-template-columns: 70% 30%;
}
```
上述代码将左边栏设为70%，右边栏设为30%。
```
grid-template-columns: repeat(12, 1fr);
```
十二网格布局

#### 3、grid-row-rap,grid-column,grid-gap 属性
grid-row属性设置行与行的间隔（行间距），grid-column-gap属性设置列与列的间隔（列间距）
```
.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}
```
上述代码中，grid-row-gap用于设置行间距，grid-column-gap用于设置列间距

grid-gap属性是grid-column-gap和grid-row-gap的合并简写形式，语法如下。
```
grid-gap: <grid-row-gap> <grid-column-gap>;
```
上述代码等同于
```
.container {
  grid-gap: 20px 20px;
}
```
如果grid-gap省略了第二个值，浏览器认为第二个值等于第一个值。

根据最新标准，上面三个属性名的grid-前缀已经删除，grid-column-gap和grid-row-gap写成column-gap和row-gap，grid-gap写成gap。
#### 4、grid-template-areas属性
网格布局允许制定”区域“，一个区域由单个或者多个单元格组成，该属性用于定义区域。
```
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}
```
上述代码先划分出9个单元格，然后将其定名为a到i的九个区域，分别对应这九个单元格。

多个单元格合并成一个区域的写法
```
grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';
```
上面代码将9个单元格分成a、b、c三个区域。

下面是一个布局实例,顶部是页眉区域header，底部是页脚区域footer，中间部分则为main和sidebar。。
```
grid-template-areas: "header header header"
                     "main main sidebar"
                     "footer footer footer";
```

如果某些区域不需要利用，则使用"点"（.）表示,表示没有用到该单元格，该单元格不属于任何区域。
```

grid-template-areas: 'a . c'
                     'd . f'
                     'g . i';

```
注意，区域的命名会影响到网格线。每个区域的起始网格线，会自动命名为区域名-start，终止网格线自动命名为区域名-end。
比如，区域名为header，则起始位置的水平网格线和垂直网格线叫做header-start，终止位置的水平网格线和垂直网格线叫做header-end。

#### 5、grid-auto-flow 属性，设置元素的排列顺序
默认的放置顺序是“先行后列”

```
grid-auto-flow: row;  //先行后列
grid-auto-flow: column;   //先列后行
```
如下表示"先行后列"，并且尽可能紧密填满，尽量不出现空格。

grid-auto-flow: row dense;

#### 6、justify-items 属性，align-items 属性，place-items 属性
justify-items 属性设置单元格的水平位置 (左中右)，

align-items 属性设置单元格内容的垂直位置。
```
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```
* start：对齐单元格的起始边缘。
* end：对齐单元格的结束边缘。
* center：单元格内部居中。
* stretch：拉伸，占满单元格的整个宽度（默认值）。

place-items属性是align-items属性和justify-items属性的合并简写形式,如果省略第二个值，则浏览器认为与第一个值相等。。

```
place-items: <align-items> <justify-items>;
place-items: start end
```
#### 7、justity-content 属性，align-content 属性，place-content 属性
justify-content 属性是整个内容区域在容器里边的水平位置，

align-content 属性是整个内容区域的垂直位置。
```
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```
* start - 对齐容器的起始边框。
* end - 对齐容器的结束边框。
* center - 容器内部居中。
* stretch - 项目大小没有指定时，拉伸占据整个网格容器。
* space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。
* space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔。
* space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。
* place-content属性是align-content属性和justify-content属性的合并简写形式,如果省略第二个值，浏览器就会假定第二个值等于第一个值.
```
place-content: <align-content> <justify-content>
place-content: space-around space-evenly;
```
#### 8、grid-auto-columns 属性 grid-auto-rows属性
有时候，一些项目的指定位置，在现有网格的外部。比如网格只有3列，但是某一个项目指定在第5行。这时，浏览器会自动生成多余的网格，以便放置项目。

grid-auto-columns属性和grid-auto-rows属性用来设置，浏览器自动创建的多余网格的列宽和行高。它们的写法与grid-template-columns和grid-template-rows完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。
```
#container{
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-auto-rows: 50px; 
}
.item-8 {
  background-color: #d0e4a9;
  grid-row-start: 4;
  grid-column-start: 2;
}
.item-9 {
  background-color: #4dc7ec;
  grid-row-start: 5;
  grid-column-start: 3;
}
```
#### 9、grid-template 属性，grid 属性，了解即可，语义化考虑，不建议合并
grid-template属性是grid-template-columns、grid-template-rows和grid-template-areas这三个属性的合并简写形式。

grid属性是grid-template-rows、grid-template-columns、grid-template-areas、 grid-auto-rows、grid-auto-columns、grid-auto-flow这六个属性的合并简写形式。

# 项目元素的属性
#### 1、grid-column-start 属性，grid-column-end 属性，grid-row-start 属性 grid-row-end属性

* grid-column-start属性：左边框所在的垂直网格线
* grid-column-end属性：右边框所在的垂直网格线
* grid-row-start属性：上边框所在的水平网格线
* grid-row-end属性：下边框所在的水平网格线
```
.item-1 {
  grid-column-start: 2;
  grid-column-end: 4;
}
```
上面代码指定，1号项目的左边框是第二根垂直网格线，右边框是第四根垂直网格线。

下面的例子是指定四个边框位置的效果

```
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 2;
  grid-row-end: 4;
}
```
这四个属性的值，除了指定为第几个网格线，还可以指定为网格线的名字。
```
.item-1 {
  grid-column-start: header-start;
  grid-column-end: header-end;
}
```
上面代码中，左边框和右边框的位置，都指定为网格线的名字。

这四个属性的值还可以使用span关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。
```
.item-1 {
  grid-column-start: span 2;
}
.item-1 {
  grid-column-end: span 2;
}
```
上面代码表示，1号项目的左边框距离右边框跨越2个网格。

使用这四个属性，如果产生了项目的重叠，则使用z-index属性指定项目的重叠顺序。

#### 2、grid-column属性，grid-row属性

grid-column属性是grid-column-start和grid-column-end的合并简写形式，

grid-row属性是grid-row-start属性和grid-row-end的合并简写形式。
语法如下
```
.item {
  grid-column:  / ;
  grid-row:  / ;
}
```
demo
```
.item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 2;
}
/* 等同于 */
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 2;
}

```
上面代码中，项目item-1占据第一行，从第一根列线到第三根列线。

这两个属性之中，也可以使用span关键字，表示跨越多少个网格。

```
.item-1 {
  background: #b03532;
  grid-column: 1 / 3;
  grid-row: 1 / 3;
}
/* 等同于 */
.item-1 {
  background: #b03532;
  grid-column: 1 / span 2;
  grid-row: 1 / span 2;
}
```
上面代码中，项目item-1占据的区域，包括第一行 + 第二行、第一列 + 第二列。

斜杠以及后面的部分可以省略，默认跨越一个网格。
```
.item-1 {
  grid-column: 1;
  grid-row: 1;
}
```
上面代码中，项目item-1占据左上角第一个网格。

#### 3、grid-area 属性
grid-area属性指定项目放在哪一个区域。
```
.item-1 {
  grid-area: e;
}
```
上面代码中，1号项目位于e区域。

grid-area属性还可用作grid-row-start、grid-column-start、grid-row-end、grid-column-end的合并简写形式，直接指定项目的位置。
```
.item {
  grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
}
.item-1 {
  grid-area: 1 / 1 / 3 / 3;
}
```
#### 4、justify-self属性 align-self 属性 place-self属性
justify-self属性设置单元格内容的水平位置（左中右），跟justify-items属性的用法完全一致，但只作用于单个项目。

align-self属性设置单元格内容的垂直位置（上中下），跟align-items属性的用法完全一致，也是只作用于单个项目。
```
.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```
* start：对齐单元格的起始边缘。
* end：对齐单元格的结束边缘。
* center：单元格内部居中。
* stretch：拉伸，占满单元格的整个宽度（默认值）。
```
.item-1  {
  justify-self: start;
}
```
place-self属性是align-self属性和justify-self属性的合并简写形式。
```
place-self: <align-self> <justify-self>;
place-self: center center;
```
https://blog.theodo.com/2018/03/stop-using-bootstrap-layout-thanks-to-css-grid/

https://learncssgrid.com/

https://code.tutsplus.com/tutorials/introduction-to-css-grid-layout-with-examples--cms-25392

https://webdesign.tutsplus.com/tutorials/how-to-build-an-off-canvas-navigation-with-css-grid--cms-28191

https://webdesign.tutsplus.com/series/understanding-the-css-grid-layout-module--cms-1079

https://css-tricks.com/snippets/css/complete-guide-grid/

https://css-tricks.com/snippets/css/complete-guide-grid/










