# CSS3

### 选择器

#### 子元素选择器

1. ``父元素 > 子元素``，直接后代选择器

   ```html
   <section>
   	<div>div article外</div>
      <article>
     		<div>div article内</div>
      </article>
   </section>
   ```

   ```css
   section > div{
     color:#f00;  /*只选择外，不选择内*/ 
   }
   ```

2. ``元素 + 兄弟元素``

   ```html
   <section>
   	<div>div article外</div>
      <article>
     		<div>div article内</div>
      </article>
      <article>
     		<div>div article内</div>
       </article>
   </section>
   ```

   ```css
   section > div + article{
     color:#f00;  /*只选择第一个article ，即跟section 下面的div紧挨着的article有效 
   }
   ```

   

3. ``元素 ～ 所有兄弟``

   ```html
   <section>
   	<div>div article外</div>
      <article>
     		<div>div article内</div>
      </article>
      <article>
     		<div>div article内</div>
       </article>
     	<article>
     		<div>div article内</div>
       </article>
     	<article>
     		<div>div article内</div>
        </article>
   </section>
   ```

   ```css
   section > div ~ article{
     color:#f00;  /*选择所有的article ，即跟section 下面的div后，所有的(不一定要连续）的article有效 
   }
   ```



#### 属性选择器

1. ``[]``属性

   ```html
   <a href="attribute.html"></a>
   <a href="#"></a>
   <a href="#1"></a>
   <a href="#2"></a>
   <a href="#3"></a>
   <a href="#4"></a>
   <a href="#5"></a>
   <a href="#6"></a>
   ```

   ```css
   a[href]{
     text-decoration:none;   /*匹配所有带 href属性的 a标签 */
   }
   a[href="#"]{
     color:red;    /*匹配所有带href 属性的a标签，且其值为#    */
   }
   a[href~="#"]{
      /* 选择器用于选取属性值中包含指定词汇的元素，只要是指定的属性，
     	并且属性值列表中包含value，而不是在某个值中以value开头或结尾，*/
   }
   a[href^="#"]{}  /* Element[attribute^="value"]匹配指定属性的指定value值开头的元素
   ，如果class中有多个value值，第一个值中的第一个字母不是指定的值，
   即使后面的属性中首字母是指定的值，那么也不能匹配上 */
   a[href$="#"]{}  /*  以#结尾 */
   a[href*="#"]{}   /* 匹配的是指定属性、并且属性值中包含value的都可以匹配到*/
   a[href|="#"]{}   /* 匹配的是指定的属性，并且属性值是以“value-“开头的元素 */
   ```

   

#### 动态伪类

1. 锚点伪类 ``:link``正常,``:visited``访问过的
2. 用户行为伪类：``:hover``鼠标接触，``:active``鼠标点击但不放开时候，``:focus``，光标切入则变化（光标进入input标签）



#### UI元素状态伪类

1. ``:enabled``，可以使用，如input标签

2. ``:disabled``，不可用，如input为灰色，不可用

3. ``:checked``，默认选择，如input标签的radio，即被选中后会出现-

   ```html
   <input type="text" disabled="disabled"/>
   <input type="text" disabled="enabled"/>
   <input type="radio" checked/>
   ```

#### 结构类选择器 :nth

1. ``:first-child``， 一个元素，如果是其父元素的第一个孩子，则被选中。同时被选中的还有其内部的元素

    `` 冒号： 前面的是要选择的 东西， 如果那个 要被选的东西   是 其父标签 第n个元素，则被选中。 顺带其内部内容也被选中``

   ```html
   <body>
     <section>
       	<div>1</div>
      	 <div>2</div>
      	 <div>3</div>
     </section>
     <section>
       	<div>4</div>
      	 <div>5</div>
      	 <div>3</div>
     </section>
   </body>
   ```

   ```css
   section:first-child{
     color:red /* 1 2 3 变红 */
      /*首先 section 出现在：前，则要选section
       如果这个section是它父元素的第一个子元素，则被选中。
       以及它内部的元素。
       */
   }
   ```

2. ``:last-child``，同理first-child

