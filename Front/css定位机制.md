# CSS浮动

### CSS定位机制

1. ``普通流（标准流）``，默认定位机制，即从上向下，从作往右的排列

   - ``块元素``：独占一行；可设置宽、高；如果不设置宽度，宽度为容器宽度的100%，包括 div、p、 h1~h6、ul、ol、li、dt、dl、dd标签
   - ``行内元素``：与其他元素同行显示；不可以设置宽、高；行内元素的宽、高就是其内文字的宽与高，包括span、a、em、i、b、u标签 

2. ``float``，浮动元素

   - 浮动元素会``左右``移动，``不能上下移动``。

   - 浮动元素碰到``包含框``，另一个``浮动框``则停止

   - 浮动元素之后的元素会围绕，之前的不受影响 

   - 浮动元素会脱离标准流

   - 语法：

     ```css
     p{
       float:left;
       float:right;
       float:both
       float:none; 
     }
     ```

   - ``清除浮动``

     ```css
     clear: none|left|right|both; /* 清除方向的浮动 */ 
     ```

     清除浮动的三种常用方法：

     1. 在浮动元素后面添加一个空的元素``<div class="clear"></div>`` ，再在css中写``clear:both``即可。

     2. 给浮动元素容器（即浮动元素的父元素）添加一个``overflow:hidden;``  注意兼容ie版本``zoom:1;``

     3. 给浮动元素的父容器添加一个带有的``after``事件的属性，即给浮动元素父容器添加一个.clearfix类，如下：

        ```css
        .clearfix:after{  /*清除after事件*/
          content:".";
          display:block;
          height:0;
          visibility:hidden;
          clear:both;
        }    /*该方法类似于第一种添加一个空元素的方法*/:
        ```

        该命令需要添加如下内容来兼容低版本的IE浏览器，``.clearfix{zoom:1;}``

3. ``绝对定位`` 

   1. ``position:static``，自然定位模型

      - 即自然流，块、行垂直排列、行内水平从左至右
      - 如果相邻两个元素都设置了外边距，则最终外边距为``外边距中最大者``
      - 如果width和height固定（即元素内部主体固定），左右外边距设置成为``auto``，则左右外边距会扩大至100%的宽度，也就是使得这个元素``水平居中``。

   2. ``position:relative``，相对定位模型

      - 使得元素成为``containing-block``，即``相对于自己原先应该待的位置（自然流里的位置）进行偏移``
      - 可以使用``top/right/bottom/left/z-index``进行相对定位，可以控制浮动元素偏移，以及他们的堆叠顺序
      - 设置了relative属性后，原先的``自然流``的位置仍然``保留（即下一个自然流元素会跳过该位置）``
      - 它的后代的``绝对定位``是相对于他现在的位置（相对偏移后的位置）

   3. ``position:absolute``，绝对定位模型

      - 绝对定位的元素，``脱离了``常规流，因而可以使用相对定位``top/right/left/bottom``，以及``百分比值``

      - 不像relative，它``不会``保留原先应该在``自然流里的位置``

        ```css
        /* 用绝对定位来实现 水平、垂直居中 */
        .box{
          position:absolute;
          width:80px;
          height:80px;
          top:0;    /*上下左右绝对位置（相对于父元素）设置为0；*/
          bottom:0;
          right:0;
          left:0;
          margin: auto auto;  /*设置上下左右外边距为自动  则该元素就会居中显示*/
        }
        ```

      - 若其父元素设置了相对定位``relative``，而不是默认的``static自然流``，则设置绝对定位的子元素的位置设定，是相对于其父元素（即设置了relative的父元素）

   4. ``position:fixed``，固定定位模型

      - 和绝对定位一样，只是``固定定位，不会随着页面的滚动而变换位置``

   5. ``position:sticky``，磁贴定位模型

      - 作用类似于``relative + fixed``结合的效果，制造出吸附效果，是``css3.0``的新特性

      - 类似于``relative``，如果产生偏移，原来的``自然流``模式下的位置`` 保留``

      - 必须设置``top``等至少一个属性， 否则效果和relative一致。``top:0px``表示，当页面滚动到该元素显示效果为top==0px的时候，``sticky激活``

      - 效果如下，刚打开页面时候banner导航条在logo下面，但是当页面滚动到下方时，banner``始终在页面显示的顶部``

        <img src="./images/Screen Shot 2019-10-16 at 3.04.10 pm.png"/>

   

   ### CSS Postion总结

   <img src="./images/Screen Shot 2019-10-16 at 3.26.29 pm.png" />