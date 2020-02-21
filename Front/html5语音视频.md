# HTML5 语音视频

本部分主要内容是介绍，当今Web环境的多媒体开发。

包括：

- ``audio``和``vedio``的开发以及API的使用。
- 自定义播放器
- 第三方框架



### Video标签

``<video>``标签支持``mp4``、``webs``、``ogv``格式的视频文件播放。

- 基本使用方法：

  ```html
  <video src="url://" controls>
  <!-- controls属性代表显示控制面板 -->
  </video> 
  ```

  ```html
  <video>
  	<source src="url" type=>
  </video>
  ```

- 标签属性

  - ``src``
  - ``width``
  - ``height``
  - ``controls``：播放控件
  - ``autoplay``：自动播放
  - ``loop``：循环播放
  - ``poster``：视频封面，即没有播放时显示的图片，值为图片url
  - ``muted``

- 标签API事件

  - ``play``

  - ``pause``

  - ``duration``：返回当前视频长度

  - ``currentTime``：设置/返回当前视频播放时间

  - ``src``：设置/返回当前视频信息来源

  - ``volume``：设置/返回视频音量

  - ``controls``：设置视频是否显示控件

  - ``muted``：设置视频是否静音

  - ``networkState``：返回视频网络状态，返回码为0：未初始化，1：视频已经选取好资源，但是未使用网络，2：视频正在加载视频资源，3：未找到视频资源

  - ``currentSrc``：返回视频的当前URL

  - ``end`` ：返回视频播放是否结束

  - ``loop``：设置/返回是否循环

  - ``playbackRate``：设置/返回视频播放速度

  - ``readyState``：返回就绪状态，状态码，4：有缓存可以播放，3：有网络链接，没缓存，数据正在缓冲，1:有数据但是快不足以支持播放了，2:有数据，但是基本缓存要用完了，0:没有关于视频的信息

  - ``timeupdate``：可以用于监听播放状态

    ```javascript
    videoID.addEventListener("timeupdate",function(){
      // 即每次视频内容发生变动，就触发事件
    })
    ```

  - ``seeked``，当用户改变进度条**结束**时候，触发状态

    ```javascript
    videoNode.onseeked=function(){
      // 用户拖拽进度条结束后出发改函数
    }
    ```

  - ``seeking``：当用户**正在**拖动进度条时候出发，``.onseeking=function(){}``

  - ``volumechange``：音量更改时触发事件``.onvolumechange=function(){}``

  - ``requestFullscreen``：全屏时触发，该事件是最新版本的事件，需要写成``webkitRequestFullscreen()``或者``mozRequestFullScreen()``，而且必须放在一个其他事件中

  - ``load``：强制刷新，重新加载

  - ``canplay``：视频已经准备好播放时触发

- 浏览器兼容性

  - ``chrome``
    - Chrome下不支持autoplay属性，即autoplay属性在chrome下不起作用。但是如果同时设置了muted属性为true，那么chrome就会支持autoplay属性
    - 不支持play事件，除非放在onclick事件中
  - ``safari``
    - 不支持webs、ogv格式文件
  - ``IE``
    - IE8及以下版本是**不支持**video标签
    - 不支持webs、ogv格式文件
  - ``firefox``
  - ``opera``

### Audio标签

``<audio>``标签支持的格式有``mp3``、``wav``、``ogg`` 

- 使用方法

  ```html
  <audio controls>
  	<source src="url">
  </audio>
  ```

  ```javascript
  // 动态生成一个audio标签
  var myAudio = new Audio();
  myAudio.src="url";
  myAudio.play();
  ```

- 兼容性

  - mp3 所有浏览器都支持
  - ogg 除了safari以外都支持

- 属性

  ``src``、``controls``、``autoplay``、``loop``、``muted``

- 事件

  -  play
  - pause
  - duration
  - currentTime
  - src
  - volume
  - controls
  - muted
  - networkState 返回当前网络状态
  - currentSrc
  - ended
  - loop
  - playBackRate
  - readyState
  - timeupdate
  - seeked
  - seeking
  - volumechange
  - requestFullScreen
  - load
  - canplay





### VideoJs

``VideoJs``是一个专门用来处理前端视频的框架。

- 引入和使用

  ```html
  <link type="tex/css" href="video-js.min.css" rel="stylesheet">
  <body>
     <video id='myVideoId' class="video-js vjs-big-play-center" ></video>    <!--class 必须是video-js ，已经被指定好了-->
    <!-- vjs-big-play-center 类规定了按钮剧中显示 --> 
  </body>
  ```

- 属性

  - ``data-setup``，设置视频数据，可以传入一个json数据
  - ``preload``，有个预加载缓冲读条。
  - ``width``、``height``
  - ``poster``封面

- 函数

  ```javascript
  videojs("myVideoId").ready( //该库封装好了一些函数，比如videojs，它接受参数为video标签的id
  	function(){    //封装的ready表示 视频已经准备就绪类似于 canplay属性
      this.play(); 
    }
  )         
  
  ```

  - ready()，传入一个函数，在视频准备就绪时执行

  - currentTime()，返回当前时间

  - duration()，返回总时间

  - Buffered()，返回以缓冲的时间

  - volume()，返回音量

  - on()，添加事件监听

    ```javascript
    myVideo.on("ended",funciton(){
               console.log(1);
               })
    ```

    



### 媒体查询

 媒体查询，其实就是检测媒体的种类，用于根据设备的大小种类，来定义css样式

1. 媒体查询简单例子

   ```html
   <style>
     @media screen and (min-height:900px){   
       /*这里就调用了一个媒体查询，@media 表示媒体查询，screen 和 min-height分别代表两个条件，即查询媒体是屏幕，且最小高度为900px。 如果两个条件成立，则执行下面的样式代码。*/
       body{background:red;width:375px;}
     }
   </style>
   ```

2. 媒体查询的类型

   - ``all(default)``，所有设备
   - ``screen``，屏幕设备
   - ``print``，打印预览设备
   - ``speech``，阅读器设备

3. 媒体的条件

   ``and``、``,``、``not``，分别代表条件查询的，与或非。

4. 媒体表达式

   - ``max-width/min-width``，``max-height/min-height``，媒体的最大最小宽高
   - ``-webkit-device-pixel-ratio/-webkit-max-device-pixel-ratio/-webkit-min-device-pixel-ratio``，设备的像素比
   - ``orientation``: landscape横屏/portrait竖屏 

5. 媒体查询的操作

   1. 写断点，即屏幕样式变化的分界点，一般分割如下

      - ``xs``超小屏幕，width<576px，小屏手机
      - ``sm``小屏幕，576px~768px，大屏手机
      - ``md``，768px~992px，平板或是小屏显示器
      - ``lg``，992px~1200px，普通显示器
      - ``xl``，>1200px，大屏显示器 

      