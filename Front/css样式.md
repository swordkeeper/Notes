# CSS样式

### 文字样式

文字样式可以设置在标签``<font>``标签中

```html
<h1>
  <font face="Times" color="red" size="20px">这是一个有颜色的文字</font> <!-- 这样也可以控制文字的显示 -->
  <!-- 现已淘汰该标签，xhtml已经不支持该标签了 -->
</h1>  
```



#### 字体样式

- ``font-family``："宋体",  "Arial", "Times New Roman"   可以设置多个属性值，依次从头到尾检测字体，存在即显示。

  值可以是``字体名称``，也可以是``字体集``：``Serif``，``Sans-serif``，``Monospace``，``Cursive``，``Fantasy``

  ```css
  p{font-family:"宋体","黑体",sans-serif;} /* p标签使用，sans-serif字体集下的宋体和黑体，并依次检测宋体和黑体，存在则使用该字体 */
  ```

- ``font-size``字体大小，

  - 包括 绝对单位``in``,``cm``,``mm``,``pt``,``pc``，分别表示英寸、厘米、毫米、磅、Pica
  -  另一种 绝对单位``xx-small``，``x-small``，``small``，``medium``，``large``，``x-large``，``xx-large``
  - 相对单位，包括``px``，``%``。相对大小会随着显示器分辨率的大小变化而变化。（因而，在手机端，一般不用px和%) 。em和%都是相对于父元素的字体大小来计算的
  - 相对单位，包括``larger``，``smaller``，相对父亲元素字体变大，变小

- ``color``文字颜色

  - ``颜色名称``：red，green
  - ``rgb(124,255,0)``，红绿蓝的数值为0～255，``rgb(10%,50%,30%)``，红绿蓝百分比的值
  - ``#ff2e25``，16进制颜色值

- ``font-weight``文字粗细，

  - ``normal``，``bold``，``bolder``，``lighter``，``100~900``，分别表示正常，加粗，更粗，更细（比normal细），100～900之间的加粗值；400=normal，700=bold

- ``font-style``字体样式

  - ``normal``，``italic``，``oblique``，分别代表正常，斜体（一般表示文章引用），倾斜体

- ``font-variant``字体变形，设置元素中文本为小型大写字母

  - ``normal``，``small-caps``，分别代表正常和``小型大写字母``（即所有的字母都是大写的，但是尺寸要小很多）

- 可以省略font，简写的方式来写

  ```css
  p{
    font:30px "Arial" italic bolder small-caps
  }
  /* font-style font-variant font-weight font-size/line-height font-family 值是有书写顺序的 ，并且以空格隔开*/
  /* 这么写虽然不符合编程规范，但是会减少文件大小，减少传输css时候的网络压力 */
  ```



#### 文本格式

- ``text-align``：文本的对齐方式
  - ``left``，``right``，``center``，``justify``（两端对齐）
  - ``text-align``只对块级标签有效
- ``width``：文本宽度，总体宽度
- ``margin``
  
  ```css 
  p{margin:0 auto;}  /*设置上下边距为0，左右边距为自动*/
  ```

- ``line-height``，文本行高，可以是``长度值``，也可以是``百分比``

  ```css
  p{
    font-size:40px;    /* 设置字体大小为40px */
    line-height:20px;   /* 设置行高为20px，该设置不正确，因为行高小于字体40px的大小，因而会造成重叠的情况 */
    line-height:1.2em;   /* 因而通常会用百分比的方式来设置行高， 通常是1.2em 的行高
  }
  ```

- ``text-indent``,段落缩进

  ```css
  p{text-indent:2em;}  /*段落首行缩进两个字符*/
  ```

- ``vertical-align``,垂直对齐方式

  - ``baseline``，``sub``，``super``，``top``，``text-top``，``middle``，``bottom``，``text-bottom``，``长度|百分比``（上移+15px，下移-15px）
  - 垂直对齐方式，只对``行级元素``有效，对``块级元素``无效

  ```css
  /*要实现多行水平和垂直居中*/ 
  /* 因为行内元素不能包含块级元素，且vertical-align不能操作块级元素 ，因而只能写成这样*/
  .wrap1{
    display:table;   /*将外层转换成表格形式*/
  }
  .content{
    vertical-align:middle;		/*设置垂直居中*/
    text-align:center;			/*设置水平对齐*/
    display:table-cell;    /*将内层转换成表格cell形式*/
  }
  ```

  ```html
  <div class="wrap1">
    <div class="content">
        <p>
          段落1
        </p>
      	<p>
          段落2
       </p>
    </div>
  </div>
  
  <!-- 这样段落1和2就一起作为.content div的一个整体 水平垂直居中于.wrap div 中了
  ```

- ``word-spacing``词间距，``word-spacing:10px;``，``word-spacing:-2px``（字符重叠了）

- ``letter-spacing``字母间距，``letter-spacing:2em;``

- ``text-transform``文本内容转换，属性值包括``captialize``首字母大写,``uppercase``全大写,``lowercase``全小写

- ``text-decoration``文本装饰，``underline``下划线,``overline``上画线,``line-through``中画线,``blink``闪烁