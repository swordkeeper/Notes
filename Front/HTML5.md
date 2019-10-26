# HTML5

### 标签声明

``<!DOCTYPE HTML>`` 声明，DTD对HTML5的元素，以及含义进行了严格的定义

- HTML 4 的声明

<img src="./images/Screen Shot 2019-10-19 at 1.10.44 am.png"/>

- HTML 5的声明

  ``<!DOCTYPE html>``

### 标签变化

#### 结构化标签

结构化标签实际上就是``div``，只是负值了不同含义

1. ``<article>``标记定义一篇文章
2. ``<header>``标记定义一个页面或一个区域头部
3. ``<nav>``标记定义一个导航链接
4. ``<section>``标记一个区域，用于规划布局
5. ``<aside>``标记定义页面内容部分的侧边栏
6. ``<hgroup>``标记定义文件中的一个区块的相关信息，主要用于包含一组``<h1~6>``标签
7. ``<figure>``标记定义一组媒体内容以及它们的标题
8.  ``<figcatpion>``定义figure元素的标题
9. ``<footer>``标记定义一个页面或区域的底部
10. ``<dialog>``标记定义一个对话框

#### 多媒体标签

```html
<video src="1234.mp4" ></video>
```



1. ``<audio>`` 音频

   ```html
   <audio src="123.mp3" autoplay="autoplay" loop="-1" controls="controls"></audio>
   <!-- autoplay为播放选项，autoplay表示自动播放
   loop表示循环次数，可以填写整数值，-1代表无限次循环 
   controls属性表示，是否显示播放器的面板
   -->
   
   <audio autoplay="autoplay">   
   	<source src="123.mp3" type="audio/mpeg" />
   </audio>
   <!-- 如果需要资源转码，可以用source标签给audio标签传递资源，并用type标签来进行转码。 ‘audio/mpeg’表示转换成
   audio标签支持的mpeg播放格式 -->
   ```

2. ``<video>`` 视频

   ```html
   <video src="1234.mp4" autoplay="autoplay" controls="controls" width="1000px">
   	<!-- <source src="" type="audio/mpeg">  ->  
   </video>
   ```

3. ``<source>`` 媒体资源，提供资源的标签，内容可见上面

4. ``<canvas>``画布标签。类似的API库包括``d3.js``可以在此处画出任意想要的图形，线条表格等

5. ``<embed>``定义外部的，可以交互的内容或插件 ，比如flash、svg等。添加方式类似于video标签

   ```html
   <embed src="12345.swf" width="768"></embed>
   ```

   

#### Web 应用标签

##### 状态标签

1. ``<meter>`` 显示实时状态的标签（如温度、气压等）

   ```html
   <meter value="180" min="110" max="380" low="200" high="320" optimum="220">
   	<!-- 定义一个类似于电压的状态标签，value表示当前电压值
   	min表示该状态的下限，max表示上限，low表示低级区域， high表示高级区域
   	optimum 表示最优值-->
   </meter>
   ```

2. ``<progress>`` 显示进度的标签（如安装、加载）

   ```html
   <progress value="30" max="150"> </progress>  <!--最大值为150，表示现在只达到1/5-->
   <progress max="150"></progress>  <!--若没给当前值，则会出现一个加载动画 -->
   ```

##### 列表标签

1. ``<datalist>`` 为``<input>``标记定义一个下拉列表，配合``<option>``标签使用

   ```html
   <input placeholder="请选择：" list="phonelist" />
   <datalist id="phonelist">   <!-- id必须等于input list的值-->
     <option value="iphone">iphone</option>
     <option value="HTC">HTC</option>
   </datalist>
   ```

   

2. ``<details>``标签定义一个元素详细内容，配合``<summary>``使用

   ```html
   <details open="open">   <!-- 值为open 则默认是打开状态 -->
     <summary>这是《一本书》</summary>
     <p>
       这是一本书的内容。
       这是一本书的内容。
       这是一本书的内容。
       这是一本书的内容。
       这是一本书的内容。
     </p>
   </detail>
   ```

   

##### 其他标签

1. ``<ruby>``注释标签

   ```html
   <p>
     我们来<ruby>夼<rt>Kuang</rt></ruby>一个话题   <!-- 先用ruby标签包裹，然后用rt标签解释 --> 
     我们来<ruby>夼<rp>(</rp><rt>Kuang</rt><rp>)</rp></ruby>一个话题  
     <!-- 如果浏览器不兼容ruby标签，rp标签的内容就会显示，否则不显示 -->
   </p>
   ```

