# 移动端Web

手机，平板电脑的Web开发



### 像素概念

1. 物理像素和逻辑像素，以及像素比
   - ``物理像素(physical pixel)`` 或者``设备像素dp(device pixel)`` 即设备的实际分辨率，像素显像点
   - ``css像素``或``逻辑像素(logical pixel)``或叫``设备独立像素dip(device independent pixel)``, 是实际开发中使用的逻辑像素
   - ``设备像素比dpr(device pixel ratio)`` = 设备像素 / css像素， 即代表一个css程序像素需要用几个实际物理像素来表示。 如果 dpr = 1，则代表一个 1 dip = 1dp， 如果dpr =2 ，则代表1 dip = 4 dp ,即 一个像素点由纵横各两个像素点组成。

2. 缩放

   缩放物理像素不变，只是dip所代表的dp发生了改变

   放大：一个dip需要跟多物理dp去表示

   缩小：一个物理dp可以表示更多逻辑dip

3. ppi 或 dpi

   每英寸物理像素点(pixels per inch) 或 每英寸物理像素点(dots per inch)

### 视口

视口，即视觉所观察到的设备大小。当浏览设备发生改变时，视口也会法身改变。从而会影响布局

- 配置视觉口

  ```html
  <!-- 视口设置 -->
  <meta name="viewport" content="width=375, height=200" >
  <!-- 或者 自适应 视口大小 -->
  <!-- 一般写成如下形式，分别表示，宽度width自适应屏幕宽度，initial-scale缩放比例1:1 -->
  <meta name="viewport" content="width=device-width, inital-scale=1.0" >
  
  
  <!-- user-scalable 属性表示是否允许用户缩放，一般不允许 -->
  <meta name="viewport" content="width=device-width, inital-scale=1.0"  user-scalable="yes">
  
  <!--   minimum-scale/maximum-scale 表示用户可以缩放的最大范围 -->
  <meta name="viewport" content="width=device-width, inital-scale=1.0"  minimum-scale=1 maximum-scale=2>
  ```

- 获取视口大小

  ```javascript
  var width = window.innerWidth;
  var width = document.documentElement.clientWidth;
  var width = document.documentElement.getBoundingClientRect().width;
  
  // 兼容写法
  var width = window.innerWidth || document.documentElement.clientWidth || document.documentElement.getBoundingClientRect().width;
  
  // 获得dpr
  var dpr = window.devicePixelRate
  ```

### box-sizing

Box-sizing：属性表示在盒子模型中，width和height到底描述的是什么

- ``box-sizing:content-box``，表示width和height描述的是内容content的大小

- ``box-sizing:border-box``，表示width和height描述的是包括border在内的所有东西的总宽高。通常移动端开发用border-box

  ```css
  *{
    margin:0;
    padding:0;
    border:0;
    box-sizing:border-box;
  }
  ```

  







### 移动端事件

1. ``touchstart``，只用户触摸屏幕某处

   ```javascript
   oneObj.addEventListener("touchstart",function(e){
     	//回调函数，触发touchstart事件后执行
        var touchEvent = e || event //兼容写法
        var position = {
           x: touchEvent.touches[0].clientX,    //touchEvent.touches[0] 表示多点控的第一个触点。
           y: touchEvent.touches[0].clientY
        }
        var rect = touchEvent.currentTarget.getBoundingClientRect() // 取得当前触点所在的矩形控件对象
        
   },false);
   ```

   

2. ``touchmove``，用户操作持续在屏幕上移动

3. ``touchend``，用户操作离开屏幕