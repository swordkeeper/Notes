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
    def goods_details(goods_id):   # 定义视图函数时，则需要接受该自定义变量
        return goods_id 
    # url 输入  127.0.0.1:5000/goods/123
    # 返回 123页面
    ```

    flask中已经定义好了几种转化器，包括``int``、``float``、``path``。

    也可以不添加转换器类型，即默认为字符串类型（不包括/），例如``"/goods/<goods_id>"``

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
    # 使用该字典key调用时可以带一个参数。该参数传递给__init__的regex参数，并最终就传递到了MyRegulation下的self.regex。此例为 r'1[3-7]\d{9}'
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
    #   此处对应的视图获得到的传入参数即为  ‘你的手机号是：1313224141’
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
            return value   # 返回该字典，字典中包含 mobile:"1234567"键值对
    ```
    
    综上，自定义解析器器，1.regex正则方法。2. to_python方法：将url参数提取并修改，然后传递给视图函数。3. to_python方法：动态生成url中的参数部分。

### 视图

- 视图 ``request``对象

     从前端获取参数的 request对象，是一个全局对象

    ```python
    from flask import request
    
    @app.route("/index")
    def index():
        form_data = request.form.get("name")   # 读取表单数据，form返回一个字典
        form_data1 = request.form.getlist("name") # 拿到同名的所有数据
        other_data = request.data # 接收请求体中的其他类型数据（非form-data、非x-www-form-urlencoded)
        url_data = request.args # 接受url中所带的参数，返回一个字典
        request_header = request.headers # 请求头，返回一个字典
        request_method = request.method # 返回请求方式
        request_url = request.url # URL地址
        request_file = request.files["file1"]  # 请求的上传的文件。返回字典。字典值为文件对象
        request_file.save("~/files/")  # 存储文件
        
    ```

- 视图响应

    - 使用 ``元组``返回自定义的响应信息

        ```python
        @app.route("/index")
        def index():
            return "index page", 400, [("name", "monkey king"), ("age","500year")]
            # 返回的响应信息，是一个元组
            # 第一位：返回response 的body部分
            # 第二位：状态码。该位置也可以是一个自定义状态码，可以添加状态码解释信息 "666 myStatusCodeDesc"
            # 第三位：二元组列表。代表键值对。该健值对，会被当成response header一部分
            # 第三位：也可以是一个字典{"name":"monkey king","age":"500year"}
        ```

    - 使用``make_response``构造响应信息

        ```python
        from flask import make_response
        
        @app.route("/index")
        def index():
            response = make_response("index page")
            resposne.status = "666 myStatusCodeDesc"  # 也可以是 404
            response.headers["name"] = "monkey king"
            return response
        ```

    -  使用``jsonify``返回 json数据

        ```python
        from flask import jsonify
        
        @app.route("/index")
        def index():
            data = {"name":"monkey king", "age":500}
            return jsonify(data)  # jsonify会自动把响应头设置为Content-Type: application/json
        
        ```
        
    - g变量（全局上下文）
    
        每次进入具体的视图函数时，g对象会被flask清空
    
        ```python
        from flask import g  # g变量是用于在一次请求的函数之间，传递参数的一个变量
        
        @app.route("index") 
        def index():  # 进入具体的视图函数，g对象被清空
            g.username='hello'
            g.age = "nonono"
            sayHello(g)   # 调用其他函数
            # g变量可以用来，在一次请求中，调用的不同函数之间，携带参数
            return "success" 
        
        def sayHello(g):
            print(g.username)
        ```
    
    - 请求钩子
    
        - 在处理第一个请求之前，运行``before_first_request``
    
            ```python
            @app.route("/index")
            def index():
                return "index page"
            
            @app.before_first_request # 装饰器加载后，handle_before_first_request函数就会在服务器第一次接收到请求时被执行
            def handle_before_first_request():
                print("执行第一次接收请求的预处理内容")
            ```
    
        - 在每次请求之前处理，运行``before_request``
    
            ```python
            @app.before_request
            def handle_before_request():
                pass
            ```
    
        - 在处理请求之后，若没有异常，运行``after_request``
    
            ```python
            @app.after_request
            def handle_after_request(response): # 后处理函数需要接受一个参数，用来得到视图函数返回对象
                return response # 而且还必须有返回值，该返回值才是最终用户的到的页面
            ```
    
        - 在处理请求之后，即使有异常，运行``teardown_request``，同理函数需要接受一个response对象。需要在非debug模式下运行才有效

