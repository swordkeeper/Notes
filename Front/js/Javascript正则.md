# Javascript正则表达式

Javascript 正则表达式只能操作``字符串``。

### 正则表达式的创建

1. 字面量或直接量

   ```javascript
   var regu = /JS/;
   ```

2. 构造函数

   ```javascript
   var regu = new RegExp();
   ```

### 正则的简单例子

```javascript
var str = "i love js";
var pattern = /js/;    //字面量定义的正则表达式
pattern.test(str);    // 返回true， test函数测试一个字符串中是否有匹配

pattern.exec(str);    //返回 ["js"]，exec函数返回一个匹配的数组，即将第一个匹配到的字符串放入一个数组中返回

```



### 正则修饰符

1. ``i`` igonoreCase 忽略大小写

2. ``g`` global  全局匹配

3. ``m`` multiline  多行匹配

   ```javascript
   var pattern1 = /Js/i     //正则式是 /Js/  后面在加 i 表示忽略大小写的一个正则式
   var pattern2 = /Js/igm   //正则式是 /Js/ 修饰符为 igm 组合
   var pattern3 = new RegExp("Js","ig")  // 用构造函数的方法定义正则表达式，Js 为正则式，ig为修饰符
   ```



### 转义字符

转义字符用反斜杠``\``标注

```javascript
var pattern = /\//  //在/ /中间添加了一个反斜杠，并紧跟/ ，表示转义/
var pattern1 = /\n/  //转义字符 \n
var pattern2 = /\t/
var pattern3 = /\x0A/ // \x0A 表示 转义字符16进制的0A，即换行符\n
var pattern4 = /\u0009/  //匹配unicode编码的0009，即\t。多用于中文汉字
// 中文的 unicode编码范围为： u4e00 ~ u9fa5 ,所以匹配中文字符可以写：
var pattern5 = /[\u4e00-\u9fa5]/;
```



### 正则规则

1. ``[]``方括号，表示任意，即匹配方括号中的任意``一个``字符，并且字符串中写在前面的先匹配

   ```javascript
   var str = "javascript123";
   var pattern = /[vj]/     //匹配[vj]中的任意一个字符，匹配str从头开始扫描。j在第一个，并且在[]中，因而匹配j
   var pattern = /[1-9]/    // []中的 - ，表示匹配一个字段，即字符1～9，因而会匹配到1
   ```

2. ``^``取反，表示不在该列表里的字符

   ```javascript
   var str = "javascript123";
   var pattern = /[^vj]/   //表示不在该列表里的字符，即匹配的不能是v和j，因而会匹配到a
   ```

3. ``{}``表示量词，即匹配的个数

   ```javascript
   var pattern = /\d{3}/  //匹配三个数字
   ```

   ``{1,3}``表示匹配一个或三个，⚠️注意，此处1,3之间一定``不能有空格``，否则会出错

4. ``.``，匹配除了``\n``以外的所有字符

   ```javascript
   var str= "3.14159";
   var pattern = /./;    // 匹配到3
   var str1 = "\n";
   pattern.exec(str1);   //匹配不到，返回null
   ```

5. ``\w``，转义字符\w，表示，大写字符、小写字符、数字、下划线

   ```javascript
   var pattern = /\w/  //转义字符\w，匹配一个字符，包括大小写英文字符、数字以及下划线，等价于正则式 /[a-zA-Z0-9_]/  
   ```

   ``\W``，大写转义字符\W，表示``\w``的反面。等价于``/[^a-zA-Z0-9_]/  ``

6. ``\d``，表示数字等价于``/[0-9]/``，``\D``表示非数字

7. ``\s``，表示匹配一个``空格``或``制表符``，``\S``匹配除了空格或制表符之外的所有字符

   

   