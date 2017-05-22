MVC 设计模式以及一个简单项目

具有用户注册、登陆、留言的基本功能

文件目录树
<pre>
mvc
│  message.txt
│  models.js
│  request.js
│  routes.js
│  server.js
│  utils.js
│
├─static
│      doge.gif
└─templates
        index.html
        login.html
        message.html
        register.html
</pre>

MVC 设计模式（一个经典的后端设计模式）
* Model       数据
* View        显示
* Controller  控制器


static 目录中存储了图片

templates 目录中存储了 html 文件

models.js 是数据存储的代码

request.js 是封装了 Request 类的代码, 主要用来保存请求信息

routes.js 是服务器能处理的 path(路由) 和 路由处理函数

server.js 是扩展的服务器代码

utils.js 包含了 log 函数(辅助类的函数)

server.js 的思路结构
<pre>
配置 host 和端口
监听请求
接收请求
    解析请求信息
        raw
        method
        path
        query
        body
    保存请求
        临时保存, 用完就丢
处理请求
    获取路由对象
        path 和响应函数的映射对象
    根据请求的 path 和对象处理请求并获得返回页面
        routes
            index
                返回页面
            login
                处理 post 请求
                    对比 post 数据和用户数据
                    返回登录结果
                返回页面
            register
                处理 post 请求
                    对比 post 数据和注册规则
                    保存合法的注册信息
                        保存到 user.txt
                    返回注册结果
                返回页面
            message
                处理 post 请求
                    将 post 的数据加入到 messageList
                返回页面
                    包含留言列表
            静态资源(图片)
                根据 query 的内容返回对应的资源
    返回响应内容
发送响应内容
关闭请求连接
</pre>

表单登陆 index.html
<body>
    <h1>登录</h1>
    <form action="/login" method="get">
        <input type="text" name="username" placeholder="username">
        <br>
        <input type="text" name="password" placeholder="password">
        <br>
        <button type="submit">登录</button>
    </form>
    <p>{{result}}</p>
</body>
</html>
