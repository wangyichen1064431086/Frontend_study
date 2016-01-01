# Chapter30 使用事件
## 30.1 使用简单事件处理器
- 处理事件的最直接方式是：用**事件属性**创建一个简单事件处理器
- 每个事件都对应一个事件属性，如mouseover事件——onmouseover属性：光标移动至元素占据的屏幕区域上方触发；
- 事件是成双成对的，mouseover事件和mouseout事件相对

## 30.1.1实现简单的内联事件处理器
### eg：用内联JavaScript处理事件
（书30-1，30-2）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
    </style>
</head>

<body>
    <p onmouseover="this.style.background='white';this.style.color='black'"
       onmouseout="this.style.removeProperty('color');this.style.removeProperty('background')" >
        <!--onmouseovers事件属性：当光标移动到元素占据的浏览器屏幕区域上方时触发-->
        <!--特殊变量this：代表触发事件元素的HTMLElement对象-->
        <!--style属性：返回该元素的CSSStyleDeclaration对象-->
        <!-- 双引号界定整个属性值，单引号指定想要的颜色;单双引号亦可交换-->
        <!-- onmouseout事件属性：mouseover与mouseout事件是相对的-->
       
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>

</body>
</html>
```
## 30.1.2 实现一个简单的事件处理函数
- 上述内联事件处理器存在两个问题：
1. 太繁琐，HTML难以阅读
2. 以上的JavaScript语句只能应用在一个元素上，其他元素要拥有相同行为必须在p元素上重复添加这些语句
- 解决办法：定义一个函数，将函数名指定为元素事件属性的值（当然函数名也可是任意其他）
### eg:用函数来处理事件
（书30-3）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
    </style>
    <script type="text/javascript">
        function handleMouseOver(elem) {
            elem.style.background='white';
            elem.style.color='black';
        }
        function handleMouseOut(elem) {
            elem.style.removeProperty('color');
            elem.style.removeProperty('background');
        }
    </script>
</head>

<body>
    <p onmouseover="handleMouseOver(this)" onmouseout="handleMouseOut(this)">
        <!-- 特殊值this指的是触发事件的元素 ,此处调用函数可以不打引号-->
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <p onmouseover="handleMouseOver(this)" onmouseout="handleMouseOut(this)">
        One of the most interesting aspects of fruit is the variety available in each country.
     </p>


</body>
</html>
```
- 自己重写的：
```
<head>
  .....
 <script type="text/javascript">
        function mouseover(element) {
            element.style.color='black';
            element.style.background='white';
        }
        function mouseout(element) {
            element.style.removeProperty("color");
            element.style.removeProperty("background");
        }
    </script>
</head>

<body>
    <p onmouseover=mouseover(this) onmouseout=mouseout(this)>
        <!-- 特殊值this指的是触发事件的元素-->
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <p onmouseover=mouseover(this) onmouseout=mouseout(this)>
        One of the most interesting aspects of fruit is the variety available in each country.
     </p>
</body>
```

## 30.2使用DOM和事件对象
### eg1： 使用DOM构建事件处理
（书30-4）
- 操作DOM的脚本，必须放在页尾
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
    </style>
</head>

<body>
    <p>
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <p>
        One of the most interesting aspects of fruit is the variety available in each country.
     </p>
    
    <script type="text/javascript">
        var pElems=document.getElementsByTagName("p");
        for (var i=0;i<pElems.length;i++){
            pElems[i].onmouseover=handleMouseOver;
            pElems[i].onmouseout=handleMouseOut;
            //找到处理事件的所有元素，，然后给事件处理器属性设置一个函数名
            //注意！这里的handleMouseOver一定不能写成handleMouseOver(),负责函数会在脚本执行时（非事件触发时）被调用
        }
        
        function handleMouseOver(e) {//参数e:它会被设成浏览器所创建的一个Event对象，用于在事件触发时代表该事件
            e.target.style.background='white';//target属性用来获取触发事件的HTMLElement(HTML元素)
            e.target.style.color='black';
        }
        function handleMouseOut(e) {
            e.target.style.removeProperty('color');
            e.target.style.removeProperty('background');
        }
    </script>


