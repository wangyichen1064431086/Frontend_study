# Chapter35 使用canvas元素
## 35.1开始使用canvas元素
canvas元素的所有功能都体现在一个JavaScript对象HTMLLCanvasElement上，元素本身只有height、width两个属性。
### eg:使用带有基本备用内容的canvas元素
（书35-1）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        canvas{
            border: medium double black;
            margin: 4px;
        }
    </style>
</head>

<body>
    <canvas width="500" height-"200">
        Your browser does not support the <code>canvas</code> element.
    </canvas>

</body>
</html>
```
## 35.2 获取画布上下文对象
HTMLCanvasElement对象成员

成员|说明|返回
----|----|-----
height|对应于height属性|数值
width|对应于width属性|数值
getContext(<context>)|为画布返回上下文对象|对象

### eg:获取画布2d上下文对象
（书35-2）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        canvas{
            border: medium double black;
            margin: 4px;
        }
    </style>
</head>

<body>
    <canvas id="canvas" width="500" height-"200">
        Your browser does not support the <code>canvas</code> element.
    </canvas>
    
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
        //HTMLCanvasElement对象的getContext方法：为画布返回上下文
        ctx.fillRect(10,10,50,50);
        //.fillRect方法绘制实心矩形
    </script>

</body>
</html>
```

## 35.3绘制矩形
上下文对象的简单的图形方法

成员|说明|返回
-----|----|----
clearRect(x,y,w,h)|清除指定矩形|void
fillRect(x,y,w,h)|绘制一个实心矩形|void
strokeRect(x,y,w,h)|绘制一个空心矩形|void

x,y是canvas元素左上角算起的偏移量，w,h指定待绘矩形的宽度和高度
### eg:使用上下文对象的简单图形方法绘制简单矩形
（书35-3、35-4）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        canvas{
            border: medium double black;
            margin: 4px;
        }
    </style>
</head>

<body>
    <canvas id="canvas" width="500" height-"200">
        Your browser does not support the <code>canvas</code> element.
    </canvas>
    
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
       
        var offset=10;
        var size=50;
        var count=5;
        
        for(var i=0;i<count;i++){
            ctx.fillRect(i*(offset+size)+offset,offset,size,size);
            ctx.strokeRect(i*(offset+size)+offset,(2*offset)+size,size,size);
			 ctx.clearRect(i*(offset+size)+offset,offset+5,size,size-10);
        }//for循环排布尺寸的方法，Good!
    </script>

</body>
</html>
```
## 35.4设置画布绘制状态
上下文对象的基本绘制状态属性

名称|说明|默认值
----|----|------
fillStyle|获取或设置用于实心图形的样式|black
lineJoin|获取或设置线条与图形连接时的样式|miter
lineWidth|获取或设置线条的宽度|1.0
strokeStyle|获取或设置用于线条的样式|black


### eg:绘图前为上下文对象设置绘制状态
(书35-5）上例修改script部分
```
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
       
        var offset=10;
        var size=50;
        var count=5;
        
        for(var i=0;i<count;i++){
            ctx.lineWidth=2+2*i
            ctx.strokeRect(i*(offset+size)+offset,(2*offset)+size,size,size);
        }
    </script>
```

## 35.4.1lineJoin属性设置线条连接样式
### eg:设置lineJoin属性
```
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
       
        var offset=30;
        var size=50;
        var count=5;
        var linejoinvalue=['round','bevel','miter']
        for(var i=0;i<count;i++){
            ctx.lineWidth=20;
            ctx.lineJoin=linejoinvalue[i%3];//%求余
            ctx.strokeRect(i*(offset+size)+offset,offset,size,size);
        }
    </script>
```

## 35.4.2 设置填充和笔触样式
### eg:用上下文对象的fillStyle和strokeStyle属性设置颜色
(书35-7）
```
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
       
        var offset=30;
        var size=50;
        var count=5;
        ctx.lineWidth=3;
        var fillColors=["black","grey","lightgrey","red","blue"];
        var strokeColors=["rgb(0,0,0)","rgb(100,100,100)",
                          "rgb(200,200,200)","rgb(255,0,0)","rgb(0,0,255)"];
        
        for(var i=0;i<count;i++){
            ctx.fillStyle=fillColors[i];
            ctx.strokeStyle=strokeColors[i];
            
            ctx.fillRect(i*(offset+size)+offset,offset,size,size);
            ctx.strokeRect(i*(offset+size)+offset,(2*offset)+size,size,size);
        }
    </script>
