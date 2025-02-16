# Grid布局

Flex 布局是轴线布局，只能指定"项目"针对轴线的位置，可以看作是**一维布局**。Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是**二维布局**。Grid 布局远比 Flex 布局强大。

## 基本概念

采用网格布局的区域，称为容器（container），容器内部采用网格定位的子元素，称为项目（item）。

> ```markup
> <div>
>   <div><p>1</p></div>
>   <div><p>2</p></div>
>   <div><p>3</p></div>
> </div>
> ```

上面代码中，最外层的`<div>`元素就是容器，内层的三个`<div>`元素就是项目。

注意：项目只能是容器的顶层子元素，不包含项目的子元素，比如上面代码的`<p>`元素就不是项目。**Grid 布局只对项目生效**。

`display: grid`指定一个容器采用网格布局。

默认情况下，容器元素都是块级元素，但也可以设为行内元素：`display: inline-grid;`

>注意，设为网格布局以后，容器子元素（项目）的`float`、`display: inline-block`、`display: table-cell`、`vertical-align`和`column-*`等设置都将失效。

## 容器属性

### 1. grid-template-rows和grid-template-columns

`grid-template-rows`属性定义每一行的行高，设定为多少行就设置多少个值，取值可以为固定像素，也可以为百分比，`grid-template-columns`属性定义每一列的列宽，设定为多少列就设置多少个值，取值可以为固定像素，也可以为百分比。

```css
.container {
    display: grid;
    grid-template-rows: 30px 30px 30px;
    grid-template-columns: 30px 30px 30px;
}
```

#### repeat()

`repeat()`函数可以简化重复的值，接受两个参数，第一个参数是重复的次数，第二个参数是所要重复的值。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
```

也可以是重复的模式

```css
grid-template-columns: repeat(2, 100px 20px 80px);
<!-- 代表定义6列，重复2次100px 20px 80px规则 -->
<!-- 即第一列和第四列的宽度为100px，第二列和第五列为20px，第三列和第六列为80px -->
```

#### auto-fill

有时，单元格的大小是固定的，但是容器的大小不确定。如果希望每一行或每一列容纳尽可能多的单元格，这时可以使用`auto-fill`关键字表示自动填充，当容器不足容纳成员时会自适应换行。

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

每列宽度100px，直至容器不能放置更多列后换行。

#### fr

表示比例关系，网格布局提供了`fr`关键字。如果两列的宽度分别为1fr和2fr，就表示后者是前者的两倍。

```css
.container {
  display: grid;
  grid-template-columns: 1fr 2fr;
}
```

#### minmax()

`minmax()`函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。当距离不够时会从最大值自动减少长度或宽度到设定最小值为止。  

```css
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

上面代码中，`minmax(100px, 1fr)`表示列宽不小于`100px`，不大于`1fr`

#### auto

`auto`关键字表示由浏览器自己决定长度。

```css
grid-template-columns: 100px auto 100px;
```

上面代码中，第二列的宽度，基本上等于该列单元格的最大宽度，除非单元格内容设置了`min-width`，且这个值大于最大宽度。

### 2. grid-row-gap、grid-column-gap和grid-gap

`grid-row-gap`属性设置行与行的间隔，即行间距。

`grid-column-gap`属性设置列与列的间隔（列间距）。

```css
.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}
```

`grid-gap`属性是`grid-column-gap`和`grid-row-gap`的合并简写形式，语法如下。

```css
grid-gap: <grid-row-gap> <grid-column-gap>;
```

上面一段CSS代码等同于

```css
.container {
  grid-gap: 20px 20px;
}
```

如果`grid-gap`省略了第二个值，浏览器认为第二个值等于第一个值。

>根据最新标准，上面三个属性名的`grid-`前缀已经删除，`grid-column-gap`和`grid-row-gap`写成`column-gap`和`row-gap`，`grid-gap`写成`gap`。

### 3. grid-template-areas

网格布局允许指定区域`area`，一个区域由单个或多个单元格组成，`grid-template-areas`属性用于定义区域。区域的命名会影响到网格线。每个区域的起始网格线，会自动命名为`{areaName}-start`，终止网格线自动命名为`{areaName}-end`。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}
```

### 4. grid-auto-flow

划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是先行后列，通过设置`grid-auto-flow`可以更改为先列后行，`grid-auto-flow`属性除了设置成`row`和`column`，还可以设成`row dense`和`column dense`，这两个值主要用于，某些项目指定位置以后，剩下的项目怎么自动放置。

```css
grid-auto-flow: column dense;   表示"先列后行"，并且尽量填满空格
grid-auto-flow: row dense;      表示"先行后列"，并且尽量填满空格
```

```html
<div id="t3">
    <div>F</div>
    <div>G</div>
    <div>H</div>
    <div>I</div>
