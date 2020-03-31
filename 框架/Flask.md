# Flask

一个简单的**web框架**



###  一个简单的Flask程序

```python
from flask import Flask   # 导入flask包下的Flask类

app = Flask(__name__)   # 创建一个Flask对象，并命名为__name__（主函数名）

@app.route("/")   # 装饰器配置解析路由
def index():
  """定义视图函数，url解析由上面的app完成"""
	print("This is a classic sentence")   # 服务器端打印信息
  	return "Hello World!"   # 服务器组织渲染页面，并返回给前端

if __name__ == "__main__":
    app.run() # 调用run方法，来运行起服务器以及框架
```



### Flask创建时参数

- ``import_name``，创建程序名
- ``static_url_path``，静态文件的url
- ``static_folder``，静态文件的路径
- ``template_folder``，模版文件的路径
- ``root_path``，根目录位置



### 环境变量配置

- 方法1：

    在项目主目录中创建``config.cfg``文件

    在文件中写入：

    ```
    FLASK_DEBUG = 1
    FLASK_APP = 'python'
    # 等环境变量
    ```

    在创建``Flask``对象app之后调用，``app.config.from_pyfile("config.cfg")``。这样环境变量就被加载入app对象中。

    app对象中的``config``属性是一个字典，用来保存这些环境变量参数。因而可以使用如下方式，来获取环境变量值。

    ``app.config["FLASK_DEBUG"]``

- 方法2：

    调用类属性方法

    ```python
    class Config():
        FLASK_DEBUG = 1
        FLASK_APP = 'python'   # 定义两个类属性
    app.config.from_object(Config)   # 调用类名加载环境变量
    ```

    

### 配置app运行地址

```python
if __name__ == "__main__":
    app.run(host="192.168.17.70", port=5000)	# 配置环境运行在192.168.17.70
    app.run(host="0.0.0.0", port=5000)	# 全网都可以访问方式运行
```



### 路由解析

- 查看路由，``app.url_map``，返回结果如下

    ```bash
    Map([<Rule '/' (HEAD, OPTIONS, GET) -> hello_world>,
     <Rule '/static/<filename>' (HEAD, OPTIONS, GET) -> static>])
    ```

    结果显示，该返回对象为一个map列表，每一条数据都是一个映射

- 路由请求方式设置，``POST`` or ``GET``

    ```python
    # 在定义装饰器时，通过methods指定
    @app.route("/index", methods=["POST", "GET"])  # 列表，包含了get和post方式
    def index():
        print("Hello World")
    ```

- 设置不同请求方式，调用不同的视图

    ```python
    # 方法不同，解析不同，但是url相同
    @app.route("/hello", methods=["POST"])
    def hello1():
        return "Hello1"
    
    @app.route("/hello", methods=["GET"])
    def hello2():
        return "Hello2"
    ```

- 设置不同url，实现多url解析到同一个视图

    ```python
    @app.route("/index1")
    @app.route("/index2")
    def index():
        return "Hello World!"
    ```

- 页面重定向

    ```python
    from flask import redirect
    
    @app.route("/login")
    def login():
        return redirect("/loginpage")  # 重定向
    ```

- 反向解析

    反向解析包括两步：1. 给某个url_mapping起别名；2. 使用别名调用反向解析函数获取真正的动态url

    flask中的反向解析，默认使用url所对应的视图函数名，来作为url_mapping 的别名

    ```python
    # 在Django 中，有专门的三元组来定义反向解析的对应关系。
    # urls = [
    #     url(r"/index", index, name="index_view")
    #]
    # 使用时，可以直接调用reverse_url("index_view") 反向解析函数就可以得到真正url
    
    from flask import url_for  # url_for 即flask中的反向解析函数
    
    @app.route("/index")
    	def index_view():
            return ""
    
    @app.route("/login")
    def login():
        url = url_for("index_view")  # 调用上面视图函数名，那么通过映射就反向解析出 "/index"
        return redirect(url)   # 跳转到"/index"
    ```
    