```

## 35.4.3使用渐变
上下文对象的渐变方法
名称|说明|返回
----|----|-----
createLinearGradient(x0,y0,x1,y1)|创建线性渐变|CanvasGradient对象
createRadialGradient(x0,y0,r0,x1,y1,r1)|创建径向渐变|CanvasGradient对象

CanvasGradient对象的方法

名称|说明|返回
----|----|----
addColorStop(<position>,<color>)|给渐变的梯度线添加一种纯色|void

### eg1:创建线性渐变
(书35-8）
```
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
       
        var grad=ctx.createLinearGradient(0,0,500,140);
        grad.addColorStop(0,"red");
        grad.addColorStop(0.5,"white");
        grad.addColorStop(1,"black");
        
        ctx.fillStyle=grad;
        ctx.fillRect(0,0,500,140);
    </script>
```
### eg2:使图形匹配渐变
(书35-11）
- 上下文对象方法createLinearGradient参数是值画布里的一组坐标，表示渐变颜色的起止位置
- 上下文对象方法fillRect参数值代表了矩形相对于单个坐标点的高度和宽度。
```
  <script>
        var ctx=document.getElementById("canvas").getContext("2d");
       
        var grad=ctx.createLinearGradient(10,10,60,60);
        grad.addColorStop(0,"red");
        grad.addColorStop(0.5,"white");
        grad.addColorStop(1,"black");
        
        ctx.fillStyle=grad;
        ctx.fillRect(10,10,50,50);
    </script>
```

## 35.4.4使用径向渐变
eg:使用上下文对象的createRadialGradient方法
（书35-12）
```
   <script>
        var ctx=document.getElementById("canvas").getContext("2d");
       
        var grad=ctx.createRadialGradient(250,70,20,200,60,100);
        grad.addColorStop(0,"red");
        grad.addColorStop(0.5,"white");
        grad.addColorStop(1,"black");
       
        ctx.fillStyle=grad;
        ctx.fillRect(0,0,500,140);
    </script>
```
## 35.5保存和恢复绘制状态
上下文对象保存和恢复状态的方法

值|说明
---|---
save()|保存绘制状态属性的值，并把它们推入状态栈
restore()|取出状态栈的第一组值，用它们来设置绘制状态

- 画布里内容不会被保存或恢复，只有绘制状态的属性值才会。包括lineWidth、fillStyle、strokeStyle
### eg:使用上下文对象的save（）和restore()方法保存和恢复状态***Good!***
（书35-16）
```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style>
            canvas {border: thin solid black; margin: 4px}
        </style>
    </head>
    <body>
        <canvas id="canvas" width="500" height="140">
            Your browser doesn't support the <code>canvas</code> element
        </canvas>
        <div>
            <button>Save</button>
            <button>Restore</button>
        </div>
        <script>
            var ctx = document.getElementById("canvas").getContext("2d");
            
            var grad=ctx.createLinearGradient(500,0,500,140);
            grad.addColorStop(0,"red");
            grad.addColorStop(0.5,"white");
            grad.addColorStop(1,"black");
            
            var colors=["black",grad,"red","green","yellow","black","grey"];
            
            var cIndex=0;
            
            ctx.fillStyle=colors[cIndex];
            draw();
            
            var buttons=document.getElementsByTagName("button");
            for(var i=0;i<buttons.length;i++){
                buttons[i].onclick=handleButtonPress;
            }
            
            function handleButtonPress(e) {
                switch (e.target.innerHTML) {
                    case 'Save':
                        ctx.save();//保存当前的绘制状态
                        cIndex=(cIndex+1)%colors.length;
                        ctx.fillStyle=colors[cIndex];//设置一个新的状态
                        draw();//使用新状态绘出实心矩形
                        break;
                    case 'Restore':
                        cIndex=Math.max(0,cIndex-1);//调整cIndex值为上一个值，使用Math.max()函数处理cIndex为0的情况
                        ctx.restore();//上一个绘制状态被恢复，即上述save()保存的状态
                        draw();
                        break;
                }
            }
            
            function draw() {
                ctx.fillRect(0,0,500,140);
            }
        </script>
    </body>
</html>

```

## 35.6绘制图像
- 使用上下文对象的drawImage方法。第一个参数为图像来源，剩下的可以是2、4、8个参数
### eg:使用drawImage方法 ***Chrome不支持，得用FireFox***
（书35-17）
```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style>
            canvas {border: thin solid black; margin: 4px}
        </style>
    </head>
    <body>
        <canvas id="canvas" width="500" height="140">
            Your browser doesn't support the <code>canvas</code> element
        </canvas>
        <img id="banana" hidden src="banana-small.png"/>
        
        <script>
            var ctx=document.getElementById("canvas").getContext("2d");
            var imageElement=document.getElementById("banana");
            
            ctx.drawImage(imageElement,10,10);
            ctx.drawImage(imageElement,120,10,100,120);
            ctx.drawImage(imageElement,20,20,100,50,250,10,100,120);
        </script>
    </body>
