---
title: css grid 布局
---
参考博客：http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html
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
grid-row属性设置行与行的间隔（行间距），grid-column-gap属性设置列与列的间隔（列间距）
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


# 项目元素的属性

