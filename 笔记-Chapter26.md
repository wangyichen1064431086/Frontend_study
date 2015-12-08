# Chapter26 使用Document对象


## eg：一个简单的例子
(书26-1）

```
<!DOCTYPE html>
<html>
<head>
    <title>Chapter25</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
</head>
<body>
    <p id="fruittext">
        There are lots of different kinds of fruit - there are over 500 varieties of <span id="banana">banana</span>alone.
        By the time we add the countless types of apples,oranges,and other well-known fruit,we are faced with thousands of choices.
    </p>
    <p id="apples">
        One of the most interesting aspects of fruit is the variety available in each country.
        I live near London, in an area which is known for its apples.
    </p>
    <script>
        document.writeln("<pre>URL:"+document.URL);//<pre>定义预格式化的文本
                                                   //document.URL属性返回当前文档的URL
                                                   //writeln方法将内容附加到HTML文档末尾
        var elems=document.getElementsByTagName("p");//getElementsByTagName方法选择属于某一给定类型的所有元素，返回的是HTMLELEMENT对象组成的一个集合
        for(var i=0; i<=elems.length; i++)//for循环来遍历集合内容
        {
            document.writeln("Element ID:"+elems[i].id);//elems[i].id属性获得id值
                                                        //document.writeln把结果附加到之前的pre元素内容上
            elems[i].style.border="medium double black";//.style.XX属性改变CSS样式
            elems[i].style.padding="4px";
        }
        document.write("</pre>");//write方法与writeln类似，但不会给添加到文档里的字符串加上行结束字符
    </script>
</body>
</html>
```
**writeln和write的区别：**<br>
 document.writeln()写下的内容末尾自带\n,document.write()写下的内容无\n

## 26.1 使用Document元数据
- 元数据：描述数据的数据，这里是提供文档信息的数据
Document元数据属性

属性|说明|返回
----|----|----
characterSet|返回文档字符集编码。只读属性|字符串
charset|获取或设置文档的字符集编码|字符串
compatMode|获取文档的兼容性模式|字符串
cookie|获取或设置当前文档的cookie|字符串
defaultCharset|获取浏览器所使用的默认字符编码|字符串
defaultView|返回当前文档的Window对象|Window
dir|获取或设置文档的文本方向|字符串
domain|获取或设置文档当前的域名|字符串
implementation|提供可用DOM功能的信息|DOMImplementation
lastModified|返回文档的最后修改时间|字符串
location|提供当前文档的URL信息|Location
readyState|返回当前文档的状态。只读属性|字符串
referer|返回链接到文档的文档URL(即HTTP标头的值）|字符串
title|获取或设置当前文档的标题|字符串

## 26.1.1 获取文档信息
### eg:使用Document元素获取元数据信息
（书26-2）
```
  <script>
        document.writeln("<pre>");
        document.writeln("characterSet:"+document.characterSet);//返回文档字符编码集，只读
        document.writeln("charset:"+document.charset);//获取或设置文档的字符编码集
        document.writeln("campatMode:"+document.compatMode);//获取文档的兼容性模式,属性值有两个：CSS1Compat(遵循有效的某HTML规范)、BackCompat(含非标准功能，已触发怪异模式)
        document.writeln("defaultCharset:"+document.defaultCharset);//获取浏览器的默认字符编码
        document.writeln("dir:"+document.dir);//获取或设置文档的文本方向（无显示？？）
        document.writeln("domain:"+document.domain);//获取或设置当前文档的域名（无显示？？）
        document.writeln("lastModified:"+document.lastModified);//返回文档的最后修改时间
        document.writeln("referrer:"+document.referer);//返回连接到当前文档的URL（对应HTTP标头的值）
        document.writeln("title:"+document.title);//获取或设置文档标题
    </script>
```

## 26.1.2 使用location对象
- document.location属性返回一个Location对象，该对象提供了细粒度的文档地址信息
### eg1: location对象获取文档信息***待在wetryer上试***
(书26-3）
```
    <script>
        document.writeln("<pre>");
        document.writeln("protocol:"+document.location.protocol);
        document.writeln("host:"+document.location.host);
        document.writeln("hostname:"+document.location.hostname);
        document.writeln("port:"+document.location.port);//端口号为默认的80时，属性不会返回值
                                                         //关于端口的理解有待学习！！！
		document.writeln("pathname:"+document.location.pathname);
        document.writeln("search:"+document.location.search);
        document.writeln("hash:"+document.location.hash);
        document.write("</pre>");
    </script>
```
### eg2: ocation.hash方法导航到文档某个地方 ***USEFUL!!***
(书26-4）
- document.location.hash将文档当前位置导航到文档其他地方
- hash即获取或设置文档URL的锚（井号串）部分
- 也可用你location.href属性导航:要设置完整URL
```
<!DOCTYPE html>
<html>
<head>
    <title>Chapter26</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
</head>
<body>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties of banana alone.
        By the time we add the countless types of apples,oranges,and other well-known fruit,we are faced with thousands of choices.
    </p>
    
    <button id="pressme">Press Me</button>
    
    <p>
    One of the most interesting aspects of fruit is the variety available in each country.
    I live near London, in an area which is known for its apples.
    </p>
    
    <img id="qq1" src="qq.png" alt="cute qq"/>
    <script>
        document.getElementById("pressme").onclick=function(){
            document.location.hash="qq1";
        }
    </script>
</body>
</html>
```
### eg3:location.assign方法导航
- location.assign导航到指定URL上
- replace和assign区别：replace导航会把当前文档从浏览器历史中删除
```
		<button id="pressme">Press Me</button>
        <img id="qq" src="qq.png" alt="cute qq"/>
        <script>
            document.getElementById("pressme").onclick=function(){
                document.location.assign("http://apress.com");
            }
        </script>
```

## 26.1.3 读取和写入cookie ***待在wetryer上试***
（书26-6）
- 代码不能成功！！！？？？
```
 <p id="cookiedata">
        AsaDSCdadf adad
    </p>
    
    <button id="write">Add Cookie</button>
    <button id="update">Update Cookie</button>
    
    <script>
        var cookieCount = 0;
        document.getElementById("update").onclick=updateCookie;
        document.getElementById("write").onclick=createCookie;
        readCookies();
        
        function readCookies() { //该函数读取document.cookie的值
            document.getElementById("cookiedata").innerHTML=document.cookie;
        }
        
        function createCookie() { //该函数给cookie属性指派一个新值，该新值会被添加到cookie集合中
            cookieCount++;
            document.cookie="Cookie_"+cookieCount+"=Value_"+cookieCount;
            readCookies();
        }
        
        function updateCookie(){  //该函数给某个现有的cookie提供一个新值
            document.cookie="Cookie_"+cookieCount+"=Value_"+cookieCount;
            readCookies();
        }
    </script>
```

## 26.1.4 readystate属性返回就绪状态

- readystate属性：随着浏览器逐步加载和处理文档，readystate属性值变化 loading→interactive→complete，其与readystatechange事件结合使用时用处最大
readyState属性返回的值

值|说明
---|---
loading|浏览器正在加载和处理此文档
interactive|文档已被解析，但浏览器还在加载其中资源
complete|文档已被解析，所有资源也加载完毕

- readystatechange事件：会在每次readystate值发生变化时触发
- innerHTML属性设置或返回开始和结束标签之间的HTML
### eg:使用文档状态来推迟脚本的执行
（书26-7）
- 推迟脚本执行方法1：浏览器遇到文档里的script就会立即执行脚本，但当浏览器在遇到带有defer属性的script元素是，会将加载和执行script推迟到HTML中所有元素都解析之后
- 推迟脚本执行方法2：使用文档状态，如本例所示***如果readyStatechange事件仅仅在readyState值变化时触发，那就仅仅是两个瞬间啊。。。不理解。。。而且，为何(document.readyState=="interactive")时才执行的语句，最后显示的readyState为complete***
```
<!DOCTYPE html>
<html>
<head>
    <title>Chapter26</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <script>
        document.onreadystatechange=function(){
            if(document.readyState=="interactive"){
                document.getElementById("pressme").onclick=function(){
                    document.getElementById("results").innerHTML=document.readyState;
                }
            }
        }
    </script>
    
</head>

<body>
    <button id="pressme">Press Me</button>
    <pre id="results"></pre>
    
</body>
</html>
```
- 此处script写在head部分，关于其写在head和body中的区别，见<http://blog.csdn.net/boli1020/article/details/12949987>

## 26.1.5 document.implementation属性获取DOM的实现情况
- document.implementation属性：提供了浏览器对DOM功能的实现信息，该属性返回一个DOMImplementation对象，该对象包含了一个方法：hasFeature方法
- hasFeature方法：可判断哪些DOM功能已经实现
### eg:使用document.implementation.hasFeature方法 
（书26-8）
***循环的写法挺值得借鉴***
```
   <script>
        var features=["Core","HTML","CSS","Selectors-API"];
        var levels=["1.0","2.0","3.0"];
        
        document.writeln("<pre>");//writeln方法将内容附加到HTML文档末尾,内容尾部有行结束符
        for(var i=0;i<features.length;i++){
            document.writeln("Checking for feature:"+features[i]);
            for(var j=0;j<levels.length;j++){
                document.write(features[i]+"level"+levels[j]+":");
                                 //writeln方法将内容附加到HTML文档末尾,内容无行结束符——即下面的可以接在这一行后面写
                                 
                document.writeln(document.implementation.hasFeature(features[i],levels[j]));
            }
        }
        document.write("</pre>")  
    </script>
```



## 26.2 获取HTML元素对象
## 26.2.1使用属性获取元素对象
### eg:以document.images为例使用HTMLCollection对象
(书26-9）
- document.images属性获得了一个HTMLCollection,该HTMLCollection包含了所有代表文档里img元素的对象
- 方法一：标准的JavaScript数组索引标记（element[i]这种表示法）：可以来直接访问HTML集合里的各个对象
- 方法二：namedItem方法：它会返回集合里带有指定id或name属性值的项目
```
<!DOCTYPE html>
<html>
<head>
    <title>Chapter26</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        pre{
            border: medium double black;
        }
    </style>
</head>

<body>
    <pre id="results"></pre>
    
    <img id="lemon" src="lemon.png" alt="lemon"/>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties of banana alone.
        By the time we add the countless types of apples,oranges, and other well-known fruit, we are faced with thousands of choices.
    </p>
    
    <img id="apple" src="apple.png" alt="apple"/>
    <p>
        One of the most interesting asects of fruit is the variety availabel in each country.
        I live near London,in an area which is known for its apples.
    </p>
    
    <img id="banana" src="banana.png" alt="small banana"/>
    <script>
        var resultsElement=document.getElementById("results");
        
        var elems=document.images;
        //document.images属性获得了一个HTMLCollection,它包含了所有代表文档里img元素的对象
        
        for (var i=0;i<elems.length;i++) {
            resultsElement.innerHTML+="Image Element:"+elems[i].id+"\n";//code
        }
        //方法一：使用标准的JavaScript数组索引标记（element[i]这种表示法）来直接访问集合里的各个对象
        
        var srcValue=elems.namedItem("apple").src;
        resultsElement.innerHTML+="src for apple element is:"+srcValue+"\n";
        //方法二：使用namedItem方法，它会返回集合里带有指定id或name属性值的项目，
        //此处获取代表某个特定img元素的对象
       //此处读取了这个对象的src属性值，这个属性值由HTMLImageElement对象实现，
		//HTMLImageElement对象用于代表img元素
    </script>
 
</body>
</html>

```


## 26.2.2 使用数组标记获取已命名元素
- 使用数组风格的索引标记来获取代表某个id值为apple元素的对象（即document["apple"]）
- document["id/name"]的深度优先原则：<br>
遵循浏览器“深度优先”（depth-first）顺序,以此顺序看待文档里的所有元素，尝试将id或name属性与指定值进行匹配；若第一次匹配到的是id属性，那么浏览器就停止搜索并返回代表匹配元素的HTMLELEMENT对象；若第一次匹配到的是name属性，那么将得到一个HTMLELEMENT(若只有一个匹配元素)，或一个HTMLCollection（如果不止有一个匹配的元素）；浏览器开始匹配name值就不会再匹配id值了
### eg :使用数组标记获取已命名或有id值的元素的对象
（书26-10）
```
<!DOCTYPE html>
<html>
<head>
    <title>Chapter26</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        pre{
            border: medium double black;
        }
    </style>
</head>

<body>
    <pre id="results"></pre>
    
    <img id="lemon"  name="image" src="lemon.png" alt="lemon"/>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties of banana alone.
        By the time we add the countless types of apples,oranges, and other well-known fruit, we are faced with thousands of choices.
    </p>
    
    <img id="apple" name="image" src="apple.png" alt="apple"/>
    <p>
        One of the most interesting asects of fruit is the variety availabel in each country.
        I live near London,in an area which is known for its apples.
    </p>
    
    <img id="banana" src="banana.png" alt="small banana"/>
    
    <script>
        var resultsElement=document.getElementById("results");
        
        var elems=document["image"];
        //使用数组风格的索引标记来获取代表某个id值为apple元素的对象
        //遵循浏览器“深度优先”（depth-first）顺序,以此顺序看待文档里的所有元素，尝试将id或name属性与指定值进行匹配
        //若第一次匹配到的是id属性，那么浏览器就停止搜索并返回代表匹配元素的HTMLELEMENT对象
        //若第一次匹配到的是name属性，那么将得到一个HTMLELEMENT(若只有一个匹配元素)，或一个HTMLCollection（如果不止有一个匹配的元素）
        //浏览器开始匹配name值就不会再匹配id值了。
        
        if (elems.namedItem)//把namedItem属性当做测试项来判断得到的是HTMLElement,还是HTMLCollection,
                            //如果是集合，namedItem返回true;是Element,返回false
        {
            for(var i=0;i<elems.length;i++)//
            {
                resultsElement.innerHTML+="Image Element:"+elems[i].id+"\n";
            }
        }
        else
        {
            resultsElement.innerHTML+="Src for element is:"+elems.src+"\n";
        }
    </script>
 
</body>
</html>
```

## 26.2.3 使用Document方法获取元素
属性 |说明|返回
-----|----|----
getElementById(<id>)|返回带有指定id值的元素|HTMLElement
getElementsByClassName(<class>)|返回带有指定class值的元素|HTMLElement[]
getElementsByName(<name>)|返回带有指定name值的元素|HTMLElement[]
getElementsByTagName(<tag>)|返回指定类型(标签）的元素|HTMLElement[]
querySelector(<selector>)|返回匹配指定CSS选择器的第一个元素|HTMLElement
querySelectorAll(<selector>)|返回匹配指定CSS选择器的所有元素|HTMLElement[]

### eg1:使用document.getElement开头的方法
（书26-11）
```
<!DOCTYPE html>
<html>
<head>
    <title>Chapter26</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        pre{
            border: medium double black;
        }
    </style>
</head>

<body>
    <pre id="results">
    </pre>
    
    <img id="lemon"  class="fruits" name="image" src="lemon.png" alt="lemon"/>
    <p>
        There are lots of different kinds of fruit - there are over 500 varieties of banana alone.
        By the time we add the countless types of apples,oranges, and other well-known fruit, we are faced with thousands of choices.
    </p>
    
    <img id="apple" class="fruits" name="image" src="apple.png" alt="apple"/>
    <p>
        One of the most interesting asects of fruit is the variety availabel in each country.
        I live near London,in an area which is known for its apples.
    </p>
    
    <img id="banana" src="banana.png" alt="small banana"/>
    
    <script>
        var resultsElement=document.getElementById("results");//返回带有指定id值的元素
        
        var pElems=document.getElementsByTagName("p");//返回带有指定标签的元素
        resultsElement.innerHTML +="There are"+pElems.length+" p elements\n";
        
        var fruitsElems=document.getElementsByClassName("fruits");//返回指定class值的元素
        resultsElement.innerHTML +="There are"+fruitsElems.length+" elements in the fruits class\n";
        
        var nameElems=document.getElementsByName("image");//返回指定name值的元素
        resultsElement.innerHTML +="There are"+nameElems.length+" elements with the name 'image'"
     
    </script>
 
</body>
</html>
```
### eg2:querySelect开头的方法
其他document方法很难达到同样的效果，通常使用此类选择器的比例会更高

```
    <script>
        var resultsElement =document.getElementById("results");
        var csselems=document.querySelectorAll("p,img#apple");//返回所有p元素，和id值为apple的img元素
        resultsElement.innerHTML +="The selector matched "+csselems.length+" elements\n";
    </script>
```

## 26.2.4 使用合并进行链式搜索
- 几乎所有Document对象实现的方法都能应用于HTMLElement对象（即可以链式操作，先应用于Document对象选出HTMLElement对象，再应用于HTMLELement对象选出更细一层的HTMLElement对象）
- 唯一例外是getElementById方法，只能用于Document对象（即不能用于链式操作的后一级）
```
<!DOCTYPE html>
<html>
<head>
    <title>Chapter26</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        pre{
            border: medium double black;
        }
    </style>
</head>

<body>
    <pre id="results"></pre>
    
    <p id="tblock">
        There are lots of different kinds of fruit -
        there are over 500 varieties of<span id="banana">banana</span>alone.
        By the tiem we add the countless tyes of <span id="apple">apples</span>,
        <span="orange">oranges</span="orange">,and other well-known fruit,
        we are faced with thousands of choices.
    </p>
    
    <script>
        var resultsElement = document.getElementById("results");
        
        var elems = document.getElementById("tblock").getElementsByTagName("span");
        //getElementById再加上getElementsByTagName,返回位于id为tblock的p元素内的span元素集合
        resultsElement.innerHTML+= "There are "+elems.length+" span elements\n";
        
        var elems2 = document.getElementById("tblock").querySelectorAll("span");
        //getElementById再加上querySelectorAll,返回位于id为tblock的p元素内的span元素集合
        resultsElement.innerHTML+="There are "+elems2.length + " span elements(Mix)\n";
        
        var selElems = document.querySelectorAll("#tblock > span");
        //单独使用CSS选择方法达到同样效果
        resultsElement.innerHTML+="There are"+selElems.length+" span elements(CSS)\n";
    </script>
   
</body>
</html>

```

## 26.3 在Dom树里导航
将Dom视为一棵树，在它的层级结构里导航。所有Dom对象都支持如下属性：
属性|说明|返回
----|----|----
childNodes|返回子元素组|HTMLElement[]
firstChild|返回第一个子元素|HTMLElement
hasChildNodes()|如果当前元素有子元素就返回true|布尔值
lastChild|返回倒数第一个子元素|HTMLElement
nextSibling|返回定义在当前元素之后的兄弟元素|HTMLElement
parentNode|返回父元素|HTMElement
previousSibling|返回定义在当前元素之前的兄弟元素|HTMLElement


### eg:在DOM树里导航
***为什么某个元素的第一个子元素总是object text??***
**已解决：所有的空格，包括标签另起一段书写都会生成object text**
```
<!DOCTYPE html>
<html>
<head>
    <title>Chapter26</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style>
        pre{
            border: medium double black;
        }
    </style>
</head>

<body>
<pre id="results"></pre>
    <p id="tblock">
        There are lots of different kinds of fruit -
        there are over 500 varieties of<span id="banana">banana</span>alone.
        By the tiem we add the countless tyes of <span id="apple">apples</span>,
        <span="orange">oranges</span="orange">,and other well-known fruit,
        we are faced with thousands of choices.
    </p>
    <img id="apple" class="fruits images" name="apple" src="apple.png" alt="apple"/>
    <img id="banana" src="banana.png" alt="banana"/>
    <p>
        One of the most interesting aspects of fruit is the variety available in each country.
        I live near London, in an area which is known for its apples.
    </p>
    <p>
        <button id="parent">Parent</button>
        <button id="child">First Child</button>
        <button id="prev">Prev Sibling</button>
        <button id="next">Next Sibling</button>
    </p>
    
    <script>
        var resultsElem=document.getElementById("results");
        var element=document.body;
        
        var buttons=document.getElementsByTagName("button");
        for (var i=0;i<buttons.length;i++)
        {
            buttons[i].onclick=handleButtonClick;//电击按钮时执行函数handleButtonClick
        }
        
        processNewElement(element);//对默认的element执行processNewElement函数，默认element即document.body
        
        function handleButtonClick(e)
        {
            if (element.style){
                element.style.backgroundColor="white";
            }
            
            if (e.target.id=="parent"&& element!= document.body){
                element=element.parentNode;
            }
            else if (e.target.id=="child"&& element.hasChildNodes()){
                element=element.firstChild;
            }
            else if (e.target.id=="prev"&&element.previousSibling){
                element=element.previousSibling;
            }
            else if (e.target.id=="next"&&element.nextSibling) {
                element=element.nextSibling;
            }
            processNewElement(element);
            if (element.style)
            {
                element.style.backgroundColor="lightgrey";
            }
        }
        
        function processNewElement(elem)
        {
            resultsElem.innerHTML="Element type:"+elem+"\n";
            resultsElem.innerHTML+="Element id:"+elem.id+"\n";
            resultsElem.innerHTML+="Has child nodes:"+elem.hasChildNodes()+"\n";
            if (elem.previousSibling)
            {
                resultsElem.innerHTML+=("Prev sibling is:"+elem.previousSibling+"\n");
            }
            else
            {
                resultsElem.innerHTML+="No prev sibling\n";
            }
            if (elem.nextSibling)
            {
                resultsElem.innerHTML+="Next sibling is:"+elem.nextSibling+"\n";
            }
            else
            {
                resultsElem.innerHTML+="No next sibling\n";
            }
        }
        
        
        
    </script>
   
</body>
</html>
```

