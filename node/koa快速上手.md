# nodeJS框架 -- koa快速入门

## 一、搭建框架

1、首先新建一个文件夹，在文件夹中使用`npm init --yes`初始化文件夹，生成一个package.json文件

其中main属性的指文件入口，默认值是index.js，如果文件入口不是index.js，那么需要做相应的修改

2、执行命令`npm install koa koa-router --save`下载koa框架，下载完koa和kos-router，文件夹中多出了一个名为node_modules的文件夹和一个名为package-lock.json的文件

3、新建一个入口文件，文件名应该和package.json文件中main属性的属性值一致，这里我是用app.js；

```js
// 将koa和koa-router引入
const koa = require("koa")
const Router = require("koa-router")

// 实例化koa和koa-router
const app = new koa();
const router = new Router()

// 配置路由: router.routes()的作用是路由，router.allowedMethods()的作用是允许任何请求
// 以下两种写法均可以
// app.use(router.routes()).use(router.allowedMethods());
app.use(router.routes(), router.allowedMethods())

// 设置登录口
const port = process.env.PORT || 5000;

// 监听端口号
app.listen(port, () => {
    console.log(`server start at port:　${port}`)
})

// 路由跳转
/*
	在浏览器地址栏输入：
	localhost:5000，页面显示{ "msg": "hello, koa" }
	localhost:5000/list，页面显示not found
	localhost:5000/list/goods，页面显示商品
	localhost:5000/list/permission，显示权限
	localhost:5000/list/admin，显示管理员
*/
router.get('/', async ctx => {
    // ctx.response.body = { msg: 'hello, koa' }，与下面的写法等价
    ctx.body = { msg: 'hello, koa' }
})
router.get('/list/goods', async ctx => {
    ctx.body = '商品'
})
router.get('/list/permission', async ctx => {
    ctx.body = '权限'
})
router.get('/list/admin', async ctx => {
    ctx.body = '管理员'
})
```

4、执行文件：可以直接使用`npm run start`，或者在package.json文件中的script属性中进行配置

```json
{
    "script": {
        "start": "node app.js"
    }
}
```

配置完使用命令`node app.js`就可以执行文件

5、使用nodemon监听变化，每次保存后自动执行文件，不需要自己每次都去执行一遍

下载安装nodemon的命令：`npm install -g nodemon`全局安装

在package.json文件中配置：

```json
{
    "script": {
        "nodemon ": "nodemon app.js"
    }
}
```

直接执行命令：`nodemon`就可以了，只要我们修改了文件并保存，就会自动执行文件

6、此时在浏览器地址栏上执行输入localhost:5000，页面会显示：{ "msg": "hello, koa" }

## 二、拆分路由

一个项目的入口文件要求简洁，如果把路由都写在入口文件会显得入口文件很冗长

1、把路由迁移到新的文件夹中，新建一个文件夹，名为router，在该文件夹中新建一个index.js文件，为其默认的文件，

把app.js文件中关于路由方面的代码搬迁到index.js文件中，在index.js中通过module.exports将router暴露出去，在app.js中进行引入

```js
// app.js文件代码

// 将koa和koa-router引入
const koa = require("koa")
// 实例化koa和koa-router
const app = new koa();
// 引入路由
const router = require('./router')
// 配置路由
app.use(router.routes(), router.allowedMethods())
// 设置登录口
const port = process.env.PORT || 5000;
// 监听端口号
app.listen(port, () => {
    console.log(`server start at port:　${port}!!`)
})

// router 文件夹中index.js文件代码

const Router = require("koa-router")
const router = new Router()
router.get('/', async ctx => {
    ctx.body = { msg: 'hello, koa' }
})
router.get('/list/goods', async ctx => {
    ctx.body = '商品'
})
router.get('/list/permission', async ctx => {
    ctx.body = '权限'
})
router.get('/list/admin', async ctx => {
    ctx.body = '管理员'
})
module.exports = router
```

2、虽然这样简洁了，但是项目中的请求的路由一般是很多的，所以这么写router的index.js文件中的路由写起来就很长，显得很冗长，看这些路由可以发现，都是list路由下的，所以可以根据这个特征再次进行划分：把处于list路由下的路由搬迁到新的文件中

在router文件夹中新建一个list.js文件，将关于list的路由全部搬迁到这个文件下，通过module.exports将路由实例list暴露出去；然后在index.js文件中引入，并使用router的中间件use将其启动

