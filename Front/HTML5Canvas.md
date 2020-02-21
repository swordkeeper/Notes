# Canvas

canvas 画布，用户在网络上绘画2d动画

作用如下

- 动画
- H5游戏
- 图形图表



使用``canvas``

```html
<body>
  <canvas id="myCanvas" canvas>
  	您的浏览器不支持 HTML5 的canvas 属性
  </canvas>
</body>
```

```javascript
var canvas = document.getElementById("myCanvas");
canvas.width=100;
canvas.height=200; //制定宽高。最好不要再css中给canvas设置样式，因为这只是对canvas进行缩放拉伸之后的大小
var ctx = canvas.getContext("2d");   //给画布区域制定一个背景信息，即绘制2d内容的对象
```

1. 画线

   ```javascript
   var ctx = canvas.getContext("2d");
   ctx.moveTo(0,0); //将画笔移动到(0，0)点
   ctx.lineTo(100,150); //画线从(0,0)到 (100，150)
   ctx.lineTo(120,330);  //画线从(100，150)到(120,330)
   ctx.strokeStyle="red" //画红线
   //此时内存中有了画线的内容，但是还没显示到屏幕上
   ctx.stroke(); //实际的画线，显示
   ctx.beginPath(); //清除内存中的路线，重新画线
   ctx.moveTo(120,330);
   ctx.lineTo(200,300);
   ```

2. 画圆

   ```javascript
   ctx.arc(300,300,50,0, 2*Math.PI, true);
   // 第一二个参数表示圆心，第三个参数表示半径，第4，5个参数表示圆所画的弧度，即0～2Pi，第五个参数true表示逆时针画线
   ctx.stroke();
   ```

3. 矩形

   ```javascript
   ctx.strokeRect(300,100,200,100);  //一二参数为矩形的左上角坐标，第三四个参数代表矩形宽高
   ```

4. 闭合曲线

   ```javascript
   ctx.closePath();  //始终将第一个位置和最后一个位置连接，使得图像闭合
   ```

5. 填充

   ```javascript
   ctx.moveTo(0,0);
   ctx.lineWidth=10;       //设置线的宽度
   ctx.lineTo(100,200);
   ctx.lineTo(112,304);
   ctx.fillStyle="red";
   ctx.fill();  //自动闭合，并且将闭合区域内容进行填充
   
   ctx.fillRect(100,200,100,100) //填充矩形
   
   ```

6. 平移

   ```javascript
   ctx.translate(0,100); //平移坐标系，即平移坐标系原点，从而整体图形也跟着移动
   ```

7. 旋转

   ```javascript
   ctx.rotate(Math.PI/4) ; //旋转45度
   ```

8. 缩放

   ```javascript
   ctx.scale(1,0.5);  //x轴缩放系数为1，y轴缩放为0.5
   ```

9. 显示文字

   ```javascript
   var str= "hello world"
   ctx.font="50px sans-serif"; //设置canvas字体样式
   ctx.textAlign="center";   //文字居中
   ctx.fillText(str,0,100);  //从(0,100)位置开始显示 填充式文字
   ctx.strokeText(str,0,200)  //描边式文字
   
   ctx.textBaseline = "top"   //水平位置
   ```

10. 显示图像

    ```javascript
    var img = new Image();
    img.src="logo.png";
    img.onload = function(){   //img加载完成执行
       ctx.drawImage(img,0,0); //在画布上添加一个图片
      ctx.drawImage(img, 100,100, 150,300); //同样的绘画图片，但是实在(100,100)位置开始绘制，并且绘制的图像缩放成150*300的大小
      ctx.drawImage(img, 0,0,40,40 ， 100,100,150,300);//如果有9个参数，其中(0,0,40,40)代表对要显示的图片进行截取
      //根据需求进行使用
    }
    ```

    填充图形

    ```javascript
    var pattern=ctx.createPattern(img,"repeat");   //将图片重复扩展
    ctx.fillStyle = pattern;
    ctx.fillRect(0,0,canvas.width,canvas.height);
    ```

    

