# Chapter34 使用多媒体
## 34.1使用video元素
### eg:使用video元素
（书34-1）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
</head>

<body>
    <video width="360" height="240" src="timessquare.webm"
           autoplay controls preload="none" muted>
        <!--autoplay属性：使浏览器尽可能立刻开始播放视频-->
        <!-- controls属性：除非此属性存在，否则浏览器不会显示播放控件-->
        <!-- preload属性：告诉浏览器是否预先载入视频-->
        <!-- muted属性：此属性使视频一开始就处于静音状态-->
        Video cannot be displayed.
    </video>

</body>
</html>
```

## 34.1.1preload属性：预先加载视频
preload属性
值|说明
---|-----
none|用户开始播放之前不会载入视频
metadata|用户开始播放之前只载入视频元数据（宽度、高度、第一帧、长度等）
auto|请求浏览器尽快下载整个视频

- 决定是否预先加载视频时，应当权衡用户想要观看视频的可能性和自动载入视频内容所需要的带宽
### 34-2
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
</head>

<body>
    <video width="360" height="240" src="timessquare.webm"
          controls preload="none" muted>
        <!--autoplay属性：使浏览器尽可能立刻开始播放视频-->
        <!-- controls属性：除非此属性存在，否则浏览器不会显示播放控件-->
        <!-- preload属性：告诉浏览器是否预先载入视频-->
        <!-- muted属性：此属性使视频一开始就处于静音状态-->
        Video cannot be displayed.
    </video>
    
       <video width="360" height="240" src="timessquare.webm"
          controls preload="metadata" muted>
        Video cannot be displayed.
    </video>

</body>
</html>
```
- 注意：浏览器会自由选择是否忽略preload属性所表达的偏好。如需限制带宽消耗，poster属性可能会更胜任

## 34.1.2poster属性：显示占位图像
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
</head>

<body>
    <video width="360" height="240" src="timessquare.webm"
          controls preload="none" poster="poster.png">
        Video cannot be displayed.
    </video>
    
</body>
</html>
```

## 34.1.3width、height设置视频尺寸
- 当width、height比例失衡时，浏览器为了保持屏幕的长宽比，只会把部分空间派给视频
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        video{
            background-color: lightgrey;
            border: medium double black;
        }
    </style>
</head>

<body>
    <video width="700" height="500" src="timessquare.webm"
          controls preload="auto" poster="poster.png">
        Video cannot be displayed.
    </video>
    
</body>
</html>
```

## 34.1.4source元素指定视频来源
- 没有那种格式能用于所有主流浏览器，你必须以多种格式编码视频
- 浏览器会沿着source元素列表寻找它能够播放的视频文件
- 浏览器判断它是否能播放某个视频的依据之一是**服务器返回的MIME类型**，可以通过给source元素应用type属性来提示用户
- media属性向浏览器指明该视频最适合在哪种设备上播放。详见Chapter7
### eg:用source元素及其type属性
（书34-6、34-7）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
</head>

<body>
    <video width="700" height="500" src="timessquare.webm"
          controls>
        <source src="timessquare.webm" type="video/webm"/>
        <source src="timessquare.ogv" type="video/ogg"/>
        <source src="timessquare.mp4" type="video/mp4"/>
        Video cannot be displayed.
    </video>
    