```js
// app.js文件不变

// index.js文件代码修改后代码如下：

const Router = require("koa-router")
const router = new Router()
const list = require('./list')
router.get('/', async ctx => {
    ctx.body = { msg: 'hello, koa' }
})
// 使用router的中间件use，第二三个参数与koa实例使用中间件use的参数一致，第一个参数用来指定路由
router.use('/list',list.routes(), list.allowedMethods())

module.exports = router

// list.js文件代码如下：

const Router = require("koa-router")
const list = new Router()
list.get('/goods', async ctx => {
    ctx.body = '商品'
})
list.get('/permission', async ctx => {
    ctx.body = '权限'
})
list.get('/admin', async ctx => {
    ctx.body = '管理员'
})
module.exports = list
```

## 三、连接MySql

### 1、安装MySQL

执行`yarn add mysql`安装MySQL

### 2、创建连接池以及操作数据库的方法

在项目根目录新建一个文件夹utils，用来存放工具类文件，在该文件夹中新建一个文件db.js，用来对数据库进行基本的增删改查

```js
// db.js文件代码

// 引入数据库 
let mysql = require("mysql")
// 创建数据库连接池
var pool = mysql.createPool({
    host: 'localhost', // 连接的服务器
    port: 3306, // mysql服务运行端口
    database: 'dbname', // 选择的数据库
    user: 'root', // 用户名
    password: '123456' // 用户密码
})
// 对数据库进行增删改查的基本操作
function query(sql, callback) {
    pool.getConnection(function (err, connection) {
        connection.query(sql, function (err, rows) {
            callback(err, rows)
            connection.release()
        })
    })
}
```

### 3、开启mysql服务

使用cmd(命令行)执行`net start mysql8.0`，开启mysql服务

首先要在本地安装有mysql，安装MySQL的过程中有如下一步：

<img src="D:\GitHubRepository\node\koa快速上手.assets\image-20210224144049668.png" alt="image-20210224144049668" style="zoom:50%;" />

Windows Service Name要记住，我在这里的命名是mysql8.0，所以我使用的是`net start mysql8.0`来启动MySQL服务，如果是其他命名，需要对启动命令做相应修改；

运行命令出现：服务名无效，说明是mysql服务名错了，或者mysql没有启动；

查看MySQL是否已经启动：win + R键，输入services.msc，找到mysql，如果状态是禁用，需要开启，改为自动；

如果MySQL已经启动，执行命令出现：发生系统错误 5，拒绝访问，这种问题是权限不够，我们可以去C:\Windows\System32中寻找到cmd.exe，右击选择以管理员身份运行，再次执行命令，就可以了；

<img src="D:\GitHubRepository\node\koa快速上手.assets\image-20210224144908491.png" alt="image-20210224144908491" style="zoom:50%;" />

注：

以管理员身份运行Windows PowerShell或者cmd都可以，快捷键：

以管理员身份运行Windows PowerShell：win + X键，选择Windows PowerShell(管理员)(A)；

以管理员身份运行cmd：①win + X键，选择任务栏设置，关闭“当我使用右键单击‘开始’按钮或按下Windows键+X时，在菜单中将命令提示符替换为Windows PowerShell”的选项，再次使用win + X键，选择命令提示符(管理员)(A)；②到C:\Windows\System32中寻找到cmd.exe，右击选择以管理员身份运行

### 4、开启mysql服务后，连接数据库

①执行：`mysql -uroot -p`，回车，输入数据库连接密码，回车；或者使用明文密码，在-p后面拼接上密码，执行`mysql -uroot -ppassword`，回车

②进入数据库，执行`show databases;`查看已存在的数据库，分号是必须的，sql语句不见分号不执行。

③执行`create database name;`，创建一个名为name的数据库

④执行`use name;`，使用名为name的数据库

⑤执行`show tables;`查看所使用的数据库中的表格

⑥可以使用Navicat可视化数据库，来查看数据库中的数据，安装Navicat后选择左上角的连接，选择MySQL，填写链接名，这个可以自定义，然后填写数据库密码，填写完点击确定即可，这样就可以看到mysql中的数据库

### 5、为数据库添加一张表，同时为表添加一些字段