</body>
</html>
```
### eg2:使用addEventListener和removeEventListener方法
(书30-5）
- addEventListener方法和removeEventListener方法：由HTMLElement对象实现，将函数和事件关联起来
- 函数中的参数e:它会被设成浏览器所创建的一个Event对象，用于在事件触发时代表该事件
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
    </style>
</head>

<body>
    <p>
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <p id="block2">
        One of the most interesting aspects of fruit is the variety available in each country.
     </p>
    <button id="pressme">Press Me</button>
    
    <script type="text/javascript">
        var pElems=document.getElementsByTagName("p");
        for (var i=0;i<pElems.length;i++){
            pElems[i].addEventListener("mouseover",handleMouseOver);
            //addEventListener方法：由HTMLElement对象实现，将函数和事件关联。
            //此处是把handleMouseOver函数注册为p元素的事件处理器
            pElems[i].addEventListener("mouseout",handleMouseOut);
        }
        
        document.getElementById("pressme").onclick=function(){
            document.getElementById("block2").removeEventListener("mouseout",handleMouseOut);
            //removeEventListener方法：取消函数和事件间的关联。
            //此处当button被点击后,removeEventListener方法取消了id值为block2的p元素的mouseout事件与handleMouseout函数的关联
        }
        
        function handleMouseOver(e) {//参数e:它会被设成浏览器所创建的一个Event对象，用于在事件触发时代表该事件
            e.target.style.background='white';//target属性用来获取触发事件的HTMLElement(HTML元素)
            e.target.style.color='black';
        }
        function handleMouseOut(e) {
            e.target.style.removeProperty('color');
            e.target.style.removeProperty('background');
        }
    </script>


</body>
</html>
```
- 自己重写的script部分
```
  <script type="text/javascript">
        var pElems=document.getElementsByTagName("p");
       
        for(var i=0;i<pElems.length;i++){
            pElems[i].addEventListener("mouseover",handleMouseOver);
            pElems[i].addEventListener("mouseout",handleMouseOut);
        }
        
        document.getElementById("pressme").onclick=function(){
            pElems[0].removeEventListener("mouseover",handleMouseOver);
            pElems[1].removeEventListener("mouseout",handleMouseOut);
        }
        
        function handleMouseOver(e) {//参数e:它会被设成浏览器所创建的一个Event对象，用于在事件触发时代表该事件
            e.target.style.background='white';//target属性用来获取触发事件的HTMLElement(HTML元素)
            e.target.style.color='black';
        }
        function handleMouseOut(e) {
            e.target.style.removeProperty('color');
            e.target.style.removeProperty('background');
        }
    </script>
```
Event对象的函数和属性（即e.后面接的）***important***

名称|说明|返回
type|事件的名称（如mouseover）|字符串
target|事件指向的元素|HTMLElement
currentTarget|带有当前被触发事件监听器的元素|HTMLElement
eventPhase|事件的生命周期阶段|数值
bubbles|如果事件会在文档里冒泡，则返回true,否则false|布尔值
cancelable|如果事件带有可撤销的默认行为则返回true,否则返回false|布尔值
timeStamp|事件的创建时间，如果时间不可用则为0|字符串
stopPropagation()|在当前元素的事件监听器被触发后终止事件在元素树中的流动|void
stopImmediatePropagation()|立即终止事件在元素树中的流动。当前元素上未被触发的事件监听器会被忽略|void
preventDefault()|防止浏览器执行与事件关联的默认操作|void
defaultPrevented|如果调用过preventDefault()则返回true|布尔值

## 30.2.1 按类型区分事件
- event对象（事件对象）的type属性：告诉你正在处理的是哪种类型（例如是mouseover类型呀还是mouseout类型）的事件。有了探测事件类型的能力，便可以用一个函数来处理多个类型了。

### eg:使用事件对象的type属性
（书30-6）***Good!***
- 本例只用handleMouseEvent这一个事件处理函数，使用type属性判断正在处理的是哪一种事件
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
    </style>
</head>