### 模版

- HTML 文件

    ```html
    <!-- index.html -->
    <body>
        {{ name }}   <!-- html模版文件，和Django一样 -->
        {{ age  }}
        {{ for each in keys}}
        {{ endfor }}
    </body>
    ```

- Flask中的渲染 ``render``

    ```python
    from flask import render_template
    
    @app.route("/index")
    def index():
        return render_template("index.html", name="python", age=18) # 第二个参数传递的不像Django是个字典，而是一个字典的拆包
    ```

- 过滤器

    ```html
    <body>
        {{ "hello" | upper}}
        {{ "hello" | trim | lower | captialize}} <!-- 过滤器支持链式操作 -->
    </body>
    ```

    过滤器包括：``safe 禁止转义``、``capitalize``、``lower``、``upper``、``title值中每个单子首字母大写``等

- 自定义过滤器

    ```html
    <body>
        {{list | step2}}
    </body>
    ```

    ```python
    def list_step_2(li):
        '''过滤器：隔一个取一个'''
        return li[::2]  # 利用python的切割，每次步长为2
    app.add_template_filter(list_step_2, "step2") #将函数注册到模版过滤器中，并起一个名字叫 step2
    ```

    第二种定义过滤器方式（装饰器）

    ```python
    @app.template_filter("li3")
    def list_step_3(li):
        '''过滤器：隔2个取一个'''
        return li[::3]  # 利用python的切割，每次步长为3
    ```

- 模版继承

    ```html
    <!-- base.html 父模版-->
    {% block top%}
    顶部内容
    {% endblock top}
    {% block bottom%}
    底部内容
    {% endblock bottom%}
    ```

    ```html
    <!-- extented.html 子模版-->
    {% extends "base.html"%}
    {% block content%}
    内容
    {% endblock content %}
    ```

- 默认模版属性

    ``config``、``request下的内容``、``url_for``、``get_flashed_messages``等都可以作为``{{  url_for('index')  }}``的直接使用值，没必要显示传递参数

- 闪现信息``flash``

    闪现信息只执行一次，自动把闪现信息存储到session

    ```html
    {%for msg in get_flashed_messages()%}
    {% endfor %}
    ```

    ```python
    if flag == True:
        flash("hello1. This is flash1")
        flash("hello2. This is flash2")   # 添加闪现信息，相当于缓存，在{{}}中读取了，信息就会被清空。除非再次添加
    ```

- 模型类

    参考下面``Flask_WTF``插件



### 渲染模版宏 macro

macro宏的作用类似于函数，提高渲染封装、重复利用效率

```html
<body>
    {% macro input() %}  <!--宏定义 -->
    <input type="text">
    {% endmacro %}
    <br/>
    {{ input() }}  <!-- 宏使用-->
    
    {%macro input1(type="text", name)%}  <!-- 带参数的宏， type有默认值 -->
    <input type={{type}} name={{name}}>
    {% endmacro%}
    {{input1("password")}}   <!-- 使用带参数的宏 -->
    
    <!-- 调用其他html文件中定义的宏 -->
    {% import "macro.html" as macro %}  <!-- 导入 -->
    {{macro.input1("password", "mypasswd")}}
</body>
```



### Web 存储

- Cookie 的操作。

    Cookie 只能由服务器端向客户端设置，保存在客户端
    
    ```python
    from flask import make_response, request
    
    # 设置cookie
    @app.route("/set_cookie")
    def set_cookie():
        resp = make_response("set cookie success") # 页面显示内容
        resp.set_cookie("cookie1", "hello")  # 设置了三个cookie
        resp.set_cookie("cookie2", "world")  # 默认有效期为 临时 cookie，浏览器关闭就失效
        resp.set_cookie("cookie3", "welcome", 3600) # 设置有效期3600秒
        return resp
    # 获取 cookie
    @app.route("/get_cookie")
    def get_cookie():
        cookie1 = request.cookies.get("cookie1") # request.cookies 获得的是一个cookie字典
      
    # 主动删除cookie(标记删除，将cookie的有效期设置成当前时刻，从而失效)
    @app.route("delete_cookie")
    def delete_cookie():   # 因为cookie是存储在客户端的，要删除cookie就要由服务器返回响应，在响应中携带删除指令
        resp = make_response("delete success")
        resp.delete_cookie("cookie1")
        return resp
    
    ```
    
