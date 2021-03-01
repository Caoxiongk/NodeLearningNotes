## API文档

```
https://nodejs.org/dist/latest-v15.x/docs/api/
```

## 格式编码

```
https://tool.oschina.net/commons
```

## 简单的http请求

```
let http = require('http')
    //2、使用http.createServe()方法创建一个web服务器，返回一个serve实例
let server = http.createServer()
    //3、请求接口 
    //参数 
    //Request 请求对象 可以用来获取客户端的一些请求信息，比如请求路径
    //Response 响应对象 可以用来给客户端发送响应消息 Response对象有一个方法，write可以用来给客户端发送响应数据，但是最后一次一定要用end结束 或者直接用end发送响应数据
server.on('request', (request, response) => {
        // console.log('收到请求' + request.url)
        // response.write('12312')
        response.setHeader('Content-Type', 'text/plain;charset=utf-8') //输出中文不乱码 一定要加 text/plain输出普通文本 text/html输出html标签
        response.setHeader('Content-Type', 'image/jpeg')
        response.end('安师大')
            // response.end('asdas')
    })
    //4、绑定端口，启动服务器
server.listen(3000, () => {
    console.log('成功')
})
```

## 不同路径请求不同的方法

```
let http = require('http')
    //2、使用http.createServe()方法创建一个web服务器，返回一个serve实例
let server = http.createServer()
    //3、请求接口 
    //参数 
    //Request 请求对象 可以用来获取客户端的一些请求信息，比如请求路径
    //Response 响应对象 可以用来给客户端发送响应消息 Response对象有一个方法，write可以用来给客户端发送响应数据，但是最后一次一定要用end结束 或者直接用end发送响应数据
server.on('request', (request, response) => {
      if(request.url === '/login'){
      console.log('123')
      }
    })
    //4、绑定端口，启动服务器
server.listen(3000, () => {
    console.log('成功')
})
```



## *module*.exports和exports（定义方法被其他页面引用）

***1、定义方法***

```
let a = '123'
console.log(a)
module.exports = {
    a
}
```

***2、页面使用***

```
1、通过 let b= require('js路径')引入js文件
2、引用 b.a
```

***3、module.exports***

```
当module.exports = a 
那么页面中引用的方法为let b= require('js路径')引入js文件
当module.exports = {
a
}
那么页面中引用的方法为let b= require('js路径')引入js文件
引用 b.a
```



## PATH路径

***path.normalize .当前目录 ..跳出当前目录***  

**当找到多个连续的路径段分隔字符时（例如 POSIX 上的 `/`、Windows 上的 `\` 或 `/`），则它们将被替换为单个平台特定的路径段分隔符（POSIX 上的 `/`、Windows 上的 `\`）。 尾部的分隔符会保留**

```
path.normalize('/a/b/c/d') /a/b/c/d
path.normalize('/a/b//////c/d') /a/b/c/d
path.normalize('/a/b/./c/d') /a/b/c/d
path.normalize('/a/b/../c/d') /a/c/d  
```

***path.join 拼接路径***

**将路径片段使用特定的分隔符（window：\）连接起来形成路径，并规范化生成的路径。若任意一个路径片段类型错误，会报错。**

```
path.join('./a','b') /a/b
path.join('/a','/b/e/..c/') /a/b/c
```

***path.resolve***

**把一个路径或路径片段的序列解析为一个绝对路径。相当于执行cd操作。**

## 读取文件

```
const fs = require('fs');
// 读取文件 readFile
//err 如果读取成功 err为null 不成功就是错误对象
fs.readFile('./test.txt', 'utf8', (err, data) => {
    if (!err) {
        console.log(data);
    } else {
        console.log(err)
    }
});
```

## 写入文件

```
//err 如果读取成功 err为null 不成功就是错误对象
fs.writeFile('./hellow.txt', 'hello word', (err) => {
    if (!err) {
        console.log('写入成功');
    } else {
        console.log(err)
    }
})
```

## 读取目录下面的文件

```
fs.readdir('C:/Users/admin/Desktop/个人/新建文件夹/node', (err, files) => {
    console.log(files)
})
```

## 在原来的文件里追加内容，如果没有则创建文件

```
fs.appendFileSync('./hellow.txt', '123')
```

## 获取文件的信息或者状态

```
const result = fs.statSync('./hellow.txt')
console.log(result) result是文件所有的信息
```

## 创建文件夹

```
try{
    //执行
    fs.mkdirSync('./api')
}catch(error){
    //捕获异常异常
    console.log(errpr)
}
```

## 文件重命名

```
fs.renameSync('./hellow.txt', '123.txt')
```

## 删除文件和文件夹

```
fs.unlinkSync('./hellow.txt')
fs.rmdirSync('./api')
```

## unti模块将异步操作的回调转为promise的形式

```
const util = require('util')
const fs = require('fs')
//将异步操作的回调转为promise的形式
//unti.promisify()高阶函数
fs.readFile('path',(err,result)=>{})
const readFileAsync/*返回值也是一个函数*/=util.promisify(/*参数为异步函数*/fs.readFile)