</html>

```

## 35.6.1使用视频图像
### eg1:使用视频作为drawImage方法的来源
（书35-18）
```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style>
            canvas {
                border: thin solid black;
                margin: 4px;
            }
            body>*{
                float: left;
            }
        </style>
    </head>
    <body>
        <video id="vid" src="timessquare.webm" controls preload="auto" width="360" height="240">
            Video cannot be displayed
        </video>
        <div>
            <button id="pressme">Snapshot</button>
        </div>
        <canvas id="canvas" width="360" height="240">
            Your browser doesn't support the <code>canvas</code> element
        </canvas>
        <img id="banana" hidden src="banana-small.png"/>
        
        <script>
            var ctx=document.getElementById("canvas").getContext("2d");
            var imageElement=document.getElementById("vid");
            
            document.getElementById("pressme").onclick=function(){
                ctx.drawImage(imageElement,0,0,360,240);
            }
        </script>
    </body>
</html>
```
### eg2:用canvas显示视频并在上面绘制一个计时器***Good!***
(书35-19）
- 需注意这类技巧会使CPU负载很大
```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style>
            canvas {
                border: thin solid black;
                margin: 4px;
            }
            body>*{
                float: left;
            }
        </style>
    </head>
    <body>
        <video id="vid" src="timessquare.webm" hidden preload="auto"
               width="360" height="240" autoplay>
        </video>
       
        <canvas id="canvas" width="360" height="240">
            Your browser doesn't support the <code>canvas</code> element
        </canvas>
 
        <script>
            var ctx=document.getElementById("canvas").getContext("2d");
            var imageElement=document.getElementById("vid");
            
            var width=100;
            var height=10;
            ctx.lineWidth=5;
            ctx.strokeStyle="red";
            
            setInterval(//创建计时器setInterval(<function>,<time>):每隔time毫秒调用指定的函数
                function(){
                    ctx.drawImage(imageElement,0,0,360,240);
                    ctx.strokeRect(180-(width/2),120-(height/2),width,height);
                },
                25
            );//该计时器每25ms触发一次，绘制当前的视频帧（这样就实现的视频的播放），并添上一个空心矩形
            
            setInterval(
                function () {
                    width=(width+1)%200;
                    height=(height+3)%200;
                },
                100
            );//该计时器每100ms触发一次，改变绘制矩形所有的值
        </script>
    </body>
</html>

```

## 36.2使用画布图像
### eg:将画布作为drawImage方法的源
(书35-20）
```
<!DOCTYPE HTML>
<html>
    <head>
        <title>Example</title>
        <style>
            canvas {
                border: thin solid black;
                margin: 4px;
            }
            body>*{
                float: left;
            }
        </style>
    </head>
    <body>
        <video id="vid" src="timessquare.webm" hidden preload="auto"
               width="360" height="240" autoplay>
        </video>
        
        <canvas id="canvas" width="360" height="240">
            Your browser doesn't support the <code>canvas</code> element
        </canvas>
        <div>
            <button id="pressme">Press Me</button>
        </div>
        
        <canvas id="canvas2" width="360" height="240">
            Your browser does not support the <code>canvas</code> element
        </canvas>
       
        
        <script>
            var srcCanvasElement=document.getElementById("canvas");
            var ctx=srcCanvasElement.getContext("2d");
            var ctx2=document.getElementById("canvas2").getContext("2d");
            var imageElement=document.getElementById("vid");
            
            document.getElementById("pressme").onclick=takeSnapshot;
            
            var width=100;
            var height=10;
            ctx.lineWidth=5;
            ctx.strokeStyle="red";
            ctx2.lineWidth=30;
            ctx2.strokeStyle="black";
            
            setInterval(
                function(){
                    ctx.drawImage(imageElement,0,0,360,240);
                    ctx.strokeRect(180-(width/2),120-(height/2),width,height);
                },
                10
            );
            
            setInterval(
                function () {
                    width=(width+1)%200;
                    height=(height+3)%200;
                },
                100
            );
            
            function takeSnapshot() {
                ctx2.drawImage(srcCanvasElement,0,0,360,240);
                ctx2.strokeRect(0,0,360,240);
            }
        </script>
    </body>
</html>

```