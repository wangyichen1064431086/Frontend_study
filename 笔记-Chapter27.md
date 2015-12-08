# Chapter27 使用Window对象
## 27.1获取Window对象
两种方法：<br>
1. window（.窗口属性）
2. document.defaultView（.窗口属性）
### eg:两种方法获取window对象
（书27-1）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
</head>

<body id="bod">
    <table>
        <tr>
            <th>outerWidth:</th><td id="owidth"></td>
        </tr>
        <tr>
            <th>outerHeight:</th><td id="oheight"></td>
        </tr>
    </table>
    
    <script type="text/javascript">
        document.getElementById("owidth").innerHTML=window.outerWidth;
            //使用全局变量window来获取window对象
        document.getElementById("oheight").innerHTML=document.defaultView.outerHeight;
            //使用在Document对象上加defaultView属性
            
            //这里用以上两种方式，用Window对象来读取outerWidth和outerHeight属性值
    </script>
</body>
</html>

```

## 27.2获取窗口信息
- 就HTML而言，浏览器窗口里的标签页被视为窗口本身
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
    <style>
        table{
            border-collapse:collapse;
            border:thin solid black;
        }
    </style>
</head>

<body>
    <table border="1">
        <tr>
            <th>outerWidth:</th><td id="ow"></td>
            <th>outerHeight:</th><td id="oh"></td>
        </tr>
        <tr>
            <th>innerWidth:</th><td id="iw"></td>
            <th>innerHeight:</th><td id="ih"></td>
        </tr>
        <tr>
            <th>screen.width:</th><td id="sw"></td>
            <th>screen.height:</th><td id="sh"></td>
        </tr>
    </table>
    
    
    <script type="text/javascript">
        document.getElementById("ow").innerHTML=window.outerWidth;//获取窗口高度，包括边框和菜单栏等
        document.getElementById("oh").innerHTML=window.outerHeight;//获取窗口宽度，包括边框和菜单栏等
        document.getElementById("iw").innerHTML=window.innerWidth;//获取窗口内容区域高度
        document.getElementById("ih").innerHTML=window.innerHeight;//获取窗口内容区域宽度
        document.getElementById("sw").innerHTML=window.screen.width;//返回一个描述屏幕的Screen对象
        document.getElementById("sh").innerHTML=window.screen.height;
    </script>
</body>
</html>
```

## 27.3 与窗口进行交互
- window对象有提供一组方法，可以用它们与包含文档的窗口进行交互
window交互功能

名称|说明|返回
----|----|----
blur()|让窗口失去焦点|void
close()|关闭窗口|void
focus()|让窗口获得键盘焦点|void
print()|提示用户打印页面|woid
scrollBy(<x>,<y>)|让文档相对于当前位置进行滚动|void
scrollTo(<x>,<y>)|让文档滚动到指定位置|void
stop()|停止载入文档|void
### eg：与窗口进行交互
（书27-3）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
</head>

<body>
    <p>
        <button id="scroll">Scroll</button>
        <button id="print">Print</button>
        <button id="close">Close</button>
    </p>
    <p>
        There are lots of different kinds of fruit.
        adafd da fadg qg qga.
        adfadf  AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.
        AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
        
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa.
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbb.
        cccccccccccccccccccccccccccccccccccccccccccccccccccccc.
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa.
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa.
        sxxxxxxxxxxxxxxxfajaalllllllllllllllllllllllllllllllllllllll.
        aaaaaaaaaaaaaaaaoooooooooooooooooooooooooooooooooooooooooooo.
        kkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkk.
    </p>
    <script>
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++)
        {
            buttons[i].onclick=handleButtonPress;
        }
        
        function handleButtonPress(e)
        {
            if (e.target.id=="print")
            {
                window.print();//提示用户打印页面
            }
            else if (e.target.id=="close")
            {
                window.close();//关闭窗口
            }
            else
            {
                window.scrollTo(400,400);//让文档滚动到指定位置
            }
        }
    </script>
</body>
</html>
```

## 27.4 window对象中一些对用户进行提示的方法
方法名称|说明|返回
--------|----|----
alert(< msg>)|向用户显示一个对话框并等候其被关闭|void
confirm(< msg>)|显示一个带有确认和取消提示的对话框|布尔值
prompt(< msg>,< val>)|显示对话框提示用户输入一个值|字符串（就是输入的值）
showModalDialog(< url>)|弹出指定窗口，显示指定的URL|void

### eg:对用户进行提示 ***Good常用***
（书27-4）
- alert()是window.alert()的简写
- window方法（如window.confirm()）之前加上var a=window.confirm("xx"),函数会正常运行，然后a 为函数返回值

```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
</head>

