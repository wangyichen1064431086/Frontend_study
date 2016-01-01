# Chapter32 使用Ajax(第1部分）

## Ajax简介
- Ajax让你能向服务器异步发送和接收数据，然后用javascript解析。
- Ajax是Asychronous JavaScript and XML(异步JavaScript和XML)的缩写。
- Ajax核心规范的名称继承于你用来建立和发起请求的JavaScript对象：XMLHttpRequest

## 32.1 Ajax起步
eg:使用XMLHttpRequest对象
- var httpRequest=new XMLHttpRequest():  
  - 创建新的XMLHttpRequest对象。并非通过浏览器定义某个全局变量，而是通过关键词new
- httpRequest.open("GET",e.target.innerHTML+".html",true,"adam","secret")
  - "GET":指定HTTP方法。最为广泛的HTTP方法有“GET”和"POST"两种。**GET**请求适用于安全的交互行为，即你可以反复发起而不会带来副作用的请求；**POST**请求适用于不安全的交互行为，即每个请求都会导致服务器端发生某种变化，而重复请求可能会带来问题。
  - e.target.innerHTML+".html"：生成请求的URL
  - true:指定请求是否应当异步执行，应该始终设置为true
  - 最后两个参数：应当发送给服务器的用户名和密码

```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter32</title>
</head>

<body>
    <div>
        <button>Apples</button>
        <button>Lemons</button>
        <button>Bananas</button>
    </div>
    <div id="target">
        Press a Button
    </div>
    
    <script>
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;
        }
        
        function handleButtonPress(e) {
            var httpRequest=new XMLHttpRequest();//创建新的XMLHttpRequest对象。并非通过浏览器定义某个全局变量，而是通过关键词new
            httpRequest.onreadystatechange=handleResponse;//为readystatechange事件设置一个事件处理器，该事件处理器会在请求中被多次触发
            httpRequest.open("GET",e.target.innerHTML+".html");//告诉XMLHttpRequest对象你想要做什么：使用open方法指定HTTP方法（此处为GET）和需要请求的URL
            httpRequest.send();//没有向服务器发送任何数据，故send方法无参数可用
        }
        
        function handleResponse(e) {//处理响应
            if (e.target.readyState==XMLHttpRequest.DONE&&e.target.status==200) {
                document.getElementById("target").innerHTML=e.target.responseText;
            }
        }
    </script>

</body>
</html>
```

## 32.1.1处理响应
以上例 handleResponse函数为例：
- 当readystateChange事件被触发后，浏览器把一个Event对象传递给指定的处理函数。target属性则会指向与该事件相关的XMLHttpRequest对象
- XMLHttpRequest对象的readyState属性值：
值|数值|说明
---|---|----
UNSET|0|已创建XMLHttpRequest对象
OPENED|1|已调用open方法
HEADERS_RECEIVED|2|已收到服务器响应的标头
LOADING|3|已收到服务器响应
DONE|4|响应已完成或失效
- DONE状态并不表示请求成功，只代表请求完成。还必须用status属性获得HTTP状态码，返回200代表成功。readyState和status二者属性的值必须结合起来才能确定请求的结果。
```
    function handleResponse(e) {//处理响应
            if (e.target.readyState==XMLHttpRequest.DONE&&e.target.status==200) {
                document.getElementById("target").innerHTML=e.target.responseText;
            }
        }
```
## 32.1.2主流中的异类：Opera

## 32.2使用Ajax事件
XMLHttpRequest对象定义的事件
名称|说明|事件类型
----|----|--------
abort|在请求被中止时触发|ProgressEvent
error|在请求失败时触发|ProgressEvent
load|在请求成功完成时触发|ProgressEvent
loadend|在请求已完成时触发，无论成功还是出错|ProgressEvent
loadstart|在请求开始时触发|ProgressEvent
progress|触发以提示请求的进度|ProgressEvent
readystatechange|在请求生命周期的不同阶段触发|Event
timeout|如果请求超时则触发|ProgressEvent

- 除readystatechange和progress这两个事件是多次触发以提供进度更新外，其他事件都会在请求的某一个特定时点上触发

- 除readystatechange事件使用常规Event对象，其他事件使用ProgressEvent对象。该对象定义了Event对象所有成员，并增加了如下成员：
ProgressEvent对象定义的额外属性

名称|说明|返回值
---|----|------
lengthCompute|如果能够计算数据流的总长度则返回true|布尔值
loaded|返回当前已载入的数据量|数值
total|返回可用的数据总量|数值