3. ``:nth-child()``，同理first-child，即，要选某个元素，某个元素如果是其父元素的第n个孩子，则选择器成立，括号里填数字，表示第几个元素。

   - 可以填写如下``:nth-child(n)``表示所有的数字，``nth-child(2n)``表示所有偶数的被选中，``nth-child(3n+2)``，同理，n默认表示（0～无穷，包括0）
   - 括号内也可以是``nth-child(odd)``或``even``

4. ``:nth-last-child()``，倒着数的第几个

5. ``:nth-of-type()``，匹配其父元素下，``第n个相同类型的元素``，这个元素可能不在第二个位置上，他只是第二个匹配

6. ``:nth-last-of-type()``，同理5

7. ``:fist-of-type``，同理5

8. ``:last-of-type``，同理5

9. ``:only-child``，如果匹配是父元素的唯一子元素，即父元素只有一个孩子，那这个孩子就是（当然，类型要一致）

10. ``:only-of-type``，类似9，这个是唯一的孩子类型，（可能有多个孩子，但是这个类型的只有一个）

11. ``:empty``，如果一个标签下，什么内容都没有（空格、子标签等，都没有），则被选中

12. ``:not()``，例如``label:not( label/伪类选择器)``，匹配非指定元素，父标签下的非xx类型子标签

    - `` nav > a:not(:last-of-type) ``，从nav下的a标签开始选，但不是最有一个的a都被选中



#### 伪元素选择器

元素::伪元素（element::pseudo-element)

1. ``element::first-line``，元素中的一段文字，只对第一行做个格式化，只对块级元素有效

2. ``element::first-letter``，选中某一段的首字母

3. ``element::before``，在元素原来的内容前面插入新内容，插入元素变为第一个子元素，是行级元素

   ```html
   <div>
     第一段字
   </div>
   ```

   ```css
   div::before{
     content:"新插入的元素";
     color:#ff0;
   }
   ```

4. ``element::after``，同理3，最实用，一般用于清除浮动

   ```html
   <head>
     <nav>
       bbb
     </nav>
     <div>
       aaa
     </div>
   </head>
   ```

   ```css
   head{
     background:red;
   }
   nav{
     height:50px;
     float:left;
   }
   div{
     height:80px;
     float:left;
   }     				 /*head中的两个元素的设置了浮动，则父元素的高度就会因为所有子元素都为浮动而变为0，
     						从而显示不出head的背景色*/
     						/* 因而需要在head最后面添加一个空标签，使它清除浮动*/
   head::after{
     content:"";      /* 添加的新元素的 内容为空。*/
     display:block;   /*. 添加的是快元素才行，才能够将父元素撑开。*/
   }
   ```

   

5. ``element::selection``，表示文本内容如果被光标选中，则会出现的效果，例如背景色改变，字体大小改变等





### CSS3 边框

1. 圆角``border-radius``属性

   - 语法 ``border:radius: 1~4 length|%``， 值为1～4，单位可以是长度也可以是%，四个值分别表示``左上``开始顺时针。

     ```css
     div{
      	width:800px;
       height:200px;
       border-radius:20px;  /* 边框就变成了圆角 */
       							/* 如果写成%，则%是  分别相对宽和高的%，因而一个长方形，会出现特殊效果 */
     }
     ```

2. 盒阴影``box-shadow``属性

   - 语法``box-shadow: h-shadow v-shadow blur spread color inset/outset;``，属性分别代表，水平、垂直阴影长度，模糊度，扩展多少，颜色，内阴影或外阴影（内阴影，表示阴影和主体对调）

3. 边界图片属性``border-image``属性
   - 语法``border-image: source slice width outset repeat;``，属性分别为
     - source：url()
     - Border-image-slice: number | % | fill; 表示图像的边界向内偏移多少，主要可以用%形式调节该属性和width，来达到预期效果



### CSS3 背景

1. ``background-clip``背景图像区域，背景图像绘制区域，实际上就是规定背景图片小时到box的什么地方，border-box表示边框也被纳入显示背景图片的范围，padding-box，则是边框部分也纳入显示背景图片的范围。``就是背景裁剪``

   - Background-clip: border-box | padding-box | content-box;  其值为下：
   - Border-box：从边框位置开始绘制背景图像
   - padding-box：从内边距开始绘制 
   - content-box，从内容部分开始绘制

2. ``background-origin``，指定``background-position``属性的相对位置

   - Padding-box
   - border-box
   - Content-box.     内容同上，就是指，``background-position``，是相对哪个位置的偏移