<body>
    <p>
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <p id="block2">
        One of the most interesting aspects of fruit is the variety available in each country.
     </p>
    
    <script type="text/javascript">
        var pElems=document.getElementsByTagName("p");
        for (var i=0;i<pElems.length;i++){
           pElems[i].onmouseover=handleMouseEvent;
           pElems[i].onmouseout=handleMouseEvent;
        }
        
        function handleMouseEvent(e) {//参数e:它会被设成浏览器所创建的一个Event对象，用于在事件触发时代表该事件
            //target属性用来获取触发事件的HTMLElement(HTML元素)
            if (e.type =="mouseover") {
                e.target.style.background='white';
                e.target.style.color='black';
            }
            else{
                e.target.style.removeProperty('color');
                e.target.style.removeProperty('background');
            }
        }
    </script>


</body>
</html>
```

## 30.2.2 理解事件流
## 1.理解捕捉阶段
- 捕捉阶段过程：
1. 事件被触发时,浏览器会找出事件触发涉及的元素，即目标元素。
2. 浏览器会找出body元素和目标元素之间的所有元素（例如目标元素的父元素）并分别检查他们，看看它们是否带有事件处理器且是不是带有对后代元素触发事件的通知。
3. 浏览器会先触发这些事件处理器（body和目标元素之间的元素所带的事件处理器），再触发目标自身的处理器。
- Event对象target属性：返回事件指向的元素
- Event对象currentTarget属性：返回带有当前被触发事件监听器的元素
- Event对象eventPhase属性：返回事件生命周期阶段。包括三个值：Event.CAPTURING PHASE(捕捉阶段）、Event.AT_TARGET（目标阶段）、Event.BUBBULING_PHASE(冒泡阶段）

### eg1:捕捉事件
（书30-7）***还是不太明白为何当鼠标在p上非span的部分时，事件不会被触发？此时是否target和currentTarget为一个元素（p）---应该就是这个原因***
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
        span{
            background: white;
            color: black;
            padding: 2px;
            cursor: default;
        }
    </style>
</head>

<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of <span id="banana">banana</span> alone.
    </p>
 
    <script type="text/javascript">
        var banana=document.getElementById("banana");
        var textblock=document.getElementById("block1");
        
        banana.addEventListener("mouseover",handleMouseEvent);//目标元素<span>
        banana.addEventListener("mouseout",handleMouseEvent);
        textblock.addEventListener("mouseover",handleDescendantEvent,true);//body和目标元素之间的元素<p>
         //参数true:告诉浏览器向让p元素在**捕捉阶段** 接收后代元素的事件
        textblock.addEventListener("mouseout",handleDescendantEvent,true);
        
        function handleDescendantEvent(e) {
            if (e.type=="mouseover"&&e.eventPhase==Event.CAPTURING_PHASE) {
                e.target.style.border="thick solid red";
                e.currentTarget.style.border="thick double black";
            }
            else if (e.type=="mouseout"&&e.eventPhase==Event.CAPTURING_PHASE) {
                e.target.style.removeProperty("border");
                e.currentTarget.style.removeProperty("border");
            }
        }
        
        function handleMouseEvent(e) {//参数e:它会被设成浏览器所创建的一个Event对象，用于在事件触发时代表该事件
            //target属性用来获取触发事件的HTMLElement(HTML元素)
            if (e.type =="mouseover") {
                e.target.style.background='yellow';
                e.target.style.color='blue';
            }
            else{
                e.target.style.removeProperty('color');
                e.target.style.removeProperty('background');
            }
        }
    </script>


</body>
</html>
```
- 自己重写的script:
```
<script type="text/javascript">
       var tarobject=document.getElementById("banana");
       var midobject=document.getElementById("block1");
       
       tarobject.addEventListener("mouseover",mousefunc);
       tarobject.addEventListener("mouseout",mousefunc);
       
       midobject.addEventListener("mouseover",midmousefunc,true);
//去掉true,midmousefunc不起作用
       midobject.addEventListener("mouseout",midmousefunc,true);
       
       function mousefunc(e) {
            if (e.type=="mouseover") {
                e.target.style.background='yellow';
                e.target.style.color='green';
            }
            else if (e.type=="mouseout") {
                e.target.style.removeProperty('background');
                e.target.style.removeProperty('color');
            }
        }
        
        function midmousefunc(e) {
            if (e.type=="mouseover"&&e.eventPhase==Event.CAPTURING_PHASE) {
//去掉&&e.eventPhase==Event.CAPTURING_PHASE，其不在目标子元素捕捉阶段也能触发，能显示出'thick solid orange'
                e.target.style.border='thick double red';
                e.currentTarget.style.border='thick solid orange';
            }
            else if (e.type=="mouseout"&&e.eventPhase==Event.CAPTURING_PHASE) {
                e.target.style.removeProperty('border');
                e.currentTarget.style.removeProperty('border');
            }
           // e.stopPropagation();
        }
    </script>

```
### eg2:阻止事件流前进
(书30-8）
- 事件捕捉让目标元素的各个上级元素都有机会在事件传递到目标元素本身之前对其作出反应。搜集元素的事件处理器可以组织事件流向目标，方法是对Event对象调用stopPropagation或stopImmediatePropagation函数。
- ***？？还是不太懂stopPropagation和stopImmediatePropagation的区别？？***
```
  //修改上述代码的function handleDescendantEvent(e)
  function handleDescendantEvent(e) {
            if (e.type=="mouseover"&&e.eventPhase==Event.CAPTURING_PHASE) {
                e.target.style.border="thick solid red";
                e.currentTarget.style.border="thick double black";
            }
            else if (e.type=="mouseout"&&e.eventPhase==Event.CAPTURING_PHASE) {
                e.target.style.removeProperty("border");
                e.currentTarget.style.removeProperty("border");
            }
            e.stopPropagation();
            //stopPropagation()函数：在当前元素的事件监听器被触发后，终止事件在元素树中的流动
            //对比stopImmediatePropagation()函数；终止事件在元素树中的流动，但当前元素上未被触发的事件监听器会被忽略
        }
```

## 2.理解目标阶段
- 目标阶段： 当捕捉阶段完成后，浏览器会触发目标元素上任何已添加的事件类型监听器
- 目标元素的某个给定事件类型可以有多个监听器，即可以多次调用addEventListener函数

## 3.理解冒泡阶段
- 冒泡阶段：在完成目标阶段后，浏览器开始转而沿着上级元素链朝body元素前进。在沿途的每个元素上，浏览器都会检查是否存在针对该事件类型但没有启用捕捉的监听器（即addEventListener函数第三个参数是false）
### eg:事件冒泡
（书30-9）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
        span{
            background: white;
            color: black;
            padding: 2px;
            cursor: default;
        }
    </style>
</head>

<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of <span id="banana">banana</span> alone.
    </p>
 
    <script type="text/javascript">
        var banana=document.getElementById("banana");
        var textblock=document.getElementById("block1");
        
        banana.addEventListener("mouseover",handleMouseEvent);//目标元素<span>
        banana.addEventListener("mouseout",handleMouseEvent);
        textblock.addEventListener("mouseover",handleDescendantEvent,true);//body和目标元素之间的元素<p>
        textblock.addEventListener("mouseout",handleDescendantEvent,true);
        //参数true:告诉浏览器向让p元素在捕捉阶段接收后代元素的事件
        textblock.addEventListener('mouseover',handleBubbleMouseEvent,false);
        textblock.addEventListener('mouseout',handleBubbleMouseEvent,false);
        //参数false:告诉浏览器该监听器没有启用捕捉
        
        function handleBubbleMouseEvent(e) {
            if (e.type=="mouseover"&&e.eventPhase==Event.BUBBLING_PHASE) {
                e.target.style.textTransform="uppercase";
            }
            else if (e.type=="mouseout"&&e.eventPhase==Event.BUBBLING_PHASE) {
                e.target.style.textTransform="none";
            }
        }
        
        function handleDescendantEvent(e) {
            if (e.type=="mouseover"&&e.eventPhase==Event.CAPTURING_PHASE) {
                e.target.style.border="thick solid red";
                e.currentTarget.style.border="thick double black";
            }
            else if (e.type=="mouseout"&&e.eventPhase==Event.CAPTURING_PHASE) {
                e.target.style.removeProperty("border");
                e.currentTarget.style.removeProperty("border");
            }
        }
        
        function handleMouseEvent(e) {//参数e:它会被设成浏览器所创建的一个Event对象，用于在事件触发时代表该事件
            //target属性用来获取触发事件的HTMLElement(HTML元素)
            if (e.type =="mouseover") {
                e.target.style.background='yellow';
                e.target.style.color='blue';
            }
            else{
                e.target.style.removeProperty('color');
                e.target.style.removeProperty('background');
            }
        }
    </script>


</body>
</html>
```

- 事件流三阶段总结：
对目标元素的父元素使用addEventListener方法添加监听器是，该监听器除了会受到自身事件的通知，还会受到后代元素的事件的通知。你要选择的是在后代元素事件的目标阶段之前还是之后调用监听器。
- eg30-9总结说明：文档里的span元素在发生mouseover事件时会触发三个监听函数：
1. handleDescendantEvent函数会在捕捉阶段被触发
2. handleMouseEvent函数会在目标阶段被调用
3. handleBubbleMouseEvent函数在冒泡阶段被触发

### eg:通过bubble属性检查事件能否冒泡
（自己补充的）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
        span{
            background: white;
            color: black;
            padding: 2px;
            cursor: default;
        }
    </style>
</head>

<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of <span id="banana">banana</span> alone.
    </p>
    
    <div id="placeholder"></div>
 
    <script type="text/javascript">
        var banana=document.getElementById("banana");
        var textblock=document.getElementById("block1");
        var placeholder=document.getElementById("placeholder");
        
        banana.addEventListener("mouseover",handleMouseEvent);//目标元素<span>
        banana.addEventListener("mouseout",handleMouseEvent);
        textblock.addEventListener("mouseover",handleDescendantEvent,true);//body和目标元素之间的元素<p>
        textblock.addEventListener("mouseout",handleDescendantEvent,true);
        //参数true:告诉浏览器向让p元素在捕捉阶段接收后代元素的事件
        textblock.addEventListener('mouseover',handleBubbleMouseEvent,false);
        textblock.addEventListener('mouseout',handleBubbleMouseEvent,false);
        //参数false:告诉浏览器该监听器没有启用捕捉
        
        function handleBubbleMouseEvent(e) {
            if (e.type=="mouseover"&&e.eventPhase==Event.BUBBLING_PHASE) {
                e.target.style.textTransform="uppercase";
            }
            else if (e.type=="mouseout"&&e.eventPhase==Event.BUBBLING_PHASE) {
                e.target.style.textTransform="none";
            }
        }
        
        function handleDescendantEvent(e) {
            if (e.type=="mouseover"&&e.eventPhase==Event.CAPTURING_PHASE) {
                e.target.style.border="thick solid red";
                e.currentTarget.style.border="thick double black";
                placeholder.innerHTML="mouseover: "+e.bubbles+ "\n";//检查该事件是否能在文档里冒泡
                //??为何有\n还不换行？？
            }
            else if (e.type=="mouseout"&&e.eventPhase==Event.CAPTURING_PHASE) {
                e.target.style.removeProperty("border");
                e.currentTarget.style.removeProperty("border");
                placeholder.innerHTML+="mouseout: "+e.bubbles+"\n";//检查该事件是否能在文档里冒泡
            }
        }
        
        function handleMouseEvent(e) {//参数e:它会被设成浏览器所创建的一个Event对象，用于在事件触发时代表该事件
            //target属性用来获取触发事件的HTMLElement(HTML元素)
            if (e.type =="mouseover") {
                e.target.style.background='yellow';
                e.target.style.color='blue';
            }
            else{
                e.target.style.removeProperty('color');
                e.target.style.removeProperty('background');
            }
        }
        
  
    </script>


</body>
</html>
```

## 30.2.3 使用可撤销事件
- 有些事件定义了一种默认行为，会在事件被触发时执行，如a元素click事件的默认操作是浏览器会载入href属性所指向的URL内容。
- e.canelable属性：当一个事件拥有默认行为时，该属性值为true,且该默认行为是可撤销的
- e.preventDefault()函数：阻止某事件默认行为的执行
- e.defaultPrevented属性: 可检测PreventDefault()函数是否已经被之前的某个事件处理器上的事件调用过。

### eg:撤销事件默认行为
（书30-10）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        a{
            background: gray;
            color: white;
            padding: 10px;
            border:thin solid black;
        }
    </style>
</head>

<body>
    <p>
        <a href="http://apress.com">Visit Apress</a>
        <a href="http://w3c.org">Visit W3C</a>
    </p>
    
    <script type="text/javascript">
        function handleClick(e) {
            if (!confirm("Do you want to navigate to "+e.target.href+"?")) {
                e.preventDefault();
            }//confirm函数：弹出对话框提示哟过户是否真要导航到a元素指向的URL上，
            //选择确认则confirm函数返回true,选取消函数返回false，执行e.preventDefaulte()
            //e.preventDefault()函数：阻止浏览器执行与事件关联的默认操作，如a元素click事件的默认操作时浏览器会载入href属性所指向的URL内容
        }
        
        var elems=document.querySelectorAll("a");
        for(var i=0;i<elems.length;i++){
            elems[i].addEventListener("click",handleClick,false);
        }
    </script>


</body>
</html>
```
## 30.3使用HTML事件
## 30.3.2使用鼠标事件
- 当某个鼠标事件被触发时，浏览器会指派一个MouseEvent对象
与鼠标相关的事件

名称|说明
----|----
click|在点击并释放鼠标时触发
dblclick|在两次点击并释放鼠标时触发
mousedown|在电击鼠标时触发
mouseenter|在光标移入元素或某个后代元素所占据的屏幕区域时触发
mouseleave|在光标移除元素或某个后代元素所占据的屏幕区域时触发
mousemove|当光标在元素上移动时触发
mouseout|与mouseleave基本相同
mouseover|与mouseenter基本相同
mouseup|在释放鼠标键时触发
### eg:使用MouseEvent对象响应鼠标事件***Good!***
(书30-11）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
        table{
            margin: 5px;
            border-collapse: collapse;
        }
        th,td{
            padding: 4px;
        }
    </style>
</head>

<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of <span id="banana">banana</span> alone.
    </p>
    <table border="1">
        <tr><th>Type:</th><td id="eType"></td></tr>
        <tr><th>X:</th><td id="eX"></td></tr>
        <tr><th>Y:</th><td id="eY"></td></tr>
    </table>
    
    <script type="text/javascript">
       var textblock=document.getElementById("block1");
       var typeCell=document.getElementById("eType");
       var xCell=document.getElementById("eX");
       var yCell=document.getElementById("eY");
       
       textblock.addEventListener("mouseover",handleMouseEvent,false);
       textblock.addEventListener("mouseout",handleMouseEvent,false);
       textblock.addEventListener("mousemove",handleMouseEvent,false);
        //mousemove事件：光标在元素上移动时触发
       
       function handleMouseEvent(e) {
            if (e.eventPhase==Event.AT_TARGET) {
                typeCell.innerHTML=e.type;
                xCell.innerHTML=e.clientX;//返回事件触发时鼠标相对于元素视口的X坐标
                yCell.innerHTML=e.clientY;//返回事件触发时鼠标相对于元素视口的Y坐标
                
                if (e.type=="mousemove") {
                    e.target.style.background='black';
                    e.target.style.color='white';
                }
                else{
                    e.target.style.removeProperty('color');
                    e.target.style.removeProperty('background');
                }
            }
        }
    </script>


</body>
</html>
```
## 30.3.3 使用键盘焦点事件
- 与键盘焦点相关的事件触发于元素获得和失去焦点之时，FocusEvent对象代表了这些事件
与键盘焦点相关的事件

名称|说明
----|----
blur|在元素失去焦点时触发
focus|在元素获得焦点时触发
focusin|在元素即将获得焦点时触发
focusout|在元素即将失去焦点时触发

### eg:使用键盘焦点事件***Good!尤其是后面自己总结的两种script部分写法***
(书30-12）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
    </style>
</head>

<body>
    <form>
        <p>
            <label for="fave">
                Fruit:<input autofocus id="fave" name="fave"/>
            </label>
        </p>
        <p>
            <label for="name">
                Name:<input id="name" name="name"/>
            </label>
        </p>
        <button type="submit">
            Submit Vote
        </button>
        <button type="reset">
            Reset
        </button>
        
    </form>
    
    <script type="text/javascript">
        var inputElems=document.getElementsByTagName("input");
        for(var i=0;i<inputElems.length;i++){
            inputElems[i].onfocus=handleFocusEvent;//focus事件：在元素获得焦点时触发
            inputElems[i].onblur=handleFocusEvent;//blur事件：在元素失去焦点时触发
        }
        
        function handleFocusEvent(e) {
            if (e.type=="focus") {
                e.target.style.backgroundColor="lightgray";
                e.target.style.border="thick double red";
            }
            else{
                e.target.style.removeProperty("background-color");
                e.target.style.removeProperty("border");
            }
        }
    </script>


</body>
</html>
```
- 自己重写的script部分：方式1使用事件属性
```
<script type="text/javascript">
       var inputelements=document.getElementsByTagName("input");
       for(var i=0;i<inputelements.length;i++){
            inputelements[i].onfocus=keyfocusfunc;
            inputelements[i].onblur=keyfocusfunc;
       }
       
       function keyfocusfunc(e) {
            if (e.type=="focus") {
                e.target.style.border='thick double yellow';
                e.target.style.background='blue';
            }
            else if (e.type=="blur") {
                e.target.style.removeProperty("border");
                e.target.style.removeProperty("background");
            }
       }
    </script>
```
- 自己重写的script部分：方式2使用addEventListener和removeEventListener函数
```
   <script type="text/javascript">
       var inputelements=document.getElementsByTagName("input");
       for(var i=0;i<inputelements.length;i++){
            inputelements[i].addEventListener("focus",keyfocusfunc);
            inputelements[i].addEventListener("blur",keyfocusfunc);
       }
       
       function keyfocusfunc(e) {
            if (e.type=="focus") {
                e.target.style.border='thick double yellow';
                e.target.style.background='blue';
            }
            else if (e.type=="blur") {
                e.target.style.removeProperty("border");
                e.target.style.removeProperty("background");
            }
       }
    </script>
```

## 30.3.4 使用键盘事件
- 键盘事件由按键操作触发，KeyboardEvent对象代表了这些事件
与键盘相关的事件

名称|说明
----|----
keydown|用户按下某个按键时触发
keypress|在用户按下并释放某个按键时触发
keyup|在用户释放某个键时触发

### eg:使用键盘事件
(书30-13）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter30</title>
    <style type="text/css">
        p{
            background: gray;
            color: white;
            padding: 10px;
            margin: 5px;
            border:thin solid black;
        }
    </style>
</head>

<body>
    <form>
        <p>
            <label for="fave">
                Fruit:<input autofocus id="fave" name="fave"/>
            </label>
        </p>
        <p>
            <label for="name">
                Name:<input id="name" name="name"/>
            </label>
        </p>
        <button type="submit">
            Submit Vote
        </button>
        <button type="reset">
            Reset
        </button>
    </form>
    
    <span id="message"></span>
    
    <script type="text/javascript">
        var inputElems=document.getElementsByTagName("input");
        for(var i=0;i<inputElems.length;i++){
           inputElems[i].onkeyup=handleKeyboardEvent;//keyup事件：在用户释放某个键时触发
        }
        
        function handleKeyboardEvent(e) {
            document.getElementById("message").innerHTML="Key pressed:"+e.keyCode+" Char:"+String.fromCharCode(e.keyCode);
            //keyCode属性：获取按下按键的unicode值  String.fromCharCode()函数：将该值转变为字符
        }
    </script>


</body>
</html>
```
- 自己重写的script部分另一种写法
```
   <script type="text/javascript">
        var inputElems=document.getElementsByTagName("input");
        for(var i=0;i<inputElems.length;i++){
           inputElems[i].addEventListener("keydown",handleKeyboardEvent,false);//keyup事件：在用户释放某个键时触发
        }
        
        function handleKeyboardEvent(e) {
            if (e.eventPhase==Event.AT_TARGET) {
                document.getElementById("message").innerHTML="Key pressed:"+e.keycode+" Char:"+String.fromCharCode(e.keyCode);
            }
            //keyCode属性：获取按下按键的unicode值  String.fromCharCode()函数：将该值转变为字符
        }
    </script>
```