```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter32</title>
    <style>
        table{
            margin: 10px;
            border-collapse: collapse;
            float: left;
        }
        div{
            margin: 10px;
        }
        td,th{
            padding: 4px;
        }
    </style>
</head>

<body>
    <div>
        <button>Apples</button>
        <button>Lemons</button>
        <button>Bananas</button>
    </div>
    <table id="events" border="1">
    </table>
    
    <div id="target">
        Press a Button
    </div>
    
    <script>
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;
        }
        
        var httpRequest;
        
        function handleButtonPress(e) {
          clearEventDetails();
          httpRequest=new XMLHttpRequest();
          httpRequest.onreadystatechange=handleResponse;
          httpRequest.onerror=handleError;
          httpRequest.onload=handleLoad;
          httpRequest.onloadend=handleLoadEnd;
          httpRequest.onloadstart=handleLoadStart;
          httpRequest.onprogress=handleProgress;
          httpRequest.open("GET",e.target.innerHTML+".html");
          httpRequest.send();
        }
        
        function handleResponse(e) {
            displayEventDetails("readystate("+httpRequest.readyState+")",e);
            if (e.target.readyState== XMLHttpRequest.DONE&&e.target.status==200) {
                document.getElementById("target").innerHTML=e.target.responseText;
            }
        }
        
        function handleError(e) {
            displayEventDetails("error",e);
        }
        
        function handleLoad(e) {
            displayEventDetails("load",e);
        }
        
        function handleLoadEnd(e) {
            displayEventDetails("loadend",e);
        }
        
        function handleLoadStart(e) {
            displayEventDetails("loadstart",e);
        }
        
        function handleProgress(e) {
            displayEventDetails("progress",e);
        }
        
        function clearEventDetails(){
            document.getElementById("events").innerHTML
            ="<tr><th>Event</th><th>lengthCommputable</th><th>loaded</th><th>total</th></tr>"
        }
        
        function displayEventDetails(eventName,e) {
            if (e) {
                document.getElementById("events").innerHTML+=
                "<tr><td>"+eventName+"</td><td>"+e.lengthComputable+"</td><td>"+e.loaded+"</td><td>"+e.total+"</td></tr>";
            }
            else{
                document.getElementById("events").innerHTML+="<tr><td>"+eventName+"</td><td>NA</td><td>NA</td><td>NA</td></tr>";
            }
        }
    </script>

</body>
</html>
```
## 32.3处理错误
eg:处理Ajax三类错误的三种方式
（书32-5）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter32</title>
</head>

<body>
    <div>
        <button>Apples</button>
        <button>Lemons</button>
        <button>Bananas</button>
        <button>Cucumber</button>
        <button id="badhost">Bad Host</button>
        <button id="badurl">Bad URL</button>
    </div>
   
    <div id="target">
        Press a Button
    </div>
    
    <div id="errormsg">
    </div>
    <div id="statusmsg">
    </div>
    <script>
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;//请求成功完成，但没有返回你想要的数据，如按下Cucumber按钮，但没有Cucumber.html文档--第三类错误
        }
        
        var httpRequest;
        
        function handleButtonPress(e) {
          clearMessages();
          httpRequest=new XMLHttpRequest();
          httpRequest.onreadystatechange=handleResponse;
          httpRequest.onerror=handleError;//通过注册的error事件监听器handleError来获取错误信息。--错误处理方式二
          try{
                switch (e.target.id) {
                    case "badhost":
                        httpRequest.open("GET","http://a.nodomain/doc.html");
                        //这类错误发生在请求已经生成，但其他方面出错。如传递给open方法一个不可用的URL。---第二类错误
                        //此处有两个问题：1.主机名不能被DNS解析，故浏览器无法生成服务器连接。通过注册的error事件监听器handleError来获取错误信息。
                        //2.URL和生成请求的脚本具有不同的来源，这在默认情况下是不允许的，默认只能向载入脚本的同源URL发出Ajax请求。
                        break;
                    case "badurl":
                        httpRequest.open("GET","http://");//传递给open方法一个残缺的URL---第一类错误
                        //这是一个会阻止请求执行的错误。
                        //需要用try```catch语句来围住设置请求的代码-- 错误处理方式一
                        break;
                    default:
                        httpRequest.open("GET",e.target.innerHTML+".html");
                        break;
                }
                httpRequest.send();
            }
            catch(error){//catch子句让你有机会从错误中回复。这里调用displayErrorMsg函数来显示错误消息。
                displayErrorMsg("try/catch",error.message);
            }
        }
        
        function handleError(e) {
            displayErrorMsg("Error event",httpRequest.status+httpRequest.statusText);
        }
        
        
        function handleResponse(e) {
           if (httpRequest.readyState==4) {
            var target=document.getElementById("target");
            if (httpRequest.status==200) {
                target.innerHTML=httpRequest.responseText;
            }
            else{//当请求的文档不存在时，会获得404的状态码--错误处理方式三
                document.getElementById("statusmsg").innerHTML
                ="Status:"+httpRequest.status+" "+httpRequest.statusText;
            }
           }
        }
        
        function displayErrorMsg(src,msg) {
            document.getElementById("errormsg").innerHTML=src+":"+msg;
        }
        
        function clearMessages() {
            document.getElementById("errormsg").innerHTML="";
            document.getElementById("statusmsg").innerHTML="";
        }
    </script>