3. ``background-size``背景图片大小

   - length 大小值  ``单值100% 等于div宽度的100%大小``，``双值 第一个为宽，第二个为高``
   - percentage 百分值
   - cover ：特殊值，表 示``扩展或缩放图片`` ，使得``背景区域被充满``，如果图片过大，则裁剪多余部分
   - contain：一定要把``整张图片显示出来``，太大或太小，则某一边（高或宽）则会被压缩或是拉伸

4. ``background-image:url(1), url(2)``多重背景

   - 放在前面的，会显示在``最上面``，因而颜色深的，建议放到靠后的url中

   - 背景图片，书写顺序

     ``background: color position size repeat origin clip attachment url； ``

     建议书写格式如下（为了防止兼用性问题，把通用性的属性写到一起）

     ```css
     background: #abcdef url("a.png") no-repeat center 10px    /* 通用的，兼容性不会产生问题写一起 */
     				 					/* 背景颜色，背景图片， 重复与否，水平偏移center表示父元素中间，垂直偏移 */
     background-size:50%   /* 再写背景图片大小，单值表示水平百分比，双值表示高度值 */
     background-origin:content-box;  /* 再写，相对偏移的参考物 */
     background-clip:content-box;  /* 再写 裁剪范围。*/
     background-attachment: fixed; /*再写，粘滞（随屏幕滚动与否） */
     ```

     

   ### CSS3 渐变

   渐变可以实现在``2~多个颜色``之间实现平稳的过渡

   1. 线性渐变（linear-gradient)属性，从一个颜色过渡到另一个颜色

      - ``background:linear-gradient(direction, color-stop1, color-stop2...)``

        ```css
        /* 为了兼容性 可以写如下形式 */
        background: -webkit-linear-gradient(red,blue);    /* chrome前缀 */
        background: 	-moz-linear-gradient(red,blue);    /* 火狐前缀 */
        background: 	  -o-linear-gradient(red,blue);    /* opeara 前缀 */
        background: 	  	linear-gradient(red,blue);
        ```

      - 有方向的方法

        ```css
        /* 为了兼容性 可以写如下形式 */      /* 此出chrome和其他浏览器有些差别*/
        background: -webkit-linear-gradient(left,red,blue);    /* chrome前缀 left表示 start 的方向*/
        background: 	-moz-linear-gradient(right,red,blue);    /* 火狐前缀 right表示 end 的方向*/
        background: 	  -o-linear-gradient(right,red,blue);    /* opeara 前缀   right表示 end 的方向*/
        background: 	  	linear-gradient(to right,red,blue);   /*标准通用写法。加个to */
        ```

      - 对角渐变

        ```css
        /* 为了兼容性 可以写如下形式 */      /* 此出chrome和其他浏览器有些差别*/
        background: -webkit-linear-gradient(left top,red,blue);    /* chrome前缀 left表示 start 的方向*/
        background: 	-moz-linear-gradient(right bottom,red,blue);    /* 火狐前缀 right表示 end 的方向*/
        background: 	  -o-linear-gradient(right bottom,red,blue);/* opeara 前缀   right表示 end 的方向*/
        background: 	  	linear-gradient(to right bottom,red,blue);   /*标准通用写法。加个to */
        
        /* 除了chrome 是标志起始位置以外，其他的都是标志结束为止（通用方式是前面加to，并写结束为止） 
        方向的第一个参数表示左右（水平方向），第二个参数表示垂直方向（垂直方向）*/
        ```

      - 多颜色

        多个颜色就直接加到颜色参数列表后面就可以

      - 角度渐变

        ``background:linear-gradient(angle,color,color,color...)``

        <img src="./images/Screen Shot 2019-10-24 at 8.04.58 pm.png" width="300px" height="300px"/>

        ```css
        background:linear-gradient(45deg,red,yellow,blue)    /* 角度渐变，只影响中间的过渡成都，不影响起始位置和结束位置 */
        
        ```

        ``-webkit-``chrome浏览器内核与其他浏览器角度不太一样，角度变化如下图

        <img src="./images/Screen Shot 2019-10-24 at 8.42.20 pm.png" width=300px height=300px/>

        

      - 拉长渐变程度

        ```css
        background:linear-gradient(90deg, red 10%, orange 50%, yellow 60%, green 70%,blue 80%,indigo 90% violet 100%) /*不同百分比 影响渐变的距离，可以看到red到orange 渐变占了渐变的一半， 百分比表示渐变占比*/
        ```

      - 透明渐变

        ```css
        background:linear-gradient(90deg, rgba(255,0,0,0), rgba(255,0,0,1))  /* 透明度从0变到1）
        ```

   2. 重复线性渐变

      - ``background:repeating-linear-gradient(90deg, red 0%, blue 20%)``，从0%～20%，进行渐变，然后整个图片重复这0%～20%的渐变。

   3. 径向渐变（radial gradients) 属性，颜色从内向外渐变

      - ``background:radial-gradient(center,  size shape, color, color, color)``中心位置在哪里，颜色渐变的边长，镭射形状，颜色，颜色。。。（⚠️注意：shape 和 size之间没有逗号）

      - ```css
        background:radial-gradient(red,yellow,blue);
        ```

      - ```css
        background:radial-gradient(red 50%,yellow 70%,blue 100%);
        /* 在50%开始处从红色变为到70%的黄色  ， 该百分比是 中心点到四个角的距离的 分割 */
        ```

      - center``background:radial-grandient(30% 70%, red ,blue)``中心在30% 70%的位置

      - Shape 可以是``circle``圆形，``ellipse``椭圆（默认）

      - size 可以是 ``closest-side``最近边，``farthest-side``最远边，``closest-corner``最近角，``farthest-corner``：最远角。该参数表示，以``那个方向为长度的依据``

      

   4.重复渐变``repeating-radial-gradient()``，方法类似于线性重复渐变

   