- URL中接收参数

    ```python
    @app.route("/goods/<int:goods_id>")   # 用<>包裹一个映射，类型：自定义变量名
    def goods_details(goods_id):   # 函数定义时，则需要接受该自定义变量
        return goods_id 
    # url 输入  127.0.0.1:5000/goods/123
    # 返回 123页面
    ```

    flask中已经定义好了几种转化器，包括``int``、``float``、``path``。

    也可以不添加转换器类型，即字符串类型（不包括/），例如``"/goods/<goods_id>"``

- URL中定义万能转换器

    需要使用``werkzeug``中的类转换器来实现

    ```python
    # 定义手机号的万能转换器
    
    # 使用 类转换器，做的到这点
    from werkzeug.routing import BaseConverter
    
    # 自定义类转换器
    class MyRegulation(BaseConverter):
        def __init__(self, url_map, regex):
            super(MyConverter, self).__init__(url_map)
            self.regex = regex
    
    # 将自定义转换器注册，key-value
    app.url_map.converters["re"] = MyRegulation
    
    # 使用转换器
    @app.route("/send/<re(r'1[3-7]\d{9}'):mobile>/purchase") #转换器的使用可以接受一个参数，该参数最终会专递到类中的 self.regex
    def user_purchase(mobile):
        return mobile
    
    # ------ 使用步骤 ------
    # 首先定义一个类，继承父类BaseConverter。并重写regex属性。该属性在被解析时候，当成正则式使用
    # 然后将定义好的转换器，添加到url_map.converts字典中进行注册。
    # 设置url路由时，使用该字典的key。werkzeug自动查找字典映射，找到自定义转换器的类。
    # 使用该字典key时可以带一个参数。即自定义转换器规则，该规则最终就传递到了MyRegulation下的self.regex。此例为 r'1[3-7]\d{9}'
    # 系统通过该正则，解析url中的某字段
    # 传入不同规则，就定义了不同的MyRegulation转换器类。如传入,
    # "/send/<re(r'\w{1:15}@\w{:5}.com'):email>/purchase" 该规则匹配url中的邮箱
    ```

    转换器父类``BaseConverter``，包含两个重要的方法``to_python``和``to_url``方法
    
    ``to_python``方法
    
    ```python
    # 自定义类转换器
    class MyRegulation(BaseConverter):
        def __init__(self, url_map, regex):
            super(MyConverter, self).__init__(url_map)
            self.regex = regex
    # ----------------------------------------------------------
    # 	to_python 方法是父类就有的方法，在此重写该方法
    #   当你访问url "/send/1313224141/purchase" 后，
    #   参数 1313224141 在正则验证通过之后，会被传递到该方法的value中。该方法return的值会被传递到视图函数相应的参数处。
        def to_python(self, value):   
            return "你的手机号是：" + value 
    
    ```
    
    ``to_url``方法
    
    ```python
    # 自定义类转换器
    class MyRegulation(BaseConverter):
        def __init__(self, url_map, regex):
            super(MyConverter, self).__init__(url_map)
            self.regex = regex
    # ---------------------------------------------------------- 
    # 该方法用于反向解析和动态生成用户url
    # 在反向解析中，我们填写的url_for参数是一个url映射后的视图名。通过视图函数名，我们就可以找到对应的url。
    # 但是url中若有这种转换器参数怎么办，这部分参数必须被替换成实际的值，才能确定真正的url。否则反向解析
    # 就得不到一个真正的地址。这也就是to_url函数的作用
    
    # 在反向解析中我们可以传入参数。例如 url_for("user_purchase", mobile="1234567")。
    # 这样参数 mobile="1234567"就会被当成字典传递给 该函数to_url
    # to_url 函数的返回值，最终替换url中参数部分
    	def to_url(self, value):
            return 
    ```
    
    综上，自定义解析器器，1.regex正则方法。2. to_python方法：将url参数提取并修改，然后传递给视图函数。3. to_python方法：动态生成url中的参数部分。