//调用方法 path为readFile的参数，但是无需传递回调函数
readFileAsync(path).then(res=>{

}).catch(err=>{
    
})

//多个方法一起使用
{
    const readFileAsync/*返回值也是一个函数*/=util.promisify(/*参数为异步函数*/fs.readFile)
    const readFileAsyncs/*返回值也是一个函数*/=util.promisify(/*参数为异步函数*/fs.readFile)
    (async()=>{
      const data  = await readFileAsync(path)
    })()
}
```

## Url模块，获取地址对应的信息和拼接地址

```
const url = require('url')
const path = 'http://www.baidu.com?username=caoxiong&user=cao'
const result = url.parse(path, false /*如果为true，query是对象*/ )
console.log(result)
// 一个URL字符串转换成对象并返回。
/*
true
Url {
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'www.baidu.com',
  port: null,
  hostname: 'www.baidu.com',
  hash: null,
  search: '?username=caoxiong&user=cao',
  query: [Object: null prototype] { username: 'caoxiong', user: 'cao' },
  pathname: '/',
  path: '/?username=caoxiong&user=cao',
  href: 'http://www.baidu.com/?username=caoxiong&user=cao'
}
false
Url {
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'www.baidu.com',
  port: null,
  hostname: 'www.baidu.com',
  hash: null,
  search: '?username=caoxiong&user=cao',
  query: 'username=caoxiong&user=cao',
  pathname: '/',
  path: '/?username=caoxiong&user=cao',
  href: 'http://www.baidu.com/?username=caoxiong&user=cao'
}
*/
    // 拼接url
let result2 = url.format({
    protocol: 'http',
    hostname: 'www.baidu.com',
    port: 80,
    pathname: '/text/a/b/c',
    query: {
        name: '123',
        age: 10
    },
    hash: '#test'
})
console.log(result2)
    // http://www.baidu.com:80/text/a/b/c?name=123&age=10#test
```

## querystring模块处理url的参数

```
const qs = require('querystring')

//处理请求的参数
const str = 'tn=48020221_36_hao_pg&tns=48020221_36_hao_pg'

{
    //解析参数
    const result = qs.parse(str)
    console.log(result)
        /*
            [Object: null prototype] {
            tn: '48020221_36_hao_pg',
            tns: '48020221_36_hao_pg'
            }
        */
}
```

```
{
    //组装参数
    const obj = {
        a: 1,
        b: 2
    }
    const result = qs.stringify(obj)
    console.log(result)
        /*a=1&b=2*/
}
```

```
{
    //对特殊字符进行解码
    const result = qs.unescape('%25E9%2589%25B4%25E4%25BA%258E&rsv_pq')
    console.log(result)
        /*教育 */
}

```

```
{
    //对字符进行编码
    const result = qs.escape('超凶')
    console.log(result)
        /*%E8%B6%85%E5%87%B6*/
}
```

## Stream(流)

```
Stream 是一个抽象接口，Node 中有很多对象实现了这个接口。例如，对http 服务器发起请求的request 对象就是一个 Stream，还有stdout（标准输出）。

Node.js，Stream 有四种流类型：

Readable - 可读操作。

Writable - 可写操作。

Duplex - 可读可写操作.

Transform - 操作被写入数据，然后读出结果。

所有的 Stream 对象都是 EventEmitter 的实例。常用的事件有：

data - 当有数据可读时触发。

end - 没有更多的数据可读时触发。

