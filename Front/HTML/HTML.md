# HTML

### HTML(Hypertext markup language) 超文本标记语言

#### 背景知识

1. 2014年 W3C推``HTML5``

3. ``HTML``文件：
   1. 不需要编译，浏览器解释执行
   2. 通常后缀``.html``，``.htm``
   3. 大小写不敏感的文本文件

#### HTML基本结构

```html
<html>
 	<head>
    	<title>标题</title>
  </head> 
 	<body>
    	网页主体
  </body>
</html>
```

#### HTML 基础和细节
1. ``html``文件由一个个嵌套的``元素``组成，即``<标签名 属性1=“属性值” 属性2=“属性值2”...> 标签内容 <\标签名>``

2. ``注释``  =  ``<!-- 注释这么写-->``

3. 声明HTML``版本类型``，需用``<!DOCTYPE html>``，该标签需要放置到文档的首行。

4. 有时候，尤其是在windows平台下，打开编写的html会出现中文乱码的情况。

   该情况是由于文本编码问题。即浏览器默认解析文本文件不是通常的UTF-8。
     解决方案:

   ```html
   <head>
     <meta http-equiv="Content-Type" content="text/html" charset="utf-8" />
     <!-- 元数据解析 http 是文本html类型，使用的字符集为utf-8 -->
     <!-- 中国通常使用 UTF-8， 或是 GB2312 或是 GBK -->
     <!-- UTF-8 支持 简体中文，繁体中文，日文，韩文等 -->
     <!-- GB2312 支持 简体中文 -->
   </head>
   ```

   

#### HTML 基础标签

1. 文字和段落

   ```html
   <h1> 标题1 </h1> <h6> 标题6 </h6>
   <p> 段落标签 </p>
   段落 标签的对齐属性 align：
   	1. left，左对齐
   	2. right，右对齐
   	3. center, 中间对齐
   	4. justify，进行行伸展，这样每行都可以有相同的长度
   <br/> 换行标签 
   &nbsp; 空白，即空格，需要加分号
   ```

   html标准推荐，所有的显示文字都需要有对应的文本标签引用，否则不符合规范。

   - ``<h1>～<h6>``段落标题标签
   - ``<p>``段落标签，有属性``align``值为**left**，**right**，**center**，**justify**，justify表示每行都一样长度
   - ``<br/>``换行标签
   - ``<hr/>``画水平线
   - ``&nbsp;``空格，**注意**有分号
   - ``<pre>``标签，正常的文本在文件中的排版是不会体现在浏览器里面的，加上pre标签后，html文件里是什么排版，浏览器解析就是什么排版 
   - 修饰标签
     - ``<i>``和``<em>``文字斜体
     - ``<b>``和``<strong>``文字加粗
     - ``<sub>``下标，``<sup>``上标
   - 特殊符号（转义字符）
     - ``&lt;``左尖括号``<``
     - ``&gt;``右尖括号``>``
     - ``&reg;``注册商标``®️``
     - ``&copy;``版权``©️``
     - ``&trade;``商标``™️``

2. 列表标签

   - 无序列表：``<ul>``列表，``<li>``列表项。``<ul type>``type属性，定义了无序列表显示的列表标记，disc表圆点，square表正方形，circle表空心圆  

   - 有序列表：``<ol>``列表，``<li>``列表项。``<ol type>``属性包括，1表示数字，a表示小写字母，A表示大写字母，i表示罗马小写，I表示大写罗马字母

   - 定义列表：``<dl>``列表，``<dt>``列表项，``<dd>``列表项描述。

     ```html
     <dl>
       <dt>列表项</dt>
       <dd>列表项描述</dd>
       <dt>列表项</dt>
       <dd>列表项描述</dd>
     </dl>
     ```

3. 图片标签

   - ``<img src="">``，src是图像的必填属性。其他属性包括``alt``替代文字（鼠标移上去显示的文字）；``height``高，``width``宽，可以数字像素``50px``或是百分比``30%``

4. 超链接标签

   - ``<a>``标签，属性：

     - ``href``，填写一个url，可以是相对地址，也可以是绝对地址。可以覆盖图片、文字、段落等

     - 若想保留一些链接效果，但是还没有准备好特定的URL地址，可以临时填写``href="#"``，作为``空链接`` 

     - 说想要点击``刷新页面``，则``href=""``

     - ``target``链接目标窗口，值可以为``_self``，``_blank``表示新打开一个标签，``_top``，``_parent``

     - ``title``，链接提示文字

     - ``name``，定义锚，命名

       ```html
       <!-- first.html 页面 -->
       <a href="second.html#apple">跳转到另一页面的锚点apple出</a>
       
       <!-- second.html页面-->
       <!-- 图片前面定义了一个锚点-->
       <a name="apple"></a><img src="url"/> 
       ```

     - ``邮箱``

       ```html
       <a href="mailto:邮箱地址"> 点击我，就会打开邮箱发送 </a>
       ```

     - ``文件下载``

       ```html
       <a href="文件URL"> 点击我，就可以直接下载 </a>
       ```

       