</body>
</html>
```
- 三种错误方式（待总结）
1. 处理设置错误：向XMLHttpRequest对象传递了错误的数据。如：传递给open方法一个残缺的URL。这是一种会**阻止请求执行**的错误。
 - 可用try`...catch语句围住设置请求的代码来解决
2. 处理请求错误：请求已经生成，但其他方面出错。如：错误的主机（不能被解析的域名，或目标URL和生成请求的脚本具有不同的来源）
 - 可为error事件注册一个监听器
```
httpRequest.onerror=handleError;
 function handleError(e) {
            displayErrorMsg("Error event",httpRequest.status+httpRequest.statusText);
        }
```
3. 处理应用程序错误：请求成功完成，但没有返回想要的数据。如：文档不存在。
- 可设置函数，处理200以外的httpRequest.status
```
 function handleResponse(e) {
           if (httpRequest.readyState==4) {
            var target=document.getElementById("target");
            if (httpRequest.status==200) {
                target.innerHTML=httpRequest.responseText;
            }
            else{//当请求的文档不存在时，会获得404的状态码--错误处理方式三
                document.getElementById("statusmsg").innerHTML
                ="Status:"+httpRequest.status+" "+httpRequest.statusText;
            }
           }
```
        }

## 32.4获取和设置标头
## 32.4.1覆盖请求的HTTP方法：X-HTTP-Method-Override标头
- HTTP标准常被用于在互联网上请求和传输HTML文档，它定义了许多方法。使用最广泛的是RET和POST。还有其他方法（如DELETE和PUT），用来给向服务器请求的URL赋予意义。
- 许多主流Web技术只支持GET和POST，不少防火墙只允许GET和POST。使用X-HTTP-Method-Override标头来指定想要的HTTP方法可以规避这个限制，其实是再发送了一个POST请求
### eg:使用setRequestHeader("X-HTTP-Method-Override",<value)方法设置一个请求标头
（书32-6）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter32</title>
</head>

<body>
    <div>
        <button>Apples</button>
        <button>Lemons</button>
        <button>Bananas</button>
    </div>
   
    <div id="target">
        Press a Button
    </div>
    
    <script>
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;
        }
        
        var httpRequest;
        
        function handleButtonPress(e) {
          httpRequest=new XMLHttpRequest();
          httpRequest.onreadystatechange=handleResponse;
          httpRequest.open("GET",e.target.innerHTML+".html");
          httpRequest.setRequestHeader("X-HTTP-Method-Override","DELETE");//没反应
          httpRequest.send();
        }
        
        
        function handleResponse(e) {
           if (httpRequest.readyState==4&&httpRequest.status==200) {
                document.getElementById("target").innerHTML=httpRequest.responseText;
           }
        }
        
    
    </script>

</body>
</html>
```
- 说明：覆盖HTTP方法需要服务器端的Web应用程序框架能理解X-HTTP-Method-Override这个惯例。

## 32.4.2禁用内容缓存：Cache-Control标头
- 一些浏览器会缓存通过Ajax请求所获得的内容，在浏览器会话期间不会再请求它。此处例子意味着对Apples.html等上的改动不会立刻反映到浏览器中。
### eg:使用etRequestHeader("Cache-Control","no-cache")禁用内容缓存
（书32-7）
```
...
//只修改上例的function handleButtonPress(e)部分
    function handleButtonPress(e) {
          httpRequest=new XMLHttpRequest();
          httpRequest.onreadystatechange=handleResponse;
          httpRequest.open("GET",e.target.innerHTML+".html");
          httpRequest.setRequestHeader("Cache-Control","no-cache");
          httpRequest.send();
        }
