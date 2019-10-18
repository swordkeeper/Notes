# CSS 背景和列表

### CSS背景设置

##### background

- ``background-color`` 颜色 | transparent
  - ``transparent``是默认值，它等同于``rgba(0,0,0,0)``，即全黑的``rgb(0,0,0)``，在加一个透明度参数（参数值为0）。该参数可以忽略用户对浏览器的配置，使得背景总能显示成为设计者的样式。
  - 背景区域只囊括``内容元素``，``padding``，``border``，但是不包括外边框``margin``

- ``background-image``背景图片，值为 URL | none
  - 默认的，背景图像占据``左上角``，并在``水平和垂直方向重复``

- 同时设置背景图片和背景颜色，``背景图片``会覆盖掉``背景颜色``
- ``background-repeat``，处理背景重复的问题，值为 repeat | no-repeat | repeat-x | repeat-y
- ``background-attachment``，背景图片的显示方式，值为 scroll | fixed
  - ``scroll``为默认值，表示图片会随滚动条 滚动
  - ``fix``表示图片随页面移动
- ``background-position``，背景图片的起始位置，值为 top | left | bottom | right | center | 百分比 | 数值 
  - ``数值`` (300px, 20px) , 表示300px水平偏移，20px垂直偏移，只写一个参数，另一个参数默认为center
  - ``关键字``，设置的关键字是相对于整个网页的center | right.. 而不是相对于当前容器的
- ``background`` 缩写形式 ，该缩写没有属性先后顺序的要求

### CSS列表

##### 列表

- ``list-style-type``，列表种类类型，值为 none | 关键字 
  - 有序列表：none | decimal | lower-roman | upper-roman | lower-alpha | upper-alpha
  - 无序列表：none | circle | disc | square 
- ``list-style-image``，给列表添加小图标，值 URL | none 
- ``list-style-position``，值 inside | outside
  - Outside 默认值，表示图标在文本之外（尤其是对一个list里面有多行文本的情况下）
  - inside，表示图标会被文本环绕（多行文本就会环绕图标）
- ``list-style``，缩写形式，没有属性顺序限制，image 属性会``覆盖``掉 type属性