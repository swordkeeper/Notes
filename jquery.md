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
   })
   ```

   