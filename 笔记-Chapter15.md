# Chapter15 嵌入内容

## 1.超链接里嵌入图像
```
<!DOCTYPE html>
<html>
<head>
    <title>Example</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
</head>

<body>
    <p>
        <a href="otherpage.html">
            <img src="qq.png" ismap alt="QQ" width="100" height="100"/>
        </a>
    </p>
</body>
</html>
```
otherpage.html如下：
```
<!DOCTYPE html>

<html>
<head>
    <title>Other Page</title>
</head>

<body>
    <p>The X-coordinate is <b><span id="xcon">??</span></b></p>
    <p>The Y-coordinate is <b><span id="ycon">??</span></b></p>
    
    <script>
        var coords=window.location.href.split('?')[1].split(',');
        document.getElementById('xco').innerHTML=coords[0];
        document.getElementById('yco').innerHTML=coords[1];
    </script>
</body>
</html>
```

## 2.分区相应图
- map元素包含area元素，每个area元素代表图像上可被点击的一块区域。
- area元素的shape和coords属性规定图像区域尺寸及位置
- 要给img元素添加usemap属性以把图像与map元素关联起来
```
    <p>
        <img src="triathlon.png" usemap="#mymap" alt="Triathlon Image"/>
    </p>
    
    <map name="mymap">
        <area href="Chapter13.html" shape="rect" coords="3,5,68,62" alt="Swimming"/>
        <!--rect 时，coods依次为矩形左边缘与图形左边缘之距、矩形上边缘与图像上边缘之距、矩形右边缘与图像左边缘之距、矩形下边缘与图像上边缘之距-->
        <area href="cyclepage.html" shape="rect" coords="70,5,130,62" alt="Running"/>
        <area href="otherpage.html" shape="default" alt="default"/>
        <!--default时，不用设置coods,为图像其他所有区域-->
    </map>
```

# 3.使用iframe元素嵌入另一个html文档
- 其他元素（a、form、button、input、base）的target属性可将href属性中指定的URL载入iframe
- iframe元素的src属性可以指定一开始就载入并显示的URL
```
        <header>
            <h1>Things I Like</h1>
                <nav>
                   <ul>
                       <li>
                           <a href="fruits.html" target="myframe">Fruits I Like</a>
                       </li>
                       <li>
                           <a href="activities.html" target="frame">Activities I Like</a>
                       </li>
                   </ul>
                </nav>
        </header>
        <iframe src="Chapter13.html" name="myframe" width="300" height="100">
        </iframe>
```

# 4.使用embed元素嵌入内容
```
    <embed src="http://www.youtube.com/v/qzA60hHca9s?version=3" type="application/x-shockwave-flash" width="560" height="345" allowfullscreen="true"/>
```
# 5.使用object和param元素嵌入内容
- 用法类似4.
- param元素定义将要传递给插件的参数，每个参数都各自使用一个param
- 加入了备用内容，以防内容显示不出来
- object元素不添加type属性时，浏览器会尝试从数据本身判断其内容类型
```
   <object width="560" height="349" data="http://www.youtube.com/v/qzA60hHca9s?version=3"
            >
        <param name="allowFullScreen" value="true"/>
    </object>
    
```
该代码有问题，备用内容显示不出来

# 6.object嵌入图像
虽然object元素主要用于嵌入插件内容，但它最初是作为一种更具通用性的元素来取代某些元素，包括img
```
    <object data="triathlon.png" tye="image/png">
    </object>
```

# 7.使用object元素嵌入另一个html文档,原理同3
仅在object元素的type属性为text/html时可用
```
 <header>
            <h1>Things I Like</h1>
                <nav>
                   <ul>
                       <li>
                           <a href="fruits.html" target="myframe1">Fruits I Like</a>
                       </li>
                       <li>
                           <a href="activities.html" target="myframe1">Activities I Like</a>
                       </li>
                   </ul>
                </nav>
        </header>
        <object type="text/html" name="myframe1" width="300" height="100">
        </object> 
```

# 8.progress元素显示过程
- progress元素的value属性定义了当前进度，它位于0和max属性值所构成的范围之间。
- 当max属性被忽略时，范围是0至1,value为浮点数，如0.3
```
  <progress id="myprogress" value="10" max="100"></progress>
    <p>
        <button type="button" value="30">30%</button>
        <button type="button" value="60">60%</button>
        <button type="button" value="90">90%</button>
    </p>
    <script>
        var buttons=document.getElementsByTagName('BUTTON');
        var progress=document.getElementById('myprogress');
        for(var i=0;i<buttons.length;i++)
        {
            buttons[i].onclick=function(e)
            {
                progress.value=e.target.value;
            };
        }
    </script>
```

# 9.meter元素显示某个范围内所有可能值中的一个
- 用法类似8，meter元素的min、max属性设定了可能值所处范围的边界，可用浮点数表示。
- meter元素的显示可分为三个部位：过低，过高，最佳。在low属性设置值以下的所有值都是过低;high属性设置值以上的所有值都是过高；optimum属性指定了最佳值。
```

    <meter id="mymeter" value="90" min="10" max="100" low="40" high="80" optimum="60"></meter>
    <p>
        <button type="button" value="30">30</button>
        <button type="button" value="60">60</button>
        <button type="button" value="90">90</button>
    </p>
    <script>
        var buttons=document.getElementsByTagName('BUTTON');
        var progress=document.getElementById('mymeter');
        for(var i=0;i<buttons.length;i++)
        {
            buttons[i].onclick=function(e)
            {
                meter.value=e.target.value;
            };
        }
    </script>
```