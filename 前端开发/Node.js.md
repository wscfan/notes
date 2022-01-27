# Node.js笔记
[toc]

## 1、创建 Node.js 应用
**1.1 引入 required 模块**
```javascript
var http = require("http");
```

**1.2 创建服务器**
```javascript
http.createServer(function(request,response){
	// 发送 HTTP 头部
	// HTTP 状态值：200 ：OK
	// 内容类型：text/plain
	response.writeHead(200,{'Content-type':'text/plain'});
}).listen(8888);

	// 发送 "Hello World"
	response.end('Hello World');
```

**1.3 打开服务器**  
进入到项目目录，执行下面的代码：  
```dos
node server.js
```
然后，打开浏览器访问 http://127.0.0.1:8888/ ，会看到内容 "Hello World"。

## 2、npm安装
**2.1 升级旧版本的 npm**
```dos
npm install npm -g
```

**2.2 使用 npm 命令安装模块**
```dos
npm install <Module Name>
```
全局安装可加上 `-g` 参数，如果需要同时全局安装和本地安装，可以使用 `npm link`  

**2.3 安装时出错解决**  
如果出现以下错误：  
```dos
npm err! Error: connect ECONNREFUSED 127.0.0.1:8087
```
解决办法：
```dos
npm config set proxy null
```

**2.4 查看安装信息**
```dos
npm list -g
```
如果要查看某个模块的版本号，可以使用如下命令：
```dos
npm list grunt
```

**2.5 卸载模块**
```dos
npm uninstall express
```
到指定目录查看包是否存在：
```dos
npm ls
```

**2.6 更新模块**
```dos
npm update express
```

**2.7 搜索模块**
```dos
npm search express
```

**2.8 创建模块**
```dos
npm init
```
接下来可以使用下面命令在 npm 资源库中注册用户：
```dos
npm adduser
```
接下来可以使用下面命令来发布模块：
```dos
npm publish
```

## 3、REPL（交互式解释器）
**3.1 下划线变量**  
可以使用下划线（_）获取表达式的运算结果：
```
$ node
> var x = 10
undefined
> var y = 20
undefined
> x + y
30
> var sum = _
undefined
> console.log(sum)
30
undefined
```

## 4、回调函数