### CSS3 文本

1. ``text-shadow``属性，属于文本的阴影

   - 比较先进的技术，兼容性比较差，兼容``IE10``,``firefox 3.5+``,``chrome4+``,``safari4+``,``opera9.5``

   - ``text-shadow:h-shadow. V-shadow  blur color; `` 

     ```css
     h1{
       text-shadow:5px 5px 5px red;   /* 阴影偏移水平5像素，垂直5px，
       		模糊度是向外的（不存在负值）模糊度为0px表示没有模糊，就是一模一样的字体， 红色表示阴影颜色 */
     }
     ```

2. ``text-outline``属性，文本的轮廓，描边

   - ``text-outline:thickness blue color;``，厚度，模糊度，颜色

3. ``word-break``属性

   - 属性规定自动换行的处理方法
   - ``word-break: normal | break-all | keep-all``
   - Normal，当一个框架装不上一段话时，文本就会换行，换行时，``会根据单词来调整，以保证单词的完整性``
   - Break-all，``不保证单词的完整性，进行换行``，即单词可能被截断
   - Keep-all，只能在半角空格或连字符处换行。

4. ``word-wrap``属性，允许长单词，或者URL地址换行到下一行

   - ``word-wrap:normal | break-word``，break-word表示可以截断单词进行换行

5. ``text-align-last``规定，文本的最后一样对齐方式

   - ``text-align-last: auto |left|right|center|justify|start|end|initial|inherit``

6. ``text-overflow``规定文本发生溢出时的处理情况

   - 要是用这个属性，就一定要添加``overflow:hidden``

   - ``text-overflow: clip| ellipsis | string``

   - Clip 剪掉多出来的

   - ellipsis。裁剪多出来的，并用。。。表示后面省略。

   - string，表示用自定义符号来代替。。。（ellipsis）

     ```css
     p{
       overflow:hidden;
       text-overflow:clip; /* 裁剪 */
       text-overflow:ellipsis;  /* 裁剪 并用... 指示有省略 */
       text-overflow: ">>";    /*. 用>> 符号代替...    目前只有火狐浏览器有效*/。
     }
     ```

     



### css3 字体