2. ``<mark>``标注标签，着重强调标签，标记文本，被mark的文本会黄色高梁显示

   ```html
   <p>
     小明，你妈叫你<mark>回家</mark>吃饭  <!-- 回家就会被突出显示 -->
   </p>
   ```

3. ``<output>``输出标签，需要配合``<oninput>``标签，来计算显示值的内容

   ```html
   <form oninput="totalprice.value=parseInt(price.value)*parseInt(number.value)" /> 
   	<!--oninput事件定义算式 -->
     <input type="text" id="price" value="5000" />*
     <input type="text" id="number" value="1" />=
     <output name="totalprice" for="price number"></output> <!--output标签显示结果值 -->
   </form>
   ```

   

### HTML5 属性变化

1. ``<input>``标签属性变化（主要是扩展移动端）

   - ``email``，效果类似于文本框（主要用于移动端）

     ```html
     <input type="email" name="email" />
     ```

   - ``url``，用于输入网址（主要用于移动端）

     ```html
     <input type="url" name="url" />
     ```

   - ``tel``，用于输入电话（主要用于移动端）

     ```html
     <input type="tel" name="tel" />
     ```
   - ``number``，用于输入数字（主要用于移动端）；在pc端，该文本框不能输字母（除e）

     ```html
     <input type="number" name="number" />
     ```
     
   
   - ``date pickers``，包括date、month、week、time、datetime（弃用）、datetime-local，（主要用于移动端）
   
     ```html
     <input type="date" name="date" />
     ```
   
     ```html
     <input type="month" name="month" />
     ```
   
     ```html
     <input type="time" name="time" />
     ```
   
   - ``range``，鼠标可以拖动的选择数值的遥杆
   
     ```html
     <input type="range" name="range" min="1" max="10"/>
     ```
   
   - ``search``，定义一个搜索框，可能会默认匹配输入字串
   
     ```html
     <input type="search" name="search"
     ```
   
   - ``color``，一个可以用来选择颜色的选择按钮
   
     ```html
     <input type="color" name="color"
     ```
   
     

2. ``<form>``表单属性

   - ``autocomplete``，该标签可以用于form标签，或者是其内部包含的input标签，值为on/off，默认情况是on

     ```html
     <form autocomplete="on">
       <input type="text" autocomplete="off" />
     </form>
     ```

   - ``autofocus``，自动将光标定位于某处，适用于所有的input标签

     ```html
     <input type="text" name="text" autofocus="autofocus" />
     ```

   - ``multiple``，可以选择多个值，适用于file 或email类型的input标签，用于上传多个文件或email

     ```html
     <input type="file" multiple="multiple" />
     ```

   - ``required``，表示该input是表单必填字段

     ```html
     <form>
       <input type="text" required="required" />
     </form>
     ```

3. 链接属性

   - ``<link>``标签的，新增的属性为``sizes``，

     ```html
     <link rel="stylesheet" href="cssURL" type="text/css" />
     ```

     链接小图标，为了不同分辨率的浏览器，引用不同大小的图标

     ```html
     <link rel="icon" href="icon.gif" type="image/gif" sizes="16*16" />
     ```

   - ``target``属性，``base标签``下的target属性. ``base标签写在head标签之内``

     ```html
     <base href="http://localhost" target="_blank" />  <!-- 让所有的超链接它的默认跳转方式都为_blank -->
     ```

   - ``a``标签的:

     - ``media="手持设备。。"``属性，表示对设备进行优化，如手持设备（电视，电器等）
     - ``hreflang="zh"``，设置链接语言
     - ``ref="external"``设置超链接的引用，这里超链接为外部链接。例如一个网站引入另一个网站的视频

4. 其他属性

   - ``<script type="text/javascript" src="xxURLxx.js">``标签

     - ``defer``，script的属性，指规定当前页面加载完之后，才会执行脚本
     - ``async``，script的属性，指规定，一旦脚本可以使用，则会异步执行。

   - ``ol``的属性

     - ``start``表示起始值
     - ``reversed``表示倒序排列

   - ``manifest``定义页面配置属性

     ```html
     <html manifest="cache.manifest" >
       <!-- 定义页面缓存，离线应用文件 -->
       <!-- 即，先写一个cache.manifest文件，然后用manifest引用该文件 -->
       <!-- 在cache.manifest文件里写入需要缓存的文件列表，
     	则浏览器就会缓存这些文件，包括.css .html .js文件都可 -->
     </html>
     ```

     