- Session 存储

    保存在服务器端。flask默认把session保存在cookie中，但是实际应该是session保存在服务器后端数据库中

    ```python
    from flask import session
    # 设置session需要对app进行配置，来加密session的传输。因为flask默认把session保存在cookie中（加密形式）
    app.config["SECRET_KEY"] = "sdciuasdvbl123n;o!@" # 设置密钥，可以指定为任意字符串 
    
    # 设置session
    @app.route("/set_session")
    def set_session():
        session["cookie4"] = "python"
        session["cookie5"] = "today"
        return "success"
    
    # 获取session
    @app.route("index")
    def index():
        cookie4 = session.get("cookie4")
        return "cookie4 has been set"
    ```

    

### 错误处理

- 错误处理``abort``函数，终止本次请求的视图运行，并直接返回给页面相应的信息。类似于直接发起一个``raise``，只是abort不产生异常码。

    ```python
    from flask import abort, Response
    @app.route("/login")
    def login():
        if name != "bob" and pwd != "admin": 
            # 如果密码不正确，调用abort，让页面直接返回，并终止视图函数继续执行
            # abort 可以接收一个状态码。
            # 或者也可以接收一个Response对象，作为响应体
            abort(404) 
            # abort(Response("login fail"))
        return "login success"
    ```

- 自定义错误信息返回页面

    ```python
    # 自定义处理 404 错误信息的函数
    
    @app.errorhandler(404)    # 添加装饰器，在装饰器的参数中写入错误信息编码
    def handle_404_error(err):  # 该函数需要接收一个参数，来作为抛出错误的具体描述
        # 这个函数的返回值，会是最后用户看到的404错误页面
        return u"出现404错误信息，错误信息为：%s" %err
    ```





---



## 扩展

### 管理类Manager

Flask_script 工具

```python
# flask_script.py 文件
from flask_script import Manager   # 启动命令的管理类
app = Flask(__name__)
manager = Manager(app)

if __name__ == "__main__":
    manager.run()  # 通过管理类启动程序
```

```bash
python flask_script.py runserver 启动命令
```



### 表单扩展工具

 Flask_WTF 工具

```python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField
from wtforms.validators import DataRequired, EqualTo

# 定义表单模型类
class RegisterForm(FlaskForm):
    user_name = StringField(label="username", validators=[DataRequired("用户名不能为空")]) 
    # 表示为字符类型，参数包括用户名，和验证器。DataRequired 验证器表示该字段不能为空，可以接受一个参数，表示提示信息
    password1 = PasswordField(label="password1", validatros=[DataRequired("密码不能为空")])
    password2 = PasswordField(label="password2", validatros=[DataRequired("确认密码不能为空"), EqualTo(password1, "两次密码不一致")])
    submit = SubmitField("label"="submit")
    
@app.route("/register")
def register():
    # 创建注册表单
    form = RegisterForm()
    return render_template("register.html", form = form) # 在渲染模版时，传递创建好的form对象
```

```html
<body>
    <form method="post">
         {{ form.user_name.label}}  <!--显示表单的标签 -->
         <p>{{form.user_name}}</p>  <!--显示表单框体 -->
         <p>{{form.user_name.errors}} </p> <!--显示出错时的提示信息-->
         <!-- password 同理 -->
         {{form.submit}}
    </form>
</body>
```

因为表单需要添加``csrf``验证，以及csrf在传递过程中需要混淆处理（secret_key)。所以还需要添加如下

```python
app.config["SECRET_KEY"] = "asviohn289@cE@#B*@BkbkcKBOsdKwK"
```

```html
<form>
    {{form.csrf_token}}
</form>、
```

因为注册页面可能提交到同一地址，一般情况下后端需要根据是``post``还是``get``请求方式来获得判断。

但是在``flask_wtf``中，模块会在创建form时候自动进行判断

```python
form = RegisterForm()  # 可以是get时候的创建表单，若是post提交请求，则该步骤会自动填充form的内容
if form.validate_on_submit(): # 判断是否满足所有的验证器。没有数据时，返回false
    # post 方式，有数据，且验证合格
    user_name = form.user_name.data # 验证合格，拿取form中字段数据
    
```



### 数据库

``flask_sqlalchemy`` 工具，将模型类转换成sql语句或者将数据库结果转换成模型类对象

数据库驱动 （用于与数据库交互、链接的程序）

- ``PyMySql``，建立flask与数据库之间的网络链接 （python3）