1. ``@font-face``语法规则

   ```css
   @font-face{
     font-family:<yourWebFontName>;
     src:<source>[<format>],<source>[<format>]...;   /* format. 定义一个字体格式 ，帮助浏览器识别*/
     [font-weight:<weight>];
     [font-style:<sytle>];
   }
   ```

   - ``format``字体格式，

     1. TrueType(.ttf)格式：是windows和mac最常见的字体，可以用到手机端
     2. OpenType(.otf)格式，是一种原始的字体格式，是建立在TrueType基础上的，并提供了更多功能
     3. Web Open Font Format(.woff)格式：是一种``Web最佳格式``，他是一个Truetype/openType的``压缩版本``，同时也支持元数据包分离。不支持IOS手机端
     4. Embedded Open Type(.eot)格式：是IE专用字体
     5. SVG(.svg)渲染格式的字体：比较流行

   - 所以在实际应用中一般使用：.``woff``+``.eot``，即最佳网络格式配合ie格式的字体，有时候还要配合``.svg``格式字体，如下图

     <img src="./images/Screen Shot 2019-10-24 at 11.37.30 pm.png" />

   - 例子

     ```html
     <h1>
       今天天气好晴朗，处处好风光。嘿呦嘿。
     </h1>
     ```

     ```css
     <style>
     @font-face{
       font-family:"myFont";  /*自定义一个字体名 */
       src:url("myFont.eot"),    /*  ie9 model */
     		url("myFont.eot?#iefix") format("embedded-opentype"), /*. ie6~8. */
           url("myFont.ttf") format("truetype") ,  /*safari, android, ios */
           url("myfont.woff") format("woff"), /*modern Browsers */
           url("myFont.svg#myFont") format('svg')  /* legacy ios */
     }
     
     h1{
       font-family: "myFont";
     }
     </style>
     ```

   - 获取特殊字体的一个网站，它能将一个字体上传后，再转换别的格式，你可以通过下载，获得兼容各个浏览器的字体文件

     <a href="https://www.fontsquirrel.com/tools/webfont-generator" >https://www.fontsquirrel.com/tools/webfont-generator</a>





### CSS3 转换transform

1. transform 可以实现：

   - ``rotate()``旋转

     - ``transfrom:roate(angle)``旋转一个角度

        ```css
       -webkit-transform:rotate(20deg);
       	-moz-transform:rotate(20deg);
       	 -ms-transform:rotate(20deg);
       	  -o-transform:rotate(20deg);
       		  transform:rotate(20deg);
       ```

   - ``translate()``平移

     - ``tranform:translate(x,y)``，根据现在位置进行水平和垂直方向移动 

   - ``scale()``缩放

     - ``transform:scale(0.5,0.3)``水平缩放到0.5，垂直缩放到0.3，缩放的中心是图片的中心，水平中心和垂直中心。可以写单参数，表示水平、垂直都缩放，可以实现等比例缩放			

   - ``skew()``扭曲或斜切

     - ``skew(x,y)``
     - ``skewX(15deg)``，变成了一个``平行四边形``

2. 3D transform

   - ``rotateX()`` 旋转

     X方向的旋转，可以参考下图

     <img src="./images/Screen Shot 2019-10-25 at 4.29.03 pm 2.png" />

   - ``rotateY()``旋转，同理

   - ``rotateZ()``旋转，直观上是以中心旋转

   - ``rotate3d(x,y,z,angle)``，x，y，z取值为0~1，angle表示旋转角度，若只让y轴旋转45度则为（0，1，0，45deg），若让z，x轴同时旋转60度，则为``rotate3d(1,0,1,60deg)``

   - ``translateZ(z)``，沿着z轴方向移动，正值表示``图片更靠近我们的眼睛``，负值表示远离眼睛

   - ``translate3d(x,y,z)``，同理

   - ``scaleZ(z)``，图像沿着z轴方向缩放。图片视觉上没有变化，因为图片没有厚度。

   - ``scale3d(x,y,z)``，同理缩放，三个参数不允许省略

3. ``transform-origin``属性

   - 允许用户改变transform的坐标基准点（原点）

   - ``transform-oringin: x y z; ``

   - ```css
     transform-origin:left top;   /* 2d 情况下，以区域的左上角为基准点进行操作（旋转，平移，缩放，斜切等）
     ```



### CSS3 扩展属性

1. ``transform-style``，表示嵌套元素是， 怎样在三维空间中呈现的。需要设置到父标签中，来实现子标签的效果

   - ``transform-style: flat|preserve-3d``

   - Flat表示平面

   - preserve-3d 表示3d显示，可以实现，一部分在一个图层前面，一部分在图层的后面，如下图的``白圈，一部分在前，一部分在小人图层后面``

     <img src="./images/Screen Shot 2019-10-26 at 1.24.33 am.png" width=600px/>