<body>
  <button id="alert">Alert</button>
  <button id="confirm">Confirm</button>
  <button id="prompt">Prompt</button>
  <button id="modal">Modal Dialog</button>
  
  <script type="text/javascript">
    var buttons=document.getElementsByTagName("button");
    for(var i=0;i<buttons.length;i++)
    {
        buttons[i].onclick=handleButtonPress;
    }
    
    function handleButtonPress(e)
    {
        if (e.target.id=="alert")
        {
            window.alert("This is an alert");
        }
        else if (e.target.id=="confirm")
        {
            var confirmed=window.confirm("This is a confirm- do you want to proceed?");
            alert("Confirmed?"+confirmed);//上述对话框点击“确认”，则confirmed=true;点“取消”，则confirmed=false
        }
        else if (e.target.id=="prompt")
        {
            var response=window.prompt("Enter a word","hello");//hello为默认输入值
            alert("The word was "+ response);//response为输入后的值
        }
        else if (e.target.id=="modal")//此方法被限用了
        {
            window.showModalDialog("http://apress.com")
        }
    }
  </script>
  
</body>
</html>
```

## 27.5 window对象获取基本信息
提供信息的对象属性
名称|说明|返回
----|----|----
document|返回与此窗口关联的Document对象|Document
history|提供对浏览器历史的访问|History
location|提供当前文档地址的详细信息|Location

- Document对象详见Chapter26
- Window.location属性返回的Location对象和Document.location属性返回的相同
- window.history对象详情见下

## 27.6 使用浏览器历史 ***此一章都等服务器弄好后重看***
## 27.6.1在浏览历史中导航
### eg1: 在浏览器历史中导航
(书27-5）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
</head>

<body>
  <button id="back">Back</button>
  <button id="forward">Forward</button>
  <button id="go">Go</button>

  <script type="text/javascript">
    var buttons=document.getElementsByTagName("button");
    for(var i=0;i<buttons.length;i++)
    {
        buttons[i].onclick=handleButtonPress;
    }
    
    function handleButtonPress(e)
    {
        if (e.target.id=="back")
        {
           window.history.back();//在浏览历史中后退一步（只能是本页面的浏览历史）
        }
        else if (e.target.id=="forward")
        {
            window.history.forward();//在浏览历史中前进一步
        }
        else if (e.target.id=="go")
        {
            window.history.go("http://www.apress.com");//转到相对于当前文档的某个浏览历史位置--(不灵???)，也可为.go(+1)/go(-2）go("+1")/go("-2"）
        }
     
    }
  </script>
  
</body>
</html>
```
### eg2:点击按钮改变内容 ***此例和浏览历史处理没什么关联，但也确实很有用的小代码段***
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
</head>

<body>
  <p id="msg"></p>
  <button id="banana">Banana</button>
  <button id="apple">Apple</button>
  
  <script>
        var sel="No selection made";
        document.getElementById("msg").innerHTML=sel;
        
        var buttons=document.getElementsByTagName("button");
        
        for(var i=0;i<buttons.length;i++)
        {
            buttons[i].onclick=function(e)
            {
                document.getElementById("msg").innerHTML=e.target.innerHTML;
            }
        }
  </script>
  
</body>
</html>
```

## 27.6.2在浏览历史里插入条目
### eg window.history.pushState为浏览器历史添加条目 
 ***待用服务器和document.location相关知识一起重看***

（书27-7）
本机会报错：<br>
'file:///G:/Web%E5%89%8D%E7%AB%AF%E5%AD%A6%E4%B9%A0/%E5%89%8D%E7%AB%AF%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/Chapter27.html?banana' cannot be created in a document with origin 'null'.<br>
服务器不会


```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
</head>