#### HTML 表格

1. 表格的标签

   ```html
   <table>						   <!-- 表格 1*3 -->
     <caption>标题</caption>  <!-- 表格标题标签 -->
     <tr>							<!-- 行 -->
       <th>表头</th>        <!-- 表头标签，默认剧中显示，默认加粗显示 --> 
       <th>表头</th>        <!-- 表头标签，默认剧中显示，默认加粗显示 --> 
       <th>表头</th>        <!-- 表头标签，默认剧中显示，默认加粗显示 --> 
     </tr>
     <tr>
        <td>content</td>     <!-- 单元格标签 -->
        <td>content</td>
        <td>content</td>
     </tr>
   </table>
   ```

   

2. Table 属性

   - ``border``表格的边框大小 pixels
   - ``width``宽度  pixels、 %
   - ``align``对齐方式，left、center、right
   - ``bgcolor``颜色
   - ``cellpadding``单元格边缘与内容之间的空白，pixels、%
   - ``cellspacing``单元格之间的空白，pixels、%
   - ``frame``规定外侧边框哪个部分可见（table边框）
   - ``rules``规定内侧边框哪个部分可见（单元格边框）

3. 表格的优化

   有时候一个内容很大的表格，浏览器在加载时需要等待所有表格数据全部加载完毕才会显示。这样``加载速度慢，用户体验差``。因而产生了表格优化选项：包括``<thead>``，``<tbody>``，``<tfoot>``标签。用这些标签覆盖``<tr>``标签，会加速表格的显示。

   ```html
   <table>
     <caption></caption>
     <thead>   <!-- 用thead包住 -->
           <tr>
               <th>表头</th>
            	 <th>表头</th>
           </tr>
     </thead>
     <tbody>  <!-- 用tbody包住 -->
       	<tr>
           	<td>数据</td>
           	<td>数据</td>
       	</tr>
     </tbody>
     <tfoot></tfoot>   <!-- 同理 tfoot -->
   </table>
   ```

4. tr 标签属性

   - ``align`` 水平对齐，left、center 、right 、 justify 、char
   - ``valign``垂直对齐，top、middle、bottom、baseline
   - ``bgcolor``

5. td  标签属性

   - ``height``高度设置
   - ``width``
   - ``valign``
   - ``align``
   - ``bgcolor``

6. colspan 属性

   ```html
   <table>
     <tr>
       <td colspan="2"></td>    <!-- 一个td占一行的两个td的位置 -->
     </tr>
   </table>
   ```

7. rowspan 属性

   ```html
   <table>
     <tr >     <!-- 一列占两列的位置 -->
       	<td rowspan="2"></td>    <!-- 无论是colspan 还是rowspan，都应该是td标签的属性
     </tr>
   </table>
   ```

   

#### HTML 表单

1. 表单标签``<form>``

   - ``action``提交的url
   - ``method``提交的方式get、post
   - ``target``提交后打开页面选项
   - ``enctype``在发送表单数据之前如何对其编码，可选``application/x-www-form-urlencoded``，``multipart/form-data``，``text/plain``

2. 表单标签可以内嵌很多其他种类的元素，包括

   - ``<input>``表单输入标签，和其属性

     ```html
     <input type="类型属性" name="名称"  ../> 
     ```

     - ``name``属性：文字域的名称
     - ``maxlength``属性：用户可输入到文本框的最大字符数
     - ``size``：指定文本框大小，默认是输入20个字符的大小
     - ``value``：默认值
     - ``placeholder``：规定用户填写输入字段的提示，即用户光标放入文本框时，文本框显示的灰体提示

   - ``<textarea>``文字域标签

     - ``rows``行属性
     - ``cols``列属性
     - ``placeholder``提示字符

   - ``<select>``表单和列表标签

     ```html
     <select>
       <option selected>北京</option>   <!-- selected参数为默认选择值 -->
       <option>上海</option>
       <option>广州</option>
       <option>深圳</option>
     </select>
     ```

     - ``name``select的属性：下拉菜单的列表名
     - ``multiple``设置可选择多个选项
     - ``size``设置列表中可见的选项数目

   - ``<optgroup>``表单和列表项目分组标签

     ```html
     <select>
       <optgroup label="组1">  <!--label 属性的值会显示到列表中 -->
         <option></option>
         <option></option>
         <option></option>
       </optgroup>
        <optgroup label="组2">
         <option></option>
         <option></option>
         <option></option>
       </optgroup>
       
     </select>
     ```

     

#### 网页布局

1. ``<div>``标签，它是一个垂直容器标签。其中的每一个标签内容占据一行（块级标签），会占领一行的都叫``块级标签``
2. ``<span>``标签，它是一个水平容器标签。其中每一标签内容占据一列（行内标签），不会单独占领一行的叫``行内标签``
3. ``<h1> ~ <h6>；<dt>; <p>``块标签**不能** 包含其他``块标签``

 