2. ``perspective``属性，观察者与z=0平面的距离，使得元素具有透视效果，``当图像的某部分的z坐标大于perspective的值时，那部分不会被显示出来，即在观察者背后的部分，不会被显示``

   - ``perspective: number | none; ``默认为none

   - ```css
     perspective:500px;  /*数值越大，表示观察距离越远 */
     ```

3. ``perspective-origin``属性，

   - ``top|bottom|center``，分别表示不同的观察角度

4. ``backface-visibility``属性，表示背面是否可见

   - ``backface-visibility:hidden | visiable``
   - 当我们旋转一个div时候，旋转180度时候，这个div上面的字就因该跟着这个div平面，``转过去``，因而如果是hidden，我们就看不到这个div上的文字了，如下图的区别

   <img src="./images/Screen Shot 2019-10-26 at 1.53.40 am.png" width=400px/>





### CSS3 过渡

``transition``过渡，区别``transform``转换

- ``transition`` 在一个时间区间内缓慢的``平滑的从一种状态过渡到另一种状态``.

1. ``transition-property``：检索或设置对象中的参与过渡的属性

   - ``transition-property: none | all | 或者具体属性名称 ``

   - ``none``没有属性改变

   - ``all``所有属性改变，是默认值

   - ``property``，元素的属性名称

   - ```css
     transition-property:color;  
     ```

   - 例子：

     ```css
     div{
       transform:rotate(0deg);   /*正常时候没有旋转 */
       -webkit-transition-property: transform;  /* 给transform属性 设置上过度，即正常时朝rotate(0deg)过渡*/
       	-moz-transition-property: transform;
       	  -ms-transition-property:transform;
       	   -o-transition-property:transform;
      			  transition-property:transform;
     }
     div:hover{
       transform:rotate(180deg);   /*鼠标悬停时候旋转 */
       -webkit-transition-property: transform;/* 给transform属性 设置上过度，即悬停时朝rotate(180deg)过渡*/
       	-moz-transition-property: transform;
       	  -ms-transition-property:transform;
       	   -o-transition-property:transform;
      			  transition-property:transform;
     }
     ```

2. ``transition-duration``属性，设置transition所需要的时间

   - ``transition-duration:time;``，默认时间时0，可以以秒或毫秒设置 

     ```css
     div{
       transform:rotate(0deg);   /*正常时候没有旋转 */
       -webkit-transition-property: transform;  /* 给transform属性 设置上过度，即正常时朝rotate(0deg)过渡*/
       	-moz-transition-property: transform;
       	  -ms-transition-property:transform;
       	   -o-transition-property:transform;
      			  transition-property:transform;
       -webkit-transition-duration: 2s;  /*给div设置2s，表示该div从鼠标移开恢复原状需要2s*/
       	-moz-transition-duration: 2s;
       	  -ms-transition-duration:2s;
       	   -o-transition-duration:2s;
      			  transition-duration:2s;
       
     }
     div:hover{
       transform:rotate(180deg);   /*鼠标悬停时候旋转 */
       -webkit-transition-property: transform;/* 给transform属性 设置上过度，即悬停时朝rotate(180deg)过渡*/
       	-moz-transition-property: transform;
       	  -ms-transition-property:transform;
       	   -o-transition-property:transform;
      			  transition-property:transform;
               -webkit-transition-duration: 2s;  /*给div:hover设置2s，表示该div从鼠标悬停开始需要2s*/
               -moz-transition-duration: 2s;
                 -ms-transition-duration:2s;
                  -o-transition-duration:2s;
                   transition-duration:2s;
     ```

3. ``transition-timing-function``属性

   - ``transition-timing-function: ease | linear | ease-in | ease-out | ease-in-out | step-start | step-end |steps(<integer>) | cubic-bezier(number , number , number , numer)``
   - ``linear``: 线性过渡，等于被贝塞尔曲线（0.0 , 0.0,  1.0, 1.0) ，即匀速
   - ``ease``：平滑过渡，等于被贝塞尔曲线（0.25, 0.1,  0.25, 1.0)   平滑过渡
   - ``ease-in``：有慢到快
   - ``ease-out``：有快到慢
   - ``ease-in-out``，慢——快——慢

4. ``transition-delay``属性

   - ``transition-delay:5s``，延迟5秒再执行过渡。默认不写delay是0s

