# CSS语法

CSS层叠样式表(Cascading Style Sheets)，用于定义HTML在浏览器中显示的样式

现在已经发展到了``CSS3.0``。在手机开发中偏向于用``HTML5``+``CSS3``的组合



### CSS样式规则

``CSS``=``选择器``+``声明``

```css
h1{color:red;font-size:14px;}  /* h1表示选择器， {}括号内的内容为声明，用来指明选择器的样式*/
```

CSS语句可以放在``<head>``标签之内，也可单独存储为``.css``的文件，然后在通过引用加载。若要引用CSS样式，无论是外部文件还是内部文件。需要添加``<style>``标签

```html
<head>
  <style type="text/css">
    h1{font-size:21px;}
    p{font-size:21px;}
    h2,h3,h4,h5,p{font-size:30px}  /*对不同的标签可以用逗号隔开，表示同时设置这些标签*/
  </style>
</head>
```

### CSS样式引入

1. 行内样式（内联样式）

   ```html
   <p style="color:red;font-size:20px">
     p的内容，就变成红色了
   </p>
   ```

2. 内部样式表（嵌入样式）

   ```html
   <head>
     <style type="text/css">
       <!--             /*此处和下面添加了<!-- -->html注释标签，因为低版本的浏览器可能不识别<style>标签，因而可能会把<style>标签的内容显示出来，添加了html注释标签则会注释掉其中的内容，而高版本可以解析*/
       h1{font-size:21px;}
       p{font-size:21px;}
       h2,h3,h4,h5,p{font-size:30px}  /*对不同的标签可以用逗号隔开，表示同时设置这些标签*/
       -->
     </style>
   </head>
   ```

3. 外部样式表

   写在外部，以``.css``为后缀的文件

   然后通过link标签``<link href="XX.css" rel="stylesheet" type="text/css" />`` 来引用。需要放在``<head>``之内

4. 导入式

   ```html
   <head>
     <style>
       @import url(./a.css);   /*url 外部导入 */
     </style>
   </head>
   ```

5. 4种样式导入方式的优先级``行内样式``优先级最高；``外部样式表``和（``内部样式表``+``导入式``）相比，谁在后面谁最终被执行；``内部样式表``和``导入式``，``内部样式表``优先级更高。

### CSS选择器

1. 标签选择器

   ```css
   h1,p{font-size:30px;}
   p{color:red;}
   ```

2. 类选择器

   css 用``[标签名]``+``.``+``class名``

   ```css
   .red{color:red;}  /*所有class=red的标签全部设置为红色 */
   .yellow{color:yellow;}   /*所有class=yellow的标签全部设置为黄色 */
   h1.yellow{font-size:30px;}  /*特殊的，将yellow类中的h1标签设置为30px大小的字体 */ 
   .one{text-decoration:underline;}
   
   ```

   html

   ```html
   <h1 class="red">header</h1>
   <p class="yellow">paragraph</p>
   <h1 class="yellow">paragraph</h1>
   <h2 class="yellow one">paragraph</h2>   <!-- 此标签同时属于两个类yellow 和 one ； 不同类用空格隔开 --> 
   ```

3. ID选择器

   css

   ```css
   #h1_id{color:red;}     /*   用 # 号来定位   */
   ```

   html

   ```html
   <h1 id="h1_id">header</h1>    <!-- 一个id只能用在一个标签上，即id是标签唯一的. -->
   ```

4. 全局选择器

   ```css
   *{color:red;}    /*   *号通配符可以匹配所有标签，实现全局效果。一般使用场景为，在一开始将所有的样式边距删除*/
   ```

5. 群组选择器

   ```css
   h1,h2,h3,h4{font-size:30px}  /* 用逗号隔开不同的标签，来实现群组效果 */
   ```

6. 后代选择器 

   ```css
   p em{color:red;} /* 给p标签下的em标签设置颜色，即p是em的父标签。即指定是p标签下的em标签 */
   p a em{color:red;}  /* 同理3级 */
   h1.special em{color:red;} /* 添加了类标签 */
   #pid em{color:red;}.  /* id 标签 */
   ```

   ```html
   <p>
     <em>im a hero</em>
   </p>
   ```

7. 伪类选择器

   ``<a>``标签，即链接标签有四个伪类，即 ``:link``,``:visited``,``:hover``,``:active``分别表示: 未访问，已访问，鼠标悬停，激活链接四种状态 

   Css

   ```css
   a:link{color:purple;}
   a:hover{font-size:30px;}
   ```

   html

   ```html
   <a href="#">链接</a>
   ```

   伪类的书写顺序必须是``:link``>``:visited``>``:hover``>``:active``

### 选择器优先级

1. ``id选择器``>``class选择器``>``标签选择器``

2. 多个class选择器，按照class 顺序加载样式，（跟标签中添加class的顺序无关，而与定义class的顺序有关，定义在后面的后加载，因而体现的也就是后class的效果）

3. 权值表

   - 通配符选择器：0

   - 标签选择器：1

   - 类选择器和伪类：10

   - ID选择器：100

   - 行内选择器：1000 

   - !important：10000

     ```css
     p b{color:red !important;}   /* !important 优先级最高 */
     ```

### CSS 样式命名规范

- 可以包含，``字母``、``数字``、``-``、``_``
- 不能以``数字``、``-``、``_``开头
- 常用命名
  - 页头：header
  - 页面主体：main
  - 页尾：footer
  - 内容：content/container
  - 容器：container
  - 导航：nav
  - 侧栏：sidebar
  - 栏目：column
  - 页面外围控制：wrapper
  - 左中右：left center right
  - 菜单：menu
  - 标题：title
  - 摘要：summary
  - 标志：logo
  - 广告：banner
  - 登陆：login
  - 登陆条：loginbar
  - 注册：register
  - 搜索：search
  - 标题：title