<body>
  <p id="msg"></p>
  <button id="banana">Banana</button>
  <button id="apple">Apple</button>
  
  <script type="text/javascript">
        var sel="No selection made";
        if (window.location.search=="?banana")
        {
            sel="Selection:Banana";
        }
        else if (window.location.search=="?apple")//用window.location对象来读取查询字符串和所选择的值
        {
            sel="Selection:Apple";
        }
        document.getElementById("msg").innerHTML=sel;
        
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++)
        {
            buttons[i].onclick=function(e)
            {
                document.getElementById("msg").innerHTML=e.target.innerHTML;
                window.history.pushState("","","?"+e.target.id);
                         //使用history.pushState方法向浏览器历史添加条目
                         //此处是当前URL加上一个标识用户电击了哪个按钮的查询字符串
                         //上述用window.location对象来读取查询字符串和所选择的值
        {
            }
        }
        
       
  </script>
  
</body>
</html>
```

eg5 使用history.replaceState方法替换浏览器历史中的当前条目
上述代码最后一行改为
```
 window.history.replaceState("","","otherpage27.html?"+e.target.id);

```


## 27.6.3 跨文档消息传递
- 通常情况下，不同来源（“origins”）的脚本是不允许通信的，但这一需求很强烈
- 理解脚本的来源：浏览器通过URL的各个组成部分来判断某个资源的来源。如果两个脚本的协议(http://)、主机名（www.wetyer.com）、端口号（80 or 81）相同，就被认为是拥有同一个来源——即使URL的其他部分不一样也是如此。

说明：此处例子运行仍然有错误，等30章看完后再来看

文档1(放在本机）：定位Window对象并调用postMessage方法
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
</head>

<body>
  <p id="status">Ready</p>
  <button id="send">Send Message</button>
  <p>
    <iframe name="nested" src="http://www.wetryer.com/webtest/otherdomain.html" width="90%" height="75px">
        <!--iframe元素用于载入一个不同来源的文档-->
    </iframe>
  </p>
  
  <script>
    document.getElementById("send").onclick=function()
    {
        window["nested"].postMessage("I like apples","http://www.wetyer.com");
        //window["nested"]:找到window目标对象,该目标对象包含想要发送消息的脚本
        //postMessage方法：两个参数分别为——想要发送的信息、目标脚本的来源
        document.getElementById("status").innerHTML="Message Sent";
    }
  </script>
  
</body>
</html>
```

文档2（放在服务器）：监听message事件
```
<!DOCTYPE html>

<html>
<head>
    <title>Other Page</title>
</head>

<body>
    <h1 id="banner">This is the nested document</h1>
    <script>
        window.addEventListener("message",receiveMessage,false);//addEventListener方法用于向指定元素添加事件句柄
        
        function receiveMessage(e)
        {
            if (e.origin=="http://www.wetyer.com")
            {
                displayMessage(e.data);
            }
            else
            {
                displayMessage("Message　Discarded");
            }
        }
        
        function diplayMessage(msg)
        {
            document.getElementById("banner").innerHTML=msg;
        }
    </script>



</body>
</html>
```

## 27.8 使用计时器
- Window对象提供的一个有用功能是可以设置计时器。计时器们被用于在预设的时间段后执行某个函数
- 计时方法如下表:

名称|说明|返回
----|----|-----
clearInterval(<计时器名称>)|撤销某个时间间隔计时器| void
clearTimeout(<计时器名称>)|撤销某个超时计时器|void
setInterval(<function>,<time>)|创建一个计时器，每隔time毫秒调用指定函数|指定值
setTimeout(<function>,<time>)|创建一个计时器，等待time毫秒后调用指定函数|指定增值
### eg:使用计时器方法  ***Very Good***
(书27-16）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
</head>

<body>
    <p id="msg"></p>
    <p>
        <button id="settime">Set Time</button>
        <button id="cleartime">Clear Time</button>
        <button id="setinterval">Set Interval</button>
        <button id="clearinterval">Clear Interval</button>
    </p>
    
    <script>
        var buttons=document.getElementsByTagName("button");
        for (var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;
        }
        
        var timeID;
        var intervalID;
        var count=0;
        
        function handleButtonPress(e) {
            if (e.target.id=="settime"){
                timeID=window.setTimeout(    //window.setTimeout(<function>,<time>) 创建一个计时器，等待time毫秒后调用指定函数
                    function(){
                        displayMsg("Timeout Expired");
                    },
                    5000
                );
                displayMsg("Timeout Set");
            }
            else if (e.target.id=="cleartime"){
                window.clearTimeout(timeID);   //window.clearTimeout(<计时器名称>):撤销某个超时计时器
                displayMsg("Timeout Cleared");
            }
            else if (e.target.id=="setinterval"){
                intervalID=window.setInterval(  //window.setInterval(<function>,<time>):创建一个计时器，每隔time毫秒调用制定函数
                    function(){
                        displayMsg("Interval expired. Counter:"+count++);
                    },
                    2000
                );
                displayMsg("Interval Set");   
            }
            else if (e.target.id=="clearinterval") {
                window.clearInterval(intervalID);//window.clearInterval(<计时器名称>):撤销某个时间间隔计时器
                displayMsg("Interval Cleared");
            }
        }
        
        function displayMsg(msg) {
            document.getElementById("msg").innerHTML=msg;//该函数设置id="msg"的p元素的值
        }
    </script>
 
  
</body>
</html>

```
### 自己理解的简写版：
- 在setInterval函数中一定注意是function(){showcondition(count++);}，不是showcondition(count++)***？？还有待加强理解***
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter27</title>
</head>

<body>
    <p id="msg"></p>
    <p>
        <button id="setinterval">Set Interval</button>
        <button id="clearinterval">Clear Interval</button>
    </p>
    
    <script>
        var count=0;
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=functime;
        }
        
        function functime(e) {
            if (e.target.id=="setinterval") {
               var revalue=window.setInterval(function(){showcondition(count++);},1000);
//这里一定注意是function(){showcondition(count++);}，不是showcondition(count++)？？***还有待加强理解***
            }
            else if (e.target.id=="clearinterval") {
                window.clearInterval(revalue);
                showcondition(count);
            }
        }
        
        function showcondition(content) {
            document.getElementById("msg").innerHTML=content;
        }
    </script>

</body>
</html>
```

一个好的计时器应用<http://bbs.csdn.net/topics/380133035>