...
```
## 32.4.5读取响应标头
- 通过getResponseHeader(<header>)和getAllResponseHeaders方法来读取服务器响应某个Ajax请求时发出的标头
- 标头是什么？ 标头是服务器在响应时首先发送回来的信息，故你可以在读取内容之前就先读取它们，在readyState变成2（即HEADERS_RECEIVED,表明已收到服务器响应的标头）时就可以读取。
### eg:读取响应标头
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter32</title>
    <style>
        #allheaders,#ctheader{
            border:medium solid black;
            padding:2px;
            margin:2px;
        }
    </style>
</head>

<body>
    <div>
        <button>Apples</button>
        <button>Lemons</button>
        <button>Bananas</button>
    </div>
    <div id="ctheader"></div>
    <div id="allheaders"></div>
    <div id="target">
        Press a Button
    </div>
    
    <script>
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;
        }
        
        var httpRequest;
        
        function handleButtonPress(e) {
          httpRequest=new XMLHttpRequest();
          httpRequest.onreadystatechange=handleResponse;
          httpRequest.open("GET",e.target.innerHTML+".html");
          httpRequest.send();
        }
        
        
        function handleResponse(e) {
           if (httpRequest.readyState==2) {
                document.getElementById("allheaders").innerHTML=httpRequest.getAllResponseHeaders();
                document.getElementById("ctheader").innerHTML=httpRequest.getResponseHeader("Content-Type");
            }
            else if (httpRequest.readyState==4&&httpRequest.status==200) {
                document.getElementById("target").innerHTML=httpRequest.responseText;
            }
        }
        
    
    </script>

</body>
</html>
```

## 32.5生成跨源Ajax请求
此节的学习需要打好Node.js基础，Node.js教程参见<http://www.runoob.com/nodejs/nodejs-tutorial.html>

- 默认情况下，浏览器限制脚本只能在她们所属文档的来源（所运行的脚本和请求的脚本所在的主机和端口号要相同）内生成Ajax请求。即从http://www.wetryer.com/webtest/Chapter32.html载入文档后，该文档内的脚本无法生成对http://www.wetryer.com:8080的请求。该策略的目的是降低**跨站脚本攻击**（简称CSS）
- **跨资源共享**（简称CORS）规范提供了一种合法的方式来生成跨源请求。
### eg:尝试生成跨源请求的html脚本
(书32-9）
- 该脚本尝试生成一个Ajax请求（http://www.wetryer.com:8080/Apples）
- 此html文件Chapter32.html放在wetyer服务器的D:/www.wetryer.com/webtest,通过在本地输入网址http://www.wetryer.com/webtest/Chapter32.html运行
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter32</title>
</head>

<body>
    <div>
        <button>Apples</button>
        <button>Lemons</button>
        <button>Bananas</button>
    </div>
    <div id="target">
        Press a Button
    </div>
    
    <script>
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;
        }
        
        var httpRequest;
        
        function handleButtonPress(e) {
          httpRequest=new XMLHttpRequest();
          httpRequest.onreadystatechange=handleResponse;
          httpRequest.open("GET","http://www.wetryer.com:8080/"+e.target.innerHTML);
          httpRequest.send();
        }
        
        
        function handleResponse(e) {
           if (httpRequest.readyState==2) {
                document.getElementById("allheaders").innerHTML=httpRequest.getAllResponseHeaders();
                document.getElementById("ctheader").innerHTML=httpRequest.getResponseHeader("Content-Type");
            }
            else if (httpRequest.readyState==4&&httpRequest.status==200) {
                document.getElementById("target").innerHTML=httpRequest.responseText;
            }
        }
        
    
    </script>

</body>
</html>
```
### eg:添加了跨源标头的Node.js脚本fruitselector.js
（书32-10、32-11）
- Access-Control-Allow-Origin标头指定了某个来源应当被允许对此文档生成跨源请求
- 此Node.js文件放在wetryer服务器的D盘，通过在服务器的cmd输入D:\>node fruitselector.js运行
```
var http=require('http');//使用 require 指令来载入 http 模块

