# CSS网页布局

 ### 行布局

1. 行布局让``div水平居中``

   css

   ```css
   .container{
     width:1000px;
     height:2000px;
   }
   .content{
     width:300px;
     height:300px;
     margin:0 auto;     /* 设置外边距上下为0，左右auto，则左右margin自动占满上一级div的大小，因而就会
     					挤的content水平居中；垂直居中同理 */
   }
   ```

   html 

   ```html
   <div class="container">
     <div class="content">  
       <!-- container容器中有个，content容器，现在要让content容器在container容器中居中显示-->
     </div>
   </div>
   ```

2. 让``div宽度随浏览器宽度变化``

   html

   ```html
   <body>
     <div class="content">
       <!-- 浏览器宽度大小自动改变body的宽度。-->
     </div>
   </body>
   ```

   css 

   ```css
   .content{
     width:80%;   /* 让 div 的宽度为浏览器宽度的80%，这样浏览器大小变动，该div的大小也就变动 */
     margin:0 auto;   /* 让该 div 水平居中，即始终处在浏览器中央 */
     max-width:1000px;   /* 让 div 有个像素的限制，即无论用户调节浏览器大小，当没有超过该像素限制时，div随浏览器
     							大小变动而变动；但是超过该限制，浏览器大小的变动将不会影响 div 的大小 */
   }
   ```

3. 常用的让div``垂直、水平同时居中``

   html 

   ```html
   <body>
     <div class="content">
       
     </div>
   </body>
   ```

   css 

   ```css
   .content{
     width:300px;
     height:120px;  /* 类似于百度首页搜索条，先设定好大小 */
     position: absolute;    /* 设置绝对相对位置 */
     top:50%;
     left:50%;     /* 设置绝对相对位置后，在设置相对于父元素的偏移，偏移50%， 这样content的左上角点，就会居中 */
     margin-top: -60px;
     margin-left:-150px; /* 在对外边距进行设置，使得该div 能够在向上和向左移动半个元素大小的位置，这样就居中了 */
   }
   ```

4. 经典行布局

   <img src="./images/Screen Shot 2019-10-16 at 6.11.23 pm.png" />

   即，页面的组成，是一各个主行块组成。

### 多列布局

#### 两列布局

1. 效果图如下

   <img src="./images/Screen Shot 2019-10-16 at 8.19.12 pm.png" />





#### 三列布局

1. 布局如下

   <img src="./images/Screen Shot 2019-10-16 at 9.50.05 pm.png" />



### 圣杯布局

1. 布局如下

   <img src="./images/Screen Shot 2019-10-17 at 12.40.13 am.png" />

2. 要求中间栏在浏览器中优先展示渲染

3. 允许任意列的高度最高

   ```html
   <div class="container" >
     <div class="middle" >  中间  </div> <!--优先显示中间,所以先定义 -->
     <div class="left"> 左侧</div>
     <div class="right">右侧</div>
   </div>
   ```

   ```css
   body{
     min-width:700px
   }
   .container{
     padding:0 220px 0 220px;   /* 圣杯布局的关键，让中间容器左右缩进 */
   }
   .left , .middle , .right{   /* 圣杯布局的核心*/
     position:relative;
     float:left;
     min-height:300px;
   }
   .middle{
     width:100%   /*中间容器左右100%显示缩进后的contaienr*/
   }
   .left{
     width:200px;  
     margin-left:-100%;
     left:-200px;
   }
   .right{
      width:200px;
     margin-left:-220px;
     right:-220px;
   }
   ```

   

### 双飞翼布局

1. 双飞翼布局去掉了相对布局，只需要浮动和负边距
2. 双飞翼和圣杯布局最重要的就是``margin-left:-100%``，margin为负数时，使得整体``float``的块元素上移