5. ``transition``综合写法

   - ```css
     transition:property duration timing-fucntion delay;  /*对顺序有要求 */
     ```





### CSS3 动画

1. ``animation-name``属性，指定动画名称

   - ``animation-name: keyframename | none;``

   - Keyframename: 指定要绑定到选择器的关键帧的名称，可以有多个值，用逗号隔开

   - none：没有动画，默认值

     ```css
     div{
       animation-name:circle_inner;  /*使用到了自定义的动画，自定义的动画名为circle_inner 动画定义如下*/
     }
     ```

     

2. ``@keyframes``定义动画

   ```css
   @keyframes circle_inner{    /* 用关键词 keyframes 定义一个动画，动画名叫circle_inner */
     from{transform:rotateX(0deg);}		/*类似于transition，该动画从from 中的属性状态，变化到to中的属性状态*/
     to {transform:rotateX(360deg);}
   }
   @-webkit-keyframes circle_inner{      /* 兼容写法 */
     from{transform:rotateX(0deg);}		
     to {transform:rotateX(360deg);}
   }
   ```

   - from / to 都是``keyframes-selector``，from表示0%，to表示100%，也可以填写别的百分比值

3. ``animation-duration``属性，类似于``transition-duration``

   - ```css
     animation-duration:5s.  /* 必须要写的属性，默认值为零，表示没有动画效果，所以一定要设置改属性*/
     ```

4. ``animation-timing-function``，类似于``transition-timing-function``

   - 用于给动画加时间变换形式，最好用的还是值为``linear匀速 ``和``ease-in-out 慢-快-慢``

5. ``animation-delay``属性，类似于``trnsimition-delay``

   - 用于定义动画延时开始，默认为0s（表示直接开始），animiation可以不填写该delay属性

6. ``animation-iteration-count``，设置动画打循环次数

   - ``animation-iteration-count:infinite|<number>``
   - indinite ：无限循环次数播放
   - <number>：填写数字，即循环n次

7. ``animation-direction``，表示动画的方向

   - ``animation-direction:normal | reverse | alternate | alternate-reverse |initial |inherit``
   - normal 正常方向
   - reverse：反方向运行
   - alternate：先正方向，再反方向，并持续交替
   - alternate-reverse：先返方向，再正方向

8. ``animation-fill-mode``，

   - 当动画播放完毕时，动画保持的样式，元素的样式
   - ``animation-fill-mode: none| forwards |backwards | both | initial | inherit``
   - None , 默认值，不设置动画之外的动画
   - forwards : 设置对象状态为动画``结束``时候的状态
   - Backwards: 设置对象状态为``动画``开始时的状态，(即回到最开始)
   - Both : 设置对象状态为动画结束或开始的状态

9. ``animation-play-state``

   - ``animation-play-state:paused | running `` ， 指暂停动画  或正在运行动画（默认值）

10. 综合写法

    - ``animation: name duration timing-function delay iteration-count direction  fill-mode play-state``，必须值为name 和 duration ， 写法对顺序没有要求，但是因为duration是必须的，所以会优先匹配duration而不是delay

### 防止图片抖动

1. 图片抖动的愿意主要是因为浏览器重复的不停的``渲染图片``，即刷新图片。解决办法

   - Position-fixed 代替 background-attachment

   - 带图片的元素放在为元素中

   - 骗取3d加速 hack方法

     ```css
     translateZ() hack   /*通过添加一个z轴变化但是没有实际效果的属性功能，来骗取GPU加速3d。hack表示加速
     ```

   - 用``will-change``属性，通知计算机用``GPU``来进行加速渲染，占用内存和GPU资源

     - ``will-change:auto | scroll-position | contents | <custom-ident> | <animateable-feature>;``

     - Scroll-position: 表示脚药改变元素的滚动位置

     - contents：表示将要改变元素的内容

     - <custom-ident>：明确指定将要改变的属性与给定的名称，用的最多

       ``will-change: transform``

     - <animateable-feature>：课动画的特征，比如left、top、margin

     - 注意：使用will-change最好用在伪类之中例如``:hover``，这样在GPU渲染结束后，这个伪类就消失了，GPU资源就释放掉了，否则会长期占用GPU资源。通常的做法是，用js动态控制它的结束