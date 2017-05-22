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

表单登陆 login.html
<form action="/login" method="get">
    <input type="text" name="username" placeholder="username">
    <br>
    <input type="text" name="password" placeholder="password">
    <br>
    <button type="submit">登录</button>
</form>
<p>{{result}}</p>

留言板 message.html
<form action="/message" method="post">
        <input name="author">说
        <textarea name="content" rows="8" cols="40"></textarea>
        <button type="submit">POST 提交</button>
    </form>
<pre>{{messages}}</pre>

注册页面 register.html
<form action="/register" method="post">
        <input type="text" name="username" placeholder="username">
        <br>
        <input type="text" name="password" placeholder="password">
        <br>
        <button type="submit">注册</button>
    </form>
<p>{{result}}</p>

model.js 核心代码
<pre>
const save = (data, path) => {
    // 默认情况下使用 JSON.stringify 返回的是一行数据
    // 开发的时候不利于读, 所以格式化成缩进 2 个空格的形式
    const s = JSON.stringify(data, null, 2)
    fs.writeFileSync(path, s)
}

// 从文件中读取数据, 并且转成 JSON 形式(即 object 或者 array)
// path 是保存文件的路径
const load = (path) => {
    // 指定 encoding 参数
    const options = {
        encoding: 'utf8',
    }
    // 读取之前确保文件已经存在, 这样不会报错
    ensureExists(path)
    const s = fs.readFileSync(path, options)
    const data = JSON.parse(s)
    return data
}

class Model {
 Model.dbPath()
    static dbPath() {
        const classname = this.name.toLowerCase()
        // db 的文件名通过这样的方式与类名关联在一起
        const path = `${classname}.txt`
        return path
    }

    static all() {
        const path = this.dbPath()
        const models = load(path)
        return models.map(m => this.create(m))
    }

    static create(form={}) {
        const cls = this
        const instance = new cls(form)
        return instance
    }

    save() {
        const cls = this.constructor
        const models = cls.all()
        models.push(this)
        const path = cls.dbPath()
        save(models, path)
    }

    toString() {
        const classname = this.constructor.name
        console.log('debug classname', classname)
        const keys = Object.keys(this)
        const properties = keys.map((k) => {
            const v = this[k]
            const s = `${k}: (${v})`
            return s
        }).join('\n  ')
        const s = `< ${classname}: \n  ${properties}\n>\n`
        return s
    }
}

class User extends Model {
    constructor(form={}) {
        // 继承的时候, 要先调用 super 方法, 才可以使用 this
        super()
        // User 类定义两个属性
        this.username = form.username || ''
        this.password = form.password || ''
    }
    // 校验登录的逻辑
    validateLogin() {
        log(this, this.username, this.password)
        return this.username === 'gua' && this.password === '123'

        // const us = User.all()
        // for (let i = 0; i < us.length; i++) {
        //     const user = us[i]
        //     if (u.username === user.username && u.password === user.password) {
        //
        //     }
        // }
    }

    // 校验注册的逻辑
    validateRegister() {
        return this.username.length > 2 && this.password.length > 2
    }
}

class Message extends Model {
    constructor(form={}) {
        super()
        this.author = form.author || ''
        this.content = form.content || ''
        this.extra = 'extra message'
    }
}

// 这次暴露的是一个包含两个 model 的对象
module.exports = {
    User: User,
    Message: Message,
}
</pre>