error - 在接收和写入过程中发生错误时触发。

finish - 所有数据已被写入到底层系统时触发。
```

```
var fs = require("fs");

// 创建可读流
var readerStream = fs.createReadStream('input.txt');

//创建写入流
var writerStream = fs.createWriteStream('output.txt');

//readerStream传入writerStream
readerStream.pipe(writerStream)
```

## Event事件触发与事件监听器功能的封装

```
const events = require('events')
    //创建事件对象
const emitter = new events.EventEmitter()
    //监听事件
emitter.on('test', (...rest) => {
        console.log('监听到了了')
        console.log(rest)
    })
    //监听 代替res那一块
const enentCallback = (...rest) => {
        console.log('监听到了了')
    }
    //监听一次
emitter.once('test', (...rest) => {
        console.log('监听一次')
        console.log(rest)
    })
    //触发事件
emitter.emit('test', 'a,b,a')
    //移除事件
emitter.off('test', enentCallback)
```

## nodemon 类似vue中的启动服务器，不用每次修改都重启一次

```
npm install --save-dev nodemon
nodemon   ./main.js // 启动node服务
nodemon ./main.js localhost 6677 // 在本地6677端口启动node服务
"start": "ts-node -r tsconfig-paths/register nodemon src/main.ts",
```

## chalk让打印五颜六色

```
npm i chalk
const chalk= require('chalk');
console.log(chalk.red('this is red!'));
```

## Express框架

##### ***1、安装***

```
yarn init
npm install express --save
```

##### ***2、案例***

```
const express = require('express')
const app = express()
const chalk = require('chalk');
let port = 3000
    //err失败就带有值 成功代表没值
app.listen(port, (err) => {
    if (err) {
        console.log(chalk.red('启动失败'))
    } else {
        console.log(chalk.green('启动成功'))
        console.log(chalk.green(`访问地址:http://localhost:${port}`))
    }
})
```

##### ***3、请求第三方接口***

```
//node请求第三方接口---追书神器 request需要安装
var request = require("request");
var url = "http://api.zhuishushenqi.com/cats/lv2/statistics";
request(
  {
    url: url,
    method: "GET",
    json: true,
    headers: {
      "content-type": "application/json",
      "User-Agent": "chrunleeAutoLogin",
    },
  },
  (error, response, body) => {
    if (!error && response.statusCode == 200) {
      console.log(body); // 请求成功的处理逻辑
    }
  }
);
```

##### ***4、res相应的方法***

```
res.download() 提示要下载的文件
res.end() 结束响应过程
res.json() 发送json响应
res.redirect() 重定向
res.render() 呈现视图模板
res.send() 发送各种响应类型
res.sendStatus() 设置响应状态码并将其字符串表示形式作为相应主体发送
res.status() 设置响应状态码
res.sendFile('./text.txt') 发送文件
```

##### ***5、req的监听***

```
方法1
req.on('oppo') 创建
req.on('data',(bf)=>{}) 接收数据时触发的事件
req.on('error',()=>{})  错误触发的事件
req.on('end',()=>{}) 数据接收完毕后触发的时间
```

```
方法2
try{

}.catch(err){

}
```

```
总结:看个人喜好，个人更推荐第一种
```

##### **6、*http和express结合使用***

```
const express = require("express");
const app = express();
const http = require("http");
const serve = http.createServer(app);
const colors = require("colors-console");
const port = 3000;
const url = require("url");
app.get("/login", (req, res) => {
  console.log("123");
  console.log(req);
  res.write("亲爱的Caoxiong，您当前访问的是登录API,GET请求");
  res.end("");
});
serve.listen(port, () => {
  console.log(colors("green", `访问地址:http://localhost:${port}来启动服务`));
});
```

##### ***7、app路由next的使用***

```
//router表示跳出当前，即代码只执行到这一块 next代表往下执行
app.post("/manyTimes", [
  (req, res, next) => {
    res.write("请求的第一个方法");
    next(router);
  },
  (req, res, next) => {
    res.write("请求的第二个方法");
    next();
  },
  (req, res, next) => {
    res.write("请求的第三个方法");
    res.end();
  },
]);
```

##### ***8、路由中间件(用于请求接口前要做的事情，例如拦截)***

```
//中间件 主要用于在请求前处理某些操作 如果成功就用next往下走 否则就停在这里 统一处理大家公共部分
// /和*都能匹配所有的请求
app.use("/", (res, req,next) => {
  console.log(res.url);
});

