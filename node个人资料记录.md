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
const chalk= require('chalk');
console.log(chalk.red('this is red!'));
```