</body>
</html>
```

## 34.2使用audio元素
- 注意：不要这么做：应用autoplay属性并忽略controls属性，这样会让音频自动播放而用户无法停止它
### eg:使用audio元素
(书34-8)
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
</head>

<body>
   <audio controls src="mytrack.mp3" autoplay>
       Audio content cannot be played
   </audio>
    
</body>
</html>
```
### eg:用source元素提供多种音频格式
(书34-9）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
</head>

<body>
   <audio controls autoplay>
        <source src="mytrack.ogg"/>
        <source src="mytrack.mp3"/>
        <source src="mytrack.wav"/>
       Audio content cannot be played
   </audio>
    
</body>
</html>
```

## 34.3通过DOM操作嵌入式媒体
- HTMLMediaElement对象在DOM里为audio和video统一定义了核心功能
- audio元素在DOM里由HTMLAudioElement对象所代表，无HTMLMediaElement的额外功能。video元素由HTMLVideoElement对象代表，它定义了一些额外属性
## 34.3.1获得媒体信息
### eg:使用HTMLMediaElement对象的基本成员获取媒体元素的基本信息
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        table{
            border: thin solid black;
            border-collapse: collapse;
        }
        th,td{
            padding: 3px 4px;
        }
        body>*{
            float: left;
            margin: 2px;
        }
    </style>
</head>

<body>
    <video id="media" width="360" height="240" src="timessquare.webm" controls preload="metadata">
        <source src="timessquare.webm" type="video/webm"/>
        <source src="timessquare.ogv" type="video/ogg"/>
        <source src="timessquare.mp4" type="video/mp4"/>
        Video cannot be displayed.
    </video>
    
    <table id="info" border="1">
        <tr>
            <th>Property</th><th>Value</th>
        </tr>
    </table>
    
    <script>
        var mediaElem=document.getElementById("media");
        var tableElem=document.getElementById("info");
        
        var propertyNames=["autoplay","currentSrc","controls","loop","muted","preload","src","volumn"];
        
        for(var i=0;i<propertyNames.length;i++){
            tableElem.innerHTML+="<tr><td>"+propertyNames[i]+"</td><td>"+mediaElem[propertyNames[i]]+"</td><tr>";
        }
    </script>
</body>
</html>
```
## 34.3.2canPlayType属性评估播放能力
canPlayType属性所允许的值
值|说明
---|---
""（空字符串）|浏览器无法播放该媒体类型
maybe|浏览器也许可以播放
probably|浏览器相当有把握可以播放
### eg:使用canPlayType方法***Good***
（书34-11）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        table{
            border: thin solid black;
            border-collapse: collapse;
        }
        th,td{
            padding: 3px 4px;
        }
        body>*{
            float: left;
            margin: 2px;
        }
    </style>
</head>

<body>
    <video id="media" width="360" height="240" src="timessquare.webm" controls preload="metadata">
        Video cannot be displayed.
    </video>
    
    <table id="info" border="1">
        <tr>
            <th>Property</th><th>Value</th>
        </tr>
    </table>
    
    <script>
        var mediaElem=document.getElementById("media");
        var tableElem=document.getElementById("info");
        
        var mediaFiles=["timessquare.webm","timessquare.ogv","timessquare.mp4"];
        var mediaTypes=["video/webm","video/ogv","video/mp4"];
        for (var i=0;i<mediaTypes.length;i++) {
            var playable=mediaElem.canPlayType(mediaTypes[i]);
            
            if (!playable) {
                playable="no";
            }
            
            tableElem.innerHTML+="<tr><td>"+mediaTypes[i]+"</td><td>"+playable+"</td></tr>";
            
            if (playable=="probably") {
                mediaElem.src=mediaFiles[i];//设置src属性的值
            }
        }
        
    </script>
</body>
</html>
```

## 34.3.3控制媒体播放
HTMLMediaElement对象的播放成员

成员|说明|返回
-----|-----|----
currentTime|返回媒体当前的播放点|数值
duration|返回媒体总时长|数值
ended|如果媒体播放完毕则返回true|布尔值
pause()|暂停媒体播放|void
paused|如果暂定就返回true,否则为false|布尔值
play()|开始播放媒体|void
### eg:用HTMLMediaElement属性获取媒体播放详情
（书34-12）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        table{
            border: thin solid black;
            border-collapse: collapse;
        }
        th,td{
            padding: 3px 4px;
        }
        body>*{
            float: left;
            margin: 2px;
        }
        div{
            clear: both;/*clear属性：规定元素那一侧不允许浮动元素*/
        }
    </style>
</head>

<body>
    <video id="media" width="360" height="240"
        controls preload="metadata">
        <source src="timessquare.webm"/>
        <source src="timessquare.ogv"/>
        <source src="timessquare.mp4"/>
        Video cannot be displayed.
    </video>
    
    <table id="info" border="1">
        <tr>
            <th>Property</th><th>Value</th>
        </tr>
    </table>
    
    <div>
        <button id="pressme">Press Me</button>
    </div>
    
    <script>
        var mediaElem=document.getElementById("media");
        var tableElem=document.getElementById("info");
        
        document.getElementById("pressme").onclick=displayValues;
        //按下后，会显示出四个属性的当前值
        
        displayValues();//加载后就先显示四个属性的初始值
        
        function displayValues() {
            var propertyNames=["currentTime","duration","paused","ended"];
            tableElem.innerHTML="";
            for(var i=0;i<propertyNames.length;i++){
                tableElem.innerHTML+="<tr><td>"+propertyNames[i]+"</td><td>"+mediaElem[propertyNames[i]]+"</td></tr>";
            }
        }
        
    </script>
</body>
</html>
```
### eg:用播放方法代替媒体控件controls***VeryGood! switch```case的用法***
- 去掉video元素的controls属性，用点击button按钮触发的play和pause方法来启动和停止媒体回放。
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
</head>

<body>
    <video id="media" width="360" height="240" preload="auto"><!-- 省略controls属性 -->
        <source src="timessquare.webm"/>
        <source src="timessquare.ogv"/>
        <source src="timessquare.mp4"/>
        Video cannot be displayed.
    </video>

    
    <div>
        <button>Play</button>
        <button>Pause</button>
    </div>
    
    <script>
        var mediaElem=document.getElementById("media");
        
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;
        }
        
        function handleButtonPress(e) {
            switch (e.target.innerHTML) {
                case 'Play':
                    mediaElem.play();
                    break;
                case 'Pause':
                    mediaElem.pause();
                    break;
            }
        }
        
    </script>
</body>
</html>
```