</div>
<!-- 
    F H
    G I
-->

<style type="text/css">
   #t3{
        display: grid;
        grid-template-columns: repeat(2,30px);
        grid-template-rows: repeat(2,30px);
        grid-auto-flow: column;
    }
</style>
```

### 5. justify-items、align-items和place-items

`justify-items`属性设置单元格内容的**水平位置**（左中右），`align-items`属性设置单元格内容的**垂直位置**（上中下）。

```css
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```

- start：对齐单元格的起始边缘。
- end：对齐单元格的结束边缘。
- center：单元格内部居中。
- stretch：拉伸，占满单元格的整个宽度（默认值）。

`place-items`属性是`align-items`属性和`justify-items`属性的合并简写形式。

```css
place-items: <align-items> <justify-items>;
```

如果省略第二个值，则浏览器认为与第一个值相等。

### 6. justify-content、align-content和place-content

`justify-content`属性是整个**内容区域**在容器里面的**水平位置**（左中右），`align-content`属性是整个内容区域的**垂直位置**（上中下）。

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```

- start - 对齐容器的起始边框。
- end - 对齐容器的结束边框。
- center - 容器内部居中。
- stretch - 项目大小没有指定时，拉伸占据整个网格容器。
- space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。
- space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔。
- space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。

`place-content`属性是`align-content`属性和`justify-content`属性的合并简写形式。

```css
place-content: <align-content> <justify-content>
```

如果省略第二个值，则浏览器认为与第一个值相等。

### 7. grid-auto-columns和grid-auto-rows

有时候，一些项目的指定位置，在现有网格的外部。比如网格只有3列，但是某一个项目指定在第5行。这时，浏览器会自动生成多余的网格，以便放置项目。

`grid-auto-columns`属性和`grid-auto-rows`属性用来设置，浏览器自动创建的多余网格的列宽和行高。它们的写法与`grid-template-columns`和`grid-template-rows`完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

## 项目属性

### 1. grid-column-start 属性， grid-column-end 属性， grid-row-start 属性， grid-row-end 属性

- `grid-column-start`属性：左边框所在的垂直网格线
- `grid-column-end`属性：右边框所在的垂直网格线
- `grid-row-start`属性：上边框所在的水平网格线
- `grid-row-end`属性：下边框所在的水平网格线

还可以给轴线命名来指定位置。

```html
<div class="gridBox" style="">
    <div style="grid-column-start: c2;grid-row-start: r2;">V</div>
</div>  
<!--指定c2列，r2行-->
<style type="text/css">
    .gridBox{
        display: grid;
        grid-template-columns:[c1] 30px [c2] 30px [c3]; /* 指定列并为轴线命名 */
        grid-template-rows:[r1] 30px [r2] 30px [r3]; /* 指定行并为轴线命名 */
    }
</style>
```

这四个属性的值还可以使用`span`关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。

```css
.item-1 {
  grid-column-start: span 2;
}
```

### 2. grid-column 属性， grid-row 属性

`grid-column`属性是`grid-column-start`和`grid-column-end`的合并简写形式，`grid-row`属性是`grid-row-start`属性和`grid-row-end`的合并简写形式。

```css
.item {
  grid-column: <start-line> / <end-line>;
  grid-row: <start-line> / <end-line>;
}
```

这两个属性之中，也可以使用`span`关键字，表示跨越多少个网格。例如

```css
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

斜杠以及后面的部分可以省略，默认跨越一个网格。

### 3. grid-area 属性

`grid-area`属性还可用作`grid-row-start`、`grid-column-start`、`grid-row-end`、`grid-column-end`的合并简写形式，直接指定项目的位置。

```css
.item {
  grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
}
```

### 4. justify-self 属性， align-self 属性， place-self 属性

`justify-self`属性设置单元格内容的水平位置，跟`justify-items`属性的用法完全一致，但只作用于单个项目，取值为`start | end | center | stretch;`。  
`align-self`属性设置单元格内容的垂直位置，跟`align-items`属性的用法完全一致，也是只作用于单个项目，取值为`start | end | center | stretch;`。

* `stretch`默认值：拉伸，占满单元格的整个宽度。
* `start`：对齐单元格的起始边缘。
* `end`：对齐单元格的结束边缘。
* `center`：单元格内部居中。


`place-self`属性是`align-self`属性和`justify-self`属性的合并简写形式。 

## 参考

>http://ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html
