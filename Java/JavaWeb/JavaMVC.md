# MVC 架构

MVC模式将一个web程序分为3个部分

- M（model）模型
- V（view）视图
- C（controller）控制器





## MVC具体内容

### Controller

1. 获取用户数据
2. 调用model
3. 将结果转发给view
4. 可以用``servlet``技术实现



### Model

业务逻辑，与database交互。实现数据的CRUD

通常情况下使用``JavaBean`` 来打包辅助完成业务



### View

视图是用来展示数据的。将控制器传递过来的数据与模板结合生成最终页面

可以通过``jsp``技术实现 







