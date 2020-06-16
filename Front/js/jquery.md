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
     
     // 表单元素还有  :enable 和 :disable ， :checked  :selected
   })
   ```

10. 查找和过滤

    ```html
    <aside>
    	<details>
          <details></details>
          <details></details>
      </details>
      <details></details>
    </aside>
    ```

    

    选择元素

    ```javascript
    // find(expr | obj | element)   查找某一个元素
    
    $(document).ready(function(){
      	var details = $("aside").find("details"); // 在aside下面的所有details标签都被找到
         var details1 = $("aside").children("details"); //只找到aside标签的直接后继details标签
    	  var asides =  $("details").parent();  // 找到直接父节点
      
      	var nex = details.next(); // 找到本元素的后一个元素 , 即下一个兄弟
         details.prev();   // 即上一个兄弟
      
      
       // eq（index|-index） 可以为负数
       details.eq(2); // 从选中的 details 元素列表中选出第二个返回 
      details.siblings() // 找到同层的所有其他元素
    });
    
    ```

11. filter 选择器

    ```javascript
    $("document").ready(function(){
      	 var allDIV = $("div");
      	 var python = allDIV.filter(".python");  //从已有的集合中过滤出 包含.python类的 子元素
    });
    
    // filter( expr | obj | element | fn ) 
    ```

    

## 事件

1. 鼠标事件

    ```javascript
   // click([data,function]) 单击事件
   $("a").clcik(function(){
     console.log($(this))   //返回单机的对象
   })
   // dblclick([data,function])  双击事件
   
   // mousedown 、 mouseup ，鼠标按下和松开事件
   ```

   - `` click`` 单机事件
   - ``dblclick`` 双击事件
   - ``mousedown``鼠标按下
   - ``mouseup``  鼠标松开
   - ``mouseenter``  鼠标进入
   - ``mouseleave``  鼠标松开
   - ``mouseover``，``mouseout``，鼠标进入和退出***本元素或者其子元素时候触发***
   - ``hover`` 鼠标接触，即等价于mouseenter+mouseleave。``hover(function1,function2) 可接受两个函数，分别达标进入和退出时的两种效果``
   - ``mousemove([data], function)``，鼠标移动事件
   - ``scroll([data], function)``，返回滚轮事件，即当滚动条发生变化时，触发

2. 键盘事件

   - ``keydown([data],function)``，当键盘或者按钮被按下时候触发。

     ```javascript
     $("div").keydown(function(event){   //当事件发生时候会自动生成一个event对象，用来记录事件本身信息
       console.log(event.key); //返回被按下的key
       console.log(event.keycode) ; // 返回被按下的key的ASCII编码
     })
     ```

   - ``keyup([data],function)``，当按键松开时触发

   - ``keypress([data],function)``，当按键按下时触发，支持老版本浏览器。但是他不支持输入法输入，也不支持``backspace``、``enter``、``shift``等键

3. 其他事件

   - ``ready(function)``方法，等待页面加载完成

   - ``resize([data],function)``，当页面大小变化时执行

     ```javascript
     $("window").resize(function(){
       //...
     })
     ```

   - ``focus([data],function)``、``blur([data],function)``，当元素获得和失去焦点时触发

   - ``change([data], function)``，元素的内容发生改变时触发。

   - ``select([data], function)``，当textarea 或input 元素中的文本被选中时触发。

   - ``submit([data], function)`` ， 提交表单时触发

     ```javascript
     $("input[type=button]").click(function(){  //选中提交按钮，当被点击时候触发
       $("form").submit();   // 表单提交
     })
     
     
     // 阻止表单提交
     $("input[type=button]").click(function(){  //选中提交按钮，当被点击时候触发
       $("form").submit(function(){
         		return false; //阻止表单提交
       });  
     })
     ```

     

4. 事件绑定与取消

   - ``on(events,[selector],[data],fn)`` 在选择元素上绑定一个或多个事件处理函数

     ```javascript
     $("a").on("mouseenter",function(){  //事件a绑定上mouseenter事件
        //......
     })
     $("document").on("mouseenter","div",function(event){
       //.. 选择器为div，即绑定所有的div上 mouseenter事件
       event.stopPropogation() // 阻止事件冒泡
     })
     
     $("document").on({
       mouseenter:function(event){},    //绑定多个事件。 on绑定事件的好处是，可以支持动态生成的标签
       keydown:function(event){}        // 如createElement生成的标签
     })
     ```

   - ``off(events,[selector],[fn])``，移除一个事件上的绑定函数

   - ``one(events,[selector],[fn])``，即绑定后，只执行一次就解绑定。



## 动画操作

1. ``animate()``，动画函数

   ```javascript
   var div = $("div");
   div.stop().animate({opacity:0.25,     // .stop()表示 结束正在执行的动画，然后在执行新的动画animate
               width:"256px",
               height:"256px"},  3000)  // 使得透明度高度宽度在 3秒内发生变化
   				.delay(2000)     //延迟3000之后再执行 下一个动画
               .animate({width:"50%"});
   ```

2. ``show（）/hide（）``,显示或隐藏元素。``toggle()``会同时显示与隐藏一个函数

   ```javascript
   $("div").hide(3000).delay(2000).show(); //3秒内缓慢隐藏div元素。然后延迟2秒再显示出来
   ```

   show(speed, easing, function)，其中可接受三个参数，分别代表出现的速度，方式，以及出现完毕执行的方法

   - Speed:   Slow | normal | fast | 毫秒数
   - Easing: swing | linear
   - Fn:  回调函数
   - 这三个参数，同理下面两种方法👇

3. ``fadeIn()/fadeOut()``，淡入淡出元素，合起来可以用``fadeToggle()``。 

4. ``slideDown()/slideUp()``，划入划出，合起来可以用``slideToggle()``

5. 设置定时器

   - ``setTimeOut()``，设置计时器

     ```javascript
     $("div").setTimeOut(function(){},1000); // setTimeOut值执行一次，在1秒钟以后执行function
     
     var timer = setTimeOut(function(),2000);  //设置一个计时器，并清除
     clearTimeOut(timer)
     ```

   - ``setInterval()``，设置定时器

     ```javascript
     $("div").setInterval(function(){} ,  1000); //每隔1秒执行一次
     
     var intervaler = setInterval(function(){},2000) //设置一个定时器，并清除
     clearInterval(intervaler)
     ```

     