app.post("/logins", (req, res, next) => {
  res.write("亲爱的Caoxiong，您当前访问的是登录logins,POST请求");
  res.end("");
});
```

##### ***9、处理请求的方法(规定前端使用formData还是json进行传值)***

```
方法1
//处理请求的方法
app.use(express.urlencoded); //前端传值必须为x-www-from-urlencoded
app.use(express.json); //前端传值必须为json raw
//在请求中 用req.body获得接收的参数
```

```
方法2
通过判断res.headers.ty来进行判断前端的处理方法
```

##### ***10、路由器(跳转不同的接口，方法统一)***

```
router.js

const {Router} = require('express')
const router = new Router()

router.get('/login',()=>{

})

module.exports = router;
```

```
app.js

const router = require('./router.js')
app.use('/api',router)
```

```
调用
localhost/api/login
```

## Mongodb的使用

##### ***1、安装***

```
安装地址:https://www.mongodb.com/download-center/community
菜鸟教程:https://www.runoob.com/mongodb/mongodb-window-install.html
参考菜鸟教程
```

##### ***2、配置环境变量***

```
右键我的电脑-属性-高级系统设置-环境变量-双击path-新建-Mongodb的安装路径-保存
```

##### ***3、检测是否已安装***

```
>mongod --version

Build Info: {
    "version": "4.4.4",
    "gitVersion": "8db30a63db1a9d84bdcad0c83369623f708e0397",
    "modules": [],
    "allocator": "tcmalloc",
    "environment": {
        "distmod": "windows",
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}
```

##### ***4、去C盘建立文件夹***（数据库存储路径）

```
data\db   data文件及 以及 data文件下面的db文件夹
```

##### ***5、启动和关闭Mongdb***

```
cmd中输入 mongod  开启
```

```
关闭cmd窗口代表Mongdb已关闭
```

##### ***6、修改默认的数据库存储路径(不推荐)***

```
mongod --dbpath=数据库存储路径
```

##### ***7、连接和退出数据库***

```
重新打开一个cmd窗口
输入指令mongo连接
输入指令exit退出
```

##### ***8、基本命令***

```
show dbs --查看显示所有的数据库
db --查看当前连接的数据库，默认为test，但是实际上用(show dbs)显示所有数据库的时候，是没有test的，是因为此时的test还没有数据，只有插入数据的时候，才能显示
use 数据库名称 --切换到指定的数据库，如果没有，则新建  
db.students.insertOne({'name:'caoxiong'}) --插入数据 students可以任意命名，它是一个集合，里面可以存储数据，集合等等，代表的是一张数据表
show collections --查看当前数据库的集合
db.students.find() --查询所有的students里面的数据
```

## 通过Node来操作Mongdb

##### ***1、使用第三方moogoose来操作MongoDB***

```
http://www.mongoosejs.net/docs/index.html
```

##### ***2、安装指令***

```
 npm install mongoose
```

##### ***3、实例***

```
const mongoose = require('mongoose');
//连接数据库
mongoose.connect('mongodb://localhost/test');
//创建一个模型 ---创建一张表cat  name为表头
const Cat = mongoose.model('Cat', { name: String });
//创建实例
const kitty = new Cat({ name: '曹雄s' });
//执行
//插入数据
kitty.save((err) => {
    if (err) {
        console.log('失败')
    } else {
        console.log('成功')
    }
})
```

##### ***4、官方指南***

```
// 1.引包
// 注意：按照后才能require使用
var mongoose = require('mongoose');

// 拿到schema图表
var Schema = mongoose.Schema;

// 2.连接数据库
// 指定连接数据库后不需要存在，当你插入第一条数据库后会自动创建数据库
mongoose.connect('mongodb://localhost/test');

// 3.设计集合结构（表结构）
// 用户表
var userSchema = new Schema({
	username: { //姓名
		type: String,
		require: true //添加约束，保证数据的完整性，让数据按规矩统一
	},
	password: {
		type: String,
		require: true
	},
	email: {
		type: String
	}
});

// 4.将文档结构发布为模型
// mongoose.model方法就是用来将一个架构发布为 model
// 		第一个参数：传入一个大写名词单数字符串用来表示你的数据库的名称
// 					mongoose 会自动将大写名词的字符串生成 小写复数 的集合名称
// 					例如 这里会变成users集合名称
// 		第二个参数：架构
// 	返回值：模型构造函数
var User = mongoose.model('User', userSchema);

//增加数据
var admin = new User({
    username: '123',
    password: '2',
    email: '1',
})
//err有值的时候 为失败 没值为成功 ret表示你存储的数据 
admin.save((err,ret) => {
    if (err) {
        console.log('失败')
    } else {
        console.log('成功')
    }
})
```

##### ***5、添加数据***(save)

```
save()
//实例
admin.save((err,ret) => {
    if (err) {
        console.log('失败')
    } else {
        console.log('成功')
    }
})
```

##### ***6、查询数据(find)***

```
//查询所有：
User.find(function(err,ret){
	if(err){
		console.log('查询失败');
	}else{
		console.log(ret);
	}
});
```

```
// 根据条件查询
User.find({ username:'xiaoxiao' },function(err,ret){
	if(err){
		console.log('查询失败');
	}else{
		console.log(ret);
	}
});
```

```
// 按照条件查询单个，查询出来的数据是一个对象（{}）
// 没有条件查询使用findOne方法，查询的是表中的第一条数据
User.findOne({
	username: 'xiaoxiao'
}, function(err, ret) {
	if (err) {
		console.log('查询失败');
	} else {
		console.log(ret);
	}
});
```

##### ***7、更新数据***

```
//更新所有
User.remove(conditions,doc,[options],[callback]);
```

```
//根据指定条件更新一个
User.FindOneAndUpdate([conditions],[update],[options],[callback]);
```

```
// 更新	根据id来修改表数据
User.findByIdAndUpdate('5e6c5264fada77438c45dfcd', {
	username: 'junjun'
}, function(err, ret) {
	if (err) {
		console.log('更新失败');
	} else {
		console.log('更新成功');
	}
});
```

##### ***8、删除数据***

```
//根据条件删除所有
User.remove({
	username: 'xiaoxiao'
}, function(err, ret) {
	if (err) {
		console.log('删除失败');
	} else {
		console.log('删除成功');
		console.log(ret);
	}
});
```

```
//根据条件删除一个
Model.findOneAndRemove(conditions,[options],[callback]);
```

```
//根据id删除一个
Model0.findByIdAndRemove(id,[options],[callback]);
```

## 通过Node操作mysql

##### ***1、安装mysql***

```
npm i mysql
```

##### ***2、实例***

```
var mysql = require('mysql');
//1、创建数据库
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'me',
  password : 'secret',
  database : 'my_db'//数据库
});
 //2、连接数据库
connection.connect();
 //3、执行数据操作  SELECT 1 + 1 AS solution数据库指令 这个地方只要写入增删改查的数据库指令即可
connection.query('SELECT 1 + 1 AS solution', function (error, results, fields) {
  if (error) throw error; //失败抛出错误
  console.log('The solution is: ', results[0].solution);
});
 4、关闭数据库
connection.end();
```

## 爬虫

```
const http = require('http');
const cheerio = require('cheerio');
const fs = require('fs');
//要请求的地址
let urlCrawler = 'http://www.ip3q.com/e/action/ListInfo.php?&classid=90&ph=1&slx=%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E6%A8%A1%E6%9D%BF';
const studien = require('./index01.js');

//请求地址
http.get(urlCrawler, (res) => {
    let crawlerHtml = '';
    //防止乱码
    res.setEncoding('utf-8');
    //接收数据过程中拼接数据
    res.on('data', (item) => {
            crawlerHtml += item
        })
        //接收完成后进行的操作
    res.on('end', () => {
        let files = [];
        // console.log(crawlerHtml)
        const $ = cheerio.load(crawlerHtml);
        //找到需要爬取的片段
        $('.pics-list-price ul li').each((index, value) => {
            //找到标题
            let title = $(value).find('h2').text();
            let list = {
                    title
                }
                //放到数组中
            files.push(list)
        });
        //放入文件 如果没有则生成该文件
        fs.appendFileSync('./hellow.json', JSON.stringify(files, "", '\t'))
    })
})
```