```sql
create table goods(
    /* PRIMARY KEY为指定该项为表的键，AUTO_INCREMENT为每插入一条数据，取值自增1，无需设置 */
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20) COMMENT '商品名称',
    price INT COMMENT '商品价格',
    newLevel VARCHAR(20) COMMENT '商品新旧程度',
    buyTime VARCHAR(20) COMMENT '商品购入时间',
    status VARCHAR(8) COMMENT '商品状态',
    seller VARCHAR(10) COMMENT '出售者姓名',
    sellerPhone VARCHAR(12) COMMENT '出售者电话',
    salingTime VARCHAR(20) COMMENT '商品发布时间',
    customer VARCHAR(10) COMMENT '收货人姓名',
    address VARCHAR(20) COMMENT '收货地址',
    customerPhone VARCHAR(20) COMMENT '收货人手机号码',
    saledTime VARCHAR(20) COMMENT '购买时间'
);
```

![image-20210224200615888](D:\GitHubRepository\node\koa快速上手.assets\image-20210224200615888.png)

执行`drop table tablename`可以删除名为tablename的表格(**谨慎使用**)

### 6、向表中添加/删除/修改数据

#### ①添加数据

**用法1：**

```sql
insert into goods values(1, '羽毛球', 100, '三成新', '2020-12-12', 'false', '吴彦祖', '13682740622', '2021-02-24', '', '', '', '');
```

![image-20210224203052710](D:\GitHubRepository\node\koa快速上手.assets\image-20210224203052710.png)

**用法2：**

由于id是自增的，所以可以不用设置，但是漏掉这个项不写又会报错，有一项没匹配到，所以可以换下面的写法

```sql
insert into goods (name, price, newLevel, buyTime, status, seller, sellerPhone, salingTime) values('乒乓球', 99, '九成新', '2010-10-10', 'true', '坤坤', '13682740611', '2020-02-20');
```

![image-20210224203618303](D:\GitHubRepository\node\koa快速上手.assets\image-20210224203618303.png)

**用法3：**

将数组中的数据通过循环插入到对应的位置，字段要和数据库的字段匹配，即使传的是空值

```js
// 引入db文件，使得我们对数据库进行操作
var db = require('./db.js');
// 插入数据库的数据
var goodsInfo = [
    {
        id: 3,
        name: '篮球',
        price: 999,
        newLevel: '八成新',
        buyTime: '2021-01-01',
        status: 'false',
        seller: '坤坤',
        sellerPhone: '14317628751',
        salingTime: '2021-02-02',
        customer: '',
        address: '',
        customerPhone: '',
        saledTime: ''
    },
    {
        id: 4,
        name: '足球',
        price: 999,
        newLevel: '八成新',
        buyTime: '2021-01-01',
        status: 'false',
        seller: 'C罗',
        sellerPhone: '14317628751',
        salingTime: '2021-02-02',
        customer: '',
        address: '',
        customerPhone: '',
        saledTime: ''
    }
]

goodsInfo.map(val => {
    let sql = `insert into goods values (${val.id}, '${val.name}', ${val.price}, '${val.newLevel}', '${val.buyTime}', '${val.status}', '${val.seller}', '${val.sellerPhone}', '${val.salingTime}', '${val.customer}', '${val.address}', '${val.customerPhone}', '${val.saledTime}')`

    db.query(sql, (err, data) => {
        if (err) throw err;
        console.log(data)
    })
})
```

<img src="D:\GitHubRepository\node\koa快速上手.assets\image-20210224215208930.png" alt="image-20210224215208930" style="zoom:50%;" />

![image-20210224220722498](D:\GitHubRepository\node\koa快速上手.assets\image-20210224220722498.png)

注意：

如果出现错误：`ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client`，按顺序执行以下命令：

```sql
/* 其中，123456为数据库密码 */
mysql -u root -p
123456
use mysql;
alter user 'root'@'localhost' identified with mysql_native_password by '123456';
flush privileges;
```

#### ②修改数据

修改id为2的数据中的seller等字段

```js

```

# nodeJS小知识

## 一、读取文件

```js
// 首先需要找到文件的位置
const path = require('path')
let myPath = path.join(__dirname, '../../xxx') // 此处的__dirname表示当前文件所在目录

// 引入文件流
const fs = require('fs')
fs.readFile(myPath, (err, data) => {
    if ( err ) throw err;
    console.log(data.toString()) // 如果调用toString方法，我们得到的是一个buffer二进制文件流
})
```

由于readFile是一个异步操作，所以可以将其封装成一个方法，假设所读文件在根目录的assets文件夹中

```js
const path = require('path')
const fs = require('fs')

function readFileFn(arg) {
    return new Promise((resolve, reject) => {
        let myPath = path.join(__dirname, `../assets/${arg}`)
        fs.readFile(myPath, (err, data) => {
    		if ( err ) throw err;
             resolve(data.toString())
		})
    })
}
```

