http.createServer(
                  function(req,res){//createServer方法创建服务器，req,res参数方便接收和响应数据
                      console.log("[200]"+req.method+"to"+req.url);//console.log为在服务器的cmd窗口输出内容
//req.method返回上例httpRequest.open中的"GET"，req.url返回上例httpRequest.open中的e.target.innerHTML
                      res.writeHead(
                            200,"OK",{
                                "Content-Type":"text/html",
                                "Access-Control-Allow-Origin":"http://www.wetryer.com"
                            }
                        );
//res.writeHead（）方法：向请求的客户端发出响应头
                      res.write('<html><head><title>Fruit Total</title></head><body>');
                      res.write('<p>');
                      res.write('You seleted' +req.url.substring(1));
                      res.write('</p></body></html>');
//res.write写了要返回到html文档中的内容
                      res.end();
                    }
                ).listen(8080);
```
***有关http响应头等的相关知识待恶补！！***

## 32.5.2 s使用Origin请求标头
作为CORS的一部分，浏览器会给请求添加一个Origin标头以注明当前文档的来源，通过它可以灵活设置Access-Control-Allow-Origin标头的值。
### eg:使用 Origin请求标头
（书32-12）
```
var http=require('http');//使用 require 指令来载入 http 模块

http.createServer(
                  function(req,res){
                      console.log("[200]"+req.method+"to"+req.url);
                      
                      res.statusCode=200;
                      res.setHeader("Content-Type","text/html");
                      
                      var origin=req.headers["origin"];
                      if (origin.indexOf("www.wetryer.com")>-1) {
                        res.setHeader("Access-Control-Allow-Origin",origin);
                      }//只有在请求包含Origi标头，且值理由www.wetryer.com时才设置Access-Control-Allow-Origin响应标头。
              
                      res.write('<html><head><title>Fruit Total</title></head><body>');
                      res.write('<p>');
                      res.write('You seleted' +req.url.substring(1));
                      res.write('</p></body></html>');
                      res.end();
                    }
                ).listen(8080);
```

## 32.6终止请求
### eg:在服务器端node.js文件引入一段延迟
(书32-13）
```
var http=require('http');//使用 require 指令来载入 http 模块

http.createServer(
    function(req,res){
        console.log("[200]"+req.method+"to"+req.url);
        
        res.statusCode=200;
        res.setHeader("Content-Type","text/html");
        
        setTimeout(
            function(){
                var origin=req.headers["origin"];
                if (origin.indexOf("wetryer")>-1) {
                  res.setHeader("Access-Control-Allow-Origin",origin);
                }//只有在请求包含Origi标头，且值里有www.wetryer.com时才设置Access-Control-Allow-Origin响应标头。
        
                res.write('<html><head><title>Fruit Total</title></head><body>');
                res.write('<p>');
                res.write('You seleted' +req.url.substring(1));
                res.write('</p></body></html>');
                res.end();
            },
            10000//服务器接收到一个请求后，先写入初始的响应标头，暂停10s后再完成整个响应
        );
        
    }
).listen(8080);
```
### eg:含中止请求的html
(书32-14）
***浏览器不认识abort()啊？？？***
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter32</title>
</head>

<body>
    <div>
        <button>Apples</button>
        <button>Lemons</button>
        <button>Bananas</button>
    </div>
    <div>
        <button id="abortbutton">Abort</button>
    </div>
    <div id="target">
        Press a Button
    </div>
    
    <script>
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;
        }
        
        var httpRequest;
        
        function handleButtonPress(e) {
            if (e.target.id=="abortbutton") {
                httpRequest.abort();//XMLHttpRequest的abort方法：中止进行中的请求。正好服务器端也引入了10s延迟，有故有充足的时间可以执行它
            }
            else{
                httpRequest=new XMLHttpRequest();
                httpRequest.onreadystatechange=handleResponse;
                httpRequest.onabort=handleAbort;//abort事件发生时触发函数handelAbort来标明请求已被终止
                httpRequest.open("GET","http://www.wetryer.com:8080/"+e.target.innerHTML);
                httpRequest.send();
            }
        }
        
        
        function handleResponse(e) {
            if (httpRequest.readyState==4&&httpRequest.status==200) {
                document.getElementById("target").innerHTML=httpRequest.responseText;
            }
        }
        
        function handleAbort() {
            document.getElementById("target").innerHTML="Request Aborted";
        }
    
    </script>

</body>
</html>
```

## Node.js入门学习资料

<http://www.nodebeginner.org/index-zh-cn.html#responding-request-handlers-with-non-blocking-operations>