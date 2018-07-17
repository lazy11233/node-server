# node-server
功能：处理路由，支持静态目录的 server
## 使用说明
1. 搭建 Node.js 开发环境

 从[这里](https://nodejs.org/en/download/)点击下载node，具体的安装过程无脑，网上也有很多的教程指导安装。
 在终端或者git bash中输入指令` node -v `显示为正常版本号表示安装成功。
 
2. 运行node.js

在index.js的文件路径下，终端中输入命令` node index.js `，这时，node-server正常启动

3. 打开浏览器，访问指定的文件或网页

打开浏览器，地址里输入：` localhost:8080/sample/test.html `,看到的不是` 404 Not Found `标明node-server创建运行成功。

## 代码解读
```javascript
/*
实现一个静态服务器
 */
var http = require("http");
var path = require("path");
var fs = require("fs");
var url = require("url");

//创建一个server对象,并设置它端口对象。
var server = http.createServer(function(request,response){
    // path.join(__dirname, 'static') 定位到当前文件，找到static文件夹 C:\Users\LiuZhao\Desktop\test-node\static
    staticRoot(path.join(__dirname, "node-server") ,request,response);
});
server.listen(8080);

function staticRoot(staticPath,request,response){
    //获取到用户要访问的完整url对象。
    var pathObj = url.parse(request.url, true);
    /*
    pathObj {
      protocol: null,
      slashes: null,
      auth: null,
      host: null,
      port: null,
      hostname: null,
      hash: null,
      search: '',
      query: {},
      pathname: '/demo.html',
      path: '/demo.html',
      href: '/demo.html' }
     */
    if(pathObj.pathnamea === "/"){
        pathObj.pathname += 'index.html';
    }
    var filePath = path.join(staticPath, pathObj.pathname);

    fs.readFile(filePath,"binary",function(err,fileContent){
        if(err){//没有找到路径或其他异常
            console.log("文件没能找到：" + filePath);
            response.setHeader("Content-type","text/html; charset=UTF-8");
            response.writeHead(404, "not found");
            response.end("<h1>404 Not Found</h1>")
        }else{
            console.log("请求的地址是：" + filePath);
            response.writeHead(200,"OK");
            response.write(fileContent,"binary");
            response.end();
        }
    });
}
```