- ``MYSQL-Python python2``，同理用于python2

- 数据库配置

```python
# flask mysql 配置
# 通过URI格式配置flask。“mysql://用户名：密码@IP：port/数据库名
app.config["SQLALCHEMY_DATABASE_URI"] = "mysql://root:mysql@127.0.0.1:3306/db_1" 

app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = True # 表示若表结构发生变化，模型类也跟着被修改
app.config["SQLALCHEMY_ECHO"] = True # 查询时会显示模型类转换成的sql语句 
```

- 数据库使用样例

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

class Config(object):
    SQLALCHEMY_DATABASE_URI = "mysql://root:mysql@127.0.0.1:3306/db_1"
    app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = True
    app.config["SQLALCHEMY_ECHO"] = True
    
app.config.from_object(Config)

db = SQLAlchemy(app)  # 创建数据库对象

# 创建模型类
# db.Column 表示实际存在表中的一列

#######################
class Role(db.Model):
    __tablename__ = "tbl_roles"
    id = db.Column(db.Integer, primary_key=True)  #主键id，默认自增
    role_name = db.Column(db.String(64), unique=True)  # 64字节，唯一约束
    
    # 用于查询属于同一个role_id的所有user的类。类似于一个数据库view方法。用于反向查询所有User
    # backref 等于向User中添加了一列role，用于交叉查询时的反向解析。
    users = db.relationship("User", backref="role") 
    
    def __repr__(self):   # 类似于__str__方法。alchemy里面用该方法来表示
        return self.name
    
    
########################
class User(db.Model):  # 从db对象中继承其父类
    '''用户表'''
    __tablename__ = "tbl_users" # 指名数据库的表明
    id = db.Column(db.Integer, primary_key=True)  #主键id，默认自增
    name = db.Column(db.String(64), unique=True)  # 64字节，唯一约束
    role = db.Column(db.Integer, db.ForeignKey("tbl_roles.id") # 要用表名，不能是模型类的值
if __name__ == "__main__":
    db.drop_all() # 清除数据库里所有数据，危险，只可以在创建时使用。否则得跑路了
    db.create_all()
```

- 增删改查

    ```python
    # 增----------------------
    role1 = Role(name="admin")  # 创建一个模型类对象
    db.session.add(role1)   # 创建一个事务，并添加role1
    db.session.commit()     # 提交事务
    
    # 查------------------------
    li = Role.query.all() # 返回查询列表
    print(li[0].name)
    print(Role.query.get(2).name) # 返回查询对象id为2的角色的name
    User.query.filter_by(name='wang').all() # 返回所有名为wang的对象
    
    from sqlalchemy import or_  # 代表或的一个函数
    User.query.filter(  or_(User.name=="wang", User.email.endswith("163.com"))  )
    
    User.query.filter().offset().limit().order_by().all() # 链式
    # order_by(User.id.desc())
    # offset(2).limit(5)
    
    # 分组
    from sqlalchemy import func
    db.session.query(User.role_id, func.count(User.role_id)).group_by(User.role_id) #分组查询并统计
    
    
    # 改-------------------------
    user = User.query.get(1)
    user.name = "newName"
    db.session.add(user)
    db.session.commit()
    
    User.query.filter_by(name="wang").update({"name":"newName", "email":"6@22.com"}) # 第二种方式
    db.session.commit()
    
    # 删 -----------------------
    user = User.query.get(3)
    db.session.delete(user)
    db.session.commit()
    ```


### 数据库迁移

``flask-migrate``工具，可以帮助实现数据库的迁移。该工具依赖``flask-script``

```python
from flask import Flask
from flask_migrate import Migrate, MigrateCommand
from flask_script import Shell, Manager

app = Flask(__name__)
manager = Manager(app)
db = SQLAlchemy(app)
migrate = Migrate(app, db)  # 创建一个迁移对象
manager.add_command("db", MigrateCommand)  # 向manager这个flask-scrip对象中添加一个db命令 

# 模型类 A B。。。。

if __name__ == "__main__":
    manager.run()   # 启动迁移程序
```

```bash
python app.py db init.  # 执行该python文件，传入参数db 因为("db", MigrateCommand)进行了捆绑。然后执行初始化init操作
python app.py db migrate # 生成迁移文件
python app.py db upgrade # 执行迁移文件、修改数据库 
python app.py db history # 查看历史提交
python app.py db downgrade 状态码 # 回退版本
```





