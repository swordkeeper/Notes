# Jquery

 ## 选择器

1. ID 选择器

   ```html
   <div id="first"></div>
   ```

   ```javascript
   $(document).ready(function(){
     	$("#first")
   });
   ```

   

2. 标签选择器

   ```html
   <div>
   </div>
   <a></a>
   ```

   ```javascript
   $(document).ready(function(){
   	var divs = $("div"); //这个返回的是一个数组
   });
   ```

3. 类选择器

   ```javascript
   $(document).ready(function(){
     var divs = $(".divFun"); // 返回一个数组
   })
   ```

4. 所有元素

   ```javascript
   $(document).ready(function(){
     var all = $("*");
   })
   ```

5. 多项选择器

   ```javascript
   $(document).ready(function(){
     var vv = $("selector1,selector2,.class1,.class2, #id1"); 
   })
   ```

6. 层级选择器

   ```html
   <div>
      <nav></nav>
   </div>
   ```

   ```javascript
   $(document).ready(function(){
    	var vv = $("div nav"); //只选择div下的nav
      // 该方法被称作后代选择器， $("ancestor descendant"); 该方法会选择所有的后代
     
      var vv1  = $("div > nav"); // 只选择直接后代，隔代不会被选中
     
      var vv2 = $("div + nav"); // 后继选择器，选择nav元素，而且其前一个同级别节点为div元素、
     // 语法为 $("prev + next")
     
     var vv3 = $("div ~ nav"); // 兄弟选择器，选择所有跟div同级的，位于div之后的nav元素
     // 语法为 $(" prev ~ siblings")
   })
   ```

7. 属性选择器

   ```html
   <div ttt="cnis" ></div>
   <div ttt="opq_cn"></div>
   <div ttt="opq_en"></div>
   ```

   ```javascript
   $(document).ready(function(){
     var vv = $("[ttt]");  //找出拥有ttt属性的元素
     var vv1 = $("[ttt=cnis]")  // 找出属性ttt 等于cnis的元素
     var vv2 = $("[ttt=cnis]")  // 找出属性ttt 不等于cnis的元素
     var vv3 = $("[ttt^=opq_]")  // 找出属性ttt 以某个值开头的元素
     var vv4 = $("[ttt$=cn]") // 找出属性ttt 以某个值结尾的元素
     var vv5 = $("[ttt*=_]") // 找出属性ttt 包含某个值的元素
   })
   ```

8. 子选择器

   ```javascript
   $(document).ready(function(){
     var first = $("div:first-child");  // 选中一个div，该div是其上层元素的第一个孩子，也即该层的第一个元素
     var last = $("div:last-child");  // 选中一个div，该div是其上层元素的最后个孩子
     var nchild = $("div > p:nth-child(1)"); // 第1个元素
     // 注意：最小序号为1 ，而不是序号0
     // :nth-child(n|even|odd|formula)
     var lnchild =$("div:nth-last-child(3)") // 倒数第3个元素
     var onlychild = $("div > p:only-child")  //单独的一个子元素
     
     var firstType =$("div > p:first-of-type") // 找到一个p标签，该标签是本层第一个p类型的标签
     
   });
   ```

9. 表单选择器

   ```javascript
   $(document).ready(function(){
      var inputs = $(":input"); //找到所有的隐藏input标签
     // 匹配<input> <textarea> <select> <button> 等可以输入的标签
     
     var texts = $(":text")  //匹配 <input type="text"> 标签
     
     // 同理还有
     // :password   :radio  : checkbox  :image    : reset  :button  :file
     
     // 表单元素还有  :enable 和 :disable 属性
   })
   ```

   