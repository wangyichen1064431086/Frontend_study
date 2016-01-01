# Chapter36 使用canvas元素（二）

## 36.1 用路径绘图
上下文对象的基本路径方法
名称|说明|返回
----|----|----
beginPath()|开始一条新路径|void
closePath()|尝试闭合现有路径，方法是绘制一条线，连接最后那条线的终点与初始坐标|void
fill()|填充用子路径描述的图形|void
isPointInPath(x,y)|如果指定的点在当前路径所描述的图形之内则返回true|布尔值
lineTo(x,y)|绘制一条到指定坐标的子路径|void
moveTo(x,y)|移动到指定坐标而不绘制子路径|void
rect(x,y,w,h)|绘制一个矩形|void
stroke()|给子路径描述的图形绘制轮廓线|void

## 36.1.1 用线条绘制路径
### eg1:由直线创建路径
(书36-1）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        canvas{
            border: thin solid black;
        }
        body>*{
            float: left;
        }
    </style>
</head>

<body>
    <canvas id="canvas" width="500" height="140">
        Your broser doesn't support the <code>canvas</code> element.
    </canvas>
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="yellow";
        ctx.strokeStyle="black";
        ctx.lineWidth=4;
        
        ctx.beginPath();
        ctx.moveTo(10,10);
        ctx.lineTo(110,10);
        ctx.lineTo(110,120);
        ctx.closePath();
        ctx.fill();
        
        ctx.beginPath();
        ctx.moveTo(150,10);
        ctx.lineTo(200,10);
        ctx.lineTo(200,120);
        ctx.lineTo(190,120);
        
        ctx.fill();
        ctx.stroke();
        
        ctx.beginPath();
        ctx.moveTo(250,10);
        ctx.lineTo(250,120);
        ctx.stroke();
    </script>

</body>
</html>
```

### eg2:设置lineCap属性
（书36-2）
- 上下文对象的lineCap属性可设置线条始端样式，有三个值：butt、round、square
```
<script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="red";
        ctx.lineWidth="2";
        ctx.beginPath();
        ctx.moveTo(0,50);
        ctx.lineTo(200,50);
        ctx.stroke();
        
        ctx.strokeStyle="black";
        ctx.lineWidth=40;
        
        var xpos=50;
        var styles=["butt","round","square"];
        for(var i=0;i<styles.length;i++){
            ctx.beginPath();
            ctx.lineCap=styles[i];//lineCap属性设置线条始端样式
            ctx.moveTo(xpos,50);
            ctx.lineTo(xpos,150);
            ctx.stroke();
            xpos+=50;
        }
    </script>
```

## 36.1.2绘制矩形
- 上下文对象的rect方法会为当前路径添加一条矩形的子路径。如果只单独需要一个路径，Chapter35的fillRect和strokeRect方法会更优。
### eg:用rect方法绘制矩形
(书36-3）
```
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="yellow";
        ctx.strokeStyle="black";
        ctx.lineWidth="4";
        
        ctx.beginPath();
        ctx.moveTo(110,10);
        ctx.lineTo(110,100);
        ctx.lineTo(10,10);
        ctx.closePath();
        
        ctx.rect(110,10,100,90);
        ctx.rect(110,100,130,30);
        
        ctx.fill();
        ctx.stroke();
    </script>
```
### eg2:绘制分离的子路径
（书36-4）
- 修改上述代码坐标参数，得到分离的子路径
```
   <script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="yellow";
        ctx.strokeStyle="black";
        ctx.lineWidth="4";
        
        ctx.beginPath();
        ctx.moveTo(110,10);
        ctx.lineTo(110,100);
        ctx.lineTo(10,10);
        ctx.closePath();
        
        ctx.rect(120,10,100,90);
        ctx.rect(150,110,130,30);
        
        ctx.fill();
        ctx.stroke();
    </script>
```
## 36.2绘制圆弧
arc和arcTo方法

名称|说明|返回
----|----|-----
acrTo(x1,y1,x2,y2,rad)|该圆弧有一个点与当前位置到(x1,y1) 的线段相切，还有一个点和从 (x1,y1) 到(x2,y2)的线段相切。这两个切点就是圆弧的起点和终点，圆弧绘制的方向就是连接这两个点的最短圆弧的方向。|void
act(x,y,rad,startAngle,endAngle,direction)|绘制一段圆弧，中心为(x,y)，半径为rad,起始角度为startAngle，终止角度为endAngle，方向为direction。沿x轴三点钟方向角度为0,角度沿着**顺**时针方向而增加。direction为true即逆时针，为FALSE即顺时针。|void

## 36.2.1 使用上下文对象的arcTo方法
### eg1:使用上下文对象的arcTo方法
（书36-5）
```
<script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        var point1=[100,10];
        var point2=[200,10];
        var point3=[200,110];
        
        ctx.fillStyle="yellow";
        ctx.strokeStyle="black";
        ctx.lineWidth="4";
        
        ctx.beginPath();
        ctx.moveTo(point1[0],point1[1]);
        ctx.arcTo(point2[0],point2[1],point3[0],point3[1],100);
        ctx.stroke();
        
        drawPoint(point1[0],point1[1]);
        drawPoint(point2[0],point2[1]);
        drawPoint(point3[0],point3[1]);
        
        ctx.beginPath();
        ctx.moveTo(point1[0],point1[1]);
        ctx.lineTo(point2[0],point2[1]);
        ctx.lineTo(point3[0],point3[1]);
        ctx.stroke();
        
        function drawPoint(x,y) {
            ctx.lineWidth=1;
            ctx.strokeStyle="red";
            ctx.strokeRect(x-2,y-2,4,4);
        }
        
    </script>
```
### eg2:响应鼠标移动使用arcTo方法绘制圆弧
（书36-6）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        canvas{
            border: thin solid black;
        }
        body>*{
            float: left;
        }
    </style>
</head>

<body>
    <canvas id="canvas" width="500" height="140">
        Your broser doesn't support the <code>canvas</code> element.
    </canvas>
    <script>
        var canvasElem=document.getElementById("canvas");
        var ctx=document.getElementById("canvas").getContext("2d");
        
        var point1=[100,10];
        var point2=[200,10];
        var point3=[200,110];
        
        draw();
        
        canvasElem.onmousemove=function(e){
            if (e.ctrlKey) {// 使用键盘事件，当ctrl键按下，返回true
                point1=[e.clientX,e.clientY];//使用鼠标事件，返回事件触发时鼠标的坐标
            }
            else if (e.shiftKey) {
                point2=[e.clientX,e.clientY];
            }
            else{
                point3=[e.clientX,e.clientY];
            }
            ctx.clearRect(0,0,540,140);
            draw();
        }
        
        function draw() {
            ctx.fillStyle="yellow";
            ctx.strokeStyle="black";
            ctx.lineWidth="4";
            
            ctx.beginPath();
            ctx.moveTo(point1[0],point1[1]);
            ctx.arcTo(point2[0],point2[1],point3[0],point3[1],50);
            ctx.stroke();
            
            drawPoint(point1[0],point1[1]);//在和圆弧相关的三个点分别画出三个小矩形
            drawPoint(point2[0],point2[1]);
            drawPoint(point3[0],point3[1]);
            
            ctx.beginPath();
            ctx.moveTo(point1[0],point1[1]);//在和圆弧相关的三个点之间画出两条线
            ctx.lineTo(point2[0],point2[1]);
            ctx.lineTo(point3[0],point3[1]);
            ctx.stroke();
        }
        
        
        function drawPoint(x,y) {
            ctx.lineWidth=1;
            ctx.strokeStyle="red";
            ctx.strokeRect(x-2,y-2,4,4);
        }
        
    </script>

</body>
</html>
```

## 36.2.2使用arc方法
### eg:使用arc方法绘制圆弧
(书36-7）
```
 <script>
        var canvasElem=document.getElementById("canvas");
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="yellow";
        ctx.lineWidth="3";
        
        ctx.beginPath();
        ctx.arc(70,70,60,Math.PI/2,-Math.PI/2,true);
        ctx.fill();
        ctx.stroke();
        
        ctx.beginPath();
        ctx.arc(200,70,60,Math.PI/2,Math.PI,true);
        ctx.fill();
        ctx.stroke();
        
        ctx.beginPath();
        var val=0;
        for(var i=0;i<4;i++){
            ctx.arc(350,70,60,val,val+Math.PI/4,false);
            val+=Math.PI/2;
        }
        ctx.closePath();
        ctx.fill();
        ctx.stroke();
    </script>
```

## 36.3 绘制贝塞尔曲线（跳过）

## 36.4 创建裁剪区域
### eg:使用裁剪方法clip()创建路径
（书36-10）
```
<script>
        var canvasElem=document.getElementById("canvas");
        var ctx=document.getElementById("canvas").getContext("2d");
        
        
        ctx.fillStyle="yellow";
        ctx.beginPath();
        ctx.rect(0,0,500,140);
        ctx.fill();
        
        ctx.beginPath();
        ctx.rect(100,20,300,100);
        ctx.clip();//使用裁剪区域
        
        ctx.fillStyle="red";
        ctx.beginPath();
        ctx.rect(0,0,500,140);
        ctx.fill();
    </script>
```

## 36.5 绘制文本
### eg: 在画布上绘制文本
(书36-11）
```
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="lightgrey";
        ctx.strokeStyle="black";
        ctx.lineWidth=3;
        
        ctx.font="100px sans-serif";
        ctx.fillText("Hello",50,100);//用两种颜色填充和描边文本
        ctx.strokeText("Hello",50,100);
    </script>
```

## 36.6使用特效和变换
## 36.6.1使用阴影
### eg:给图形和文本应用阴影
（书36-12）
```
  <script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="lightgrey";
        ctx.strokeStyle="black";
        ctx.lineWidth=3;
        
        ctx.shadowOffsetX=5;//设置阴影水平偏移量
        ctx.shadowOffsetY=5;//设置阴影垂直偏移量
        ctx.shadowBlur=5;//设置阴影模糊程度
        ctx.shadowColor="grey";//设置阴影颜色
        
        ctx.strokeRect(250,20,100,100);
        
        ctx.beginPath();
        ctx.arc(420,70,50,0,Math.PI,true);
        ctx.stroke();
        
        ctx.beginPath();
        ctx.arc(420,80,50,0,Math.PI,false);
        ctx.fill();
        
        ctx.font="100px sans-serif";
        ctx.fillText("Hello",10,100);
        ctx.strokeText("Hello",10,100);
        
    </script>
```
## 36.6.2使用透明度
### eg:使用globalAlpha属性
```
<script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="lightgrey";
        ctx.strokeStyle="black";
        ctx.lineWidth=3;
        
        ctx.font="100px sans-serif";
        ctx.fillText("Hello",10,100);
        ctx.strokeText("Hello",10,100);
        
        ctx.fillStyle="red";
        ctx.globalAlpha=0.5;//设置透明度，此为全局属性
        ctx.fillRect(100,10,150,100);
        
    </script>
```
## 36.6.3 使用合成 
- 透明度globalAlpha属性可以与globalCompositeOperation属性结合使用，可以控制图形和文本在画布上绘制的方式
### eg:使用globalCompositeOperation属性***Good!下拉选择表单的使用***
（例36-14）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        canvas{
            border: thin solid black;
        }
        body>*{
            float: left;
        }
    </style>
</head>

<body>
    <canvas id="canvas" width="300" height="120">
        Your broser doesn't support the <code>canvas</code> element.
    </canvas>
    <label>Composition Value:</label>
    <select id="list">//Good!下拉选择表单的使用
        <option>copy</option>
        <option>destination-atop</option><option>destination-in</option>
        <option>destination-over</option><option>destination-out</option>
        <option>lighter</option><option>source-atop</option>
        <option>source-in</option><option>source-out</option>
        <option>source-over</option><option>xor</option>
    </select>
    
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="lightgrey";
        ctx.strokeStyle="black";
        ctx.lineWidth=3;
        
        var compVal="copy";
        
        document.getElementById("list").onchange=function(e){
            compVal=e.target.value;
            draw();
        }
        
        draw();
        function draw() {
            ctx.clearRect(0,0,300,120);
            ctx.globalAlpha=1.0;
            ctx.font="100px sans-serif";
            ctx.fillText("Hello",10,100);
            ctx.strokeText("Hello",10,100);// 设置合成属性之前的为源图像
            
            ctx.globalCompositeOperation=compVal;
            
            ctx.fillStyle="red";
            ctx.globalAlpha=0.5;
            ctx.fillRect(100,10,150,100);//设置合成属性之后的为目标图像
        }
    </script>

</body>
</html>
```


## 36.6.4使用变换
### eg:使用变换属性
（书36-15）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style>
        canvas{
            border: thin solid black;
        }
        body>*{
            float: left;
        }
    </style>
</head>

<body>
    <canvas id="canvas" width="300" height="120">
        Your broser doesn't support the <code>canvas</code> element.
    </canvas>
    
    <script>
        var ctx=document.getElementById("canvas").getContext("2d");
        
        ctx.fillStyle="lightgrey";
        ctx.strokeStyle="black";
        ctx.lineWidth=3;
      
        ctx.clearRect(0,0,300,120);
        ctx.globalAlpha=1.0;
        ctx.font="100px sans-serif";
        ctx.fillText("Hello",10,100);
        ctx.strokeText("Hello",10,100);
        
        ctx.scale(1.3,1.3);//沿X轴y轴放大1.3倍
        ctx.translate(100,-50);//沿x轴y轴平移，画布坐标重映射
        ctx.rotate(0.5);//画布围绕点(0,0)顺时针旋转指定弧度
        
        ctx.fillStyle="red";
        ctx.globalAlpha=0.5;
        ctx.fillRect(100,10,150,100);
        
        ctx.strokeRect(0,0,300,200);
        
        
    </script>

</body>
</html>
```