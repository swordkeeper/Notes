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