11. 保存环境

    ```javascript
    ctx.save();  //保存一开始没有经过变动的环境，例如rotate，translate等
    ctx.translate;
    ctx.rotate;//....进行坐标scale等的变动
    ctx.stroke();// 绘制
    
    ctx.restore();  //恢复到save时的环境
    ```

12. 颜色渐变

    ```javascript
    //线性渐变
    var linearGradient = ctx.createLinearGradient(50,50,150,150);  //4个参数分别为起始点和终止点的x y 坐标
    linearGradient.addColorStop(0,"rgb(255,0,0)")    //在这条线段的0%位置，定义一个红色
    linearGradient.addColorStop(1,"rgb(0,0,255)")    //在线段末尾100%处，渐变为一个蓝色
    ctx.fillStyle=linearGradient;
    ctx.fillRect(0,0,200,200);     //将一个颜色渐变的类型 用于画一个区域矩形
    ```

    ```javascript
    //径向渐变
    var radialGradient = ctx.createRadialGradient(400,150,0,400,150,100) 
    //两组参数，第一组参数表示第一个圆的圆心坐标和半径
    // 第二个参数表示第二个圆的圆心坐标和半径
    radialGradient.addColorStop(0,"red");
    radialGradient.addColorStop(1,"blue");
    // 用法同样赋值给fillStyle 或 strokeStyle
    ```

13. 图形剪辑

    ```javascript
    ctx.arc(300,100,200,0,Math*PI*2,true);  //制定一个剪辑的区域
    ctx.clip();    //开始剪辑
    ctx.fillRect(100,100,200,200);    //之后所有绘画的区域都只显示被剪辑区域的部分。其余区域被屏蔽
    // 若要恢复剪辑前的剪辑区域 ，只能配合ctx.save() 和 ctx.restore()来使用
    ```

14. 图形阴影

    ```javascript
    ctx.shadowOffsetX=10;     //设置阴影的偏移
    ctx.shadowOffsetY=10;
    ctx.shadowColor="rgba(0,0,0,1)";   //设置阴影的颜色属性
    ctx.shadowBlur=5 ;         //设置模糊半径
    ctx.fillRect(100,100,100,100);  //画出矩形
    ```

15. 贝塞尔曲线

    ```javascript
    //贝赛尔曲线有三个点，第一个定点，2个游离点，第四个点为终点
    ctx.beginPath();
    ctx.moveTo(175,375); //第一个点
    ctx.bezierCurveTo(297,182,468,252,517,380) //贝塞尔曲线的依次四个点
    ctx.fill()
    
    ctx.beginPath();
    ctx.moveTo(175,375); //第一个点
    ctx.quadraticCurveTo(297,182,468,252) // 第二个点和终点
    ctx.fill();
    ```

    Ref:

    http://blogs.sitepointstatic.com/examples/tech/canvas-curves/quadratic-curve.html

    http://blogs.sitepointstatic.com/examples/tech/canvas-curves/bezier-curve.html

16. 清空画布

    ```javascript
    ctx.clearRect(0,0,canvas.width,canvas.height);  //清除整个画布矩形区域
    ```

17. 离屏canvas

    在一些情景，需要将复杂绘画出来的canvas作为背景添加到另一个canvas中。这样可以提高动画性能。

    因为这样背景就不用每次重新绘画

    ```javascript
    var offCanvas = document.getElementById("offCanvas").getContext("2d");
    /*.....
    绘制offCanvas
    */
    var canvas = document.getElementById("Canvas").getContext("2d");  //现在将offCanvas变为canvas的背景
    canvas.drawImage(offCanvas,  
                     0,0,offCanvas.width,offCanvas.heigth,
                     0,0,canvas.width,canvas.height); 
    //这就是个canvas引入图片的机制，只是此次引入的是一个canvas。引入参数第二行为引入对象的引入范围。
    //引入参数第三行为引入后所展现的位置。 方法类似于图像
    ```

    最后再将第二个offcanvas隐藏

    ```css
    #offCanvas{
      display:none;
    }
    ```

    