# Chapter28 使用Dom元素
## 28.1使用元素对象
### eg1 使用基本元素数据属性
（书28-1）
- 使用元素数据属性获取某个元素的信息
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        p{
            border:medium double black;
        }
    </style>
</head>

<body>
    <p id="textblock" dir="ltr" lang="en-US"><!--dir属性：规定元素中内容的文本方向；lang属性：规定元素内容的语言-->
        There are lots of different kinds of fruits - there are over 500 varieties of <span id="banana">banana</span> alone.
        By the time we add the countless types of <span id="apple">apples</span>,
        <span="orange">oranges</span>,and other well-known fruit,we are faced with thousands of choices.
    </p>
    <pre id="results"></pre>
    <script>
        var results=document.getElementById("results");
        var elem=document.getElementById("textblock");
        
        results.innerHTML+="tag:"+elem.tagName+"\n";//tagName属性：返回标签名
        results.innerHTML+="id:"+elem.id+"\n";//id属性：获取或设置id属性的值
        results.innerHTML+="dir:"+elem.dir+"\n";//dir属性：获取或设置dir属性的值
        results.innerHTML+="lang:"+elem.lang+"\n";//lang属性：获取或设置lang属性的值
        results.innerHTML+="disabled:"+elem.disabled+"\n";//disabled属性：获取或设置disabled属性是否存在
//disabled:禁用input元素
    </script>

</body>
</html>


```

##  28.1.1使用类
### eg1: 通过className属性使用类
(书28-2）
- 使用className属性返回一个类的列表。通过改变返回的值，就能添加和移除类。
- 类的一个常见用途是 有针对性地给元素应用样式。

- **必须给附加到className属性的值添加一个前置空格！！！** 因为浏览器期望获得由空格间隔的类列表，只有这样，浏览器才会应用那些基于类选择器的样式。

```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        p{
            border:medium double black;
        }
        p.newclass{
            background-color: grey;
            color: white;
        }
    </style>
</head>

<body>
    <p id="textblock" class="fruit numbers">
        There are lots of different kinds of fruits - there are over 500 varieties of <span id="banana">banana</span> alone.
        By the time we add the countless types of apples,
       oranges,and other well-known fruit,we are faced with thousands of choices.
    </p>
    <button id="pressme">
        Press Me
    </button>
    <script>
        document.getElementById("pressme").onclick = function(e){//此处e加不加都行，反正用不上其指向的事件
            document.getElementById("textblock").className += " newclass";//newclass前必须带空格，后面注释有详解
            //点击按钮后，是一个新的类（"newclass"）被附加到元素的类列表上。
            //必须给附加到className属性的值添加一个前置空格！！！因为浏览器期望获得由空格间隔的类列表，只有这样，浏览器才会应用那些基于类选择器的样式。
        };
    </script>
</body>
</html>
```

### eg2： 使用classList属性添加、移除类 ***Very Good***
（书28-3）
- classList属性返回的是一个DOMTokenList对象
- DOMTokenList对象有一些方法和属性来管理类列表，如下表：
<center>DOMTokenList成员</center>

成员|说明|返回
----|----|-----
add(<class>)|给元素添加指定的类|void
contains(<class>)|如果元素属于指定的类就返回true|布尔值
length|返回元素所属类的数量|数值
remove(<class>)|从元素上移除指定的类|void
toggle(<class>)|如果类不存在就添加它，如果存在就移除它|布尔值

```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        p{
            border:medium double black;
        }
        p.newclass{
            background-color: grey;
            color: white;
        }
    </style>
</head>

<body>
    <p id="textblock" class="fruit numbers">
        There are lots of different kinds of fruits - there are over 500 varieties of <span id="banana">banana</span> alone.
        By the time we add the countless types of apples,
       oranges,and other well-known fruit,we are faced with thousands of choices.
    </p>
    <pre id="results">
    </pre>
    <button id="toggle">
        Toggle Class
    </button>
    <script>
        var results=document.getElementById("results");
        document.getElementById("toggle").onclick=toggleClass;
        
        listClasses();
        
        function listClasses() {
            var classlist=document.getElementById("textblock").classList;
                  //使用classList属性来获取和枚举p元素所属的类，并在下文使用数组风格的索引表示法得到类名
            results.innerHTML="Current classes:"
            for(var i=0;i<classlist.length;i++){
                results.innerHTML+=classlist[i]+" ";
            }
        }
        
        function toggleClass() {
            document.getElementById("textblock").classList.toggle("newclass");
                  //使用toggle方法添加和移除一个名为newclass的类，该类关联了一个样式
            listClasses();
                  //再调用listClasses()函数得到此事的类名列表
        }
    </script>
 
</body>
</html>
```
## 28.1.2使用元素属性
- HTMLElement对象既有一些属性来对应重要的HTML全局属性，又支持对单个元素的任意属性进行读取和设置
<center>与HTML属性相关的属性和方法</center>

成员|说明|返回
----|----|----
attributes|返回应用到元素上的属性|Attr[]
dataset|返回以data-开头的属性|字符串数组[<name>]
getAttribute(<name>)|返回指定属性的值|字符串
hasAttribute(<name>)|如果元素带有指定属性则返回true,否则false|布尔值
removeAtrribute(<name>)|从元素上移除指定属性|void
setAttribute(<name>,<value>)|应用一个指定名称和值的属性|void

### eg1使用属性方法
(书28-4）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        p{
            border:medium double black;
        }
    </style>
</head>

<body>
    <p id="textblock" class="fruit numbers">
        There are lots of different kinds of fruits - there are over 500 varieties of <span id="banana">banana</span> alone.
        By the time we add the countless types of apples,
       oranges,and other well-known fruit,we are faced with thousands of choices.
    </p>
    <pre id="results">
    </pre>

    <script>
        var results=document.getElementById("results");
        var elem=document.getElementById("textblock");
        
        results.innerHTML="Element has lang attribute: "+elem.hasAttribute("lang")+"\n";//hasAttribute:如果元素带有指定属性则返回true
        results.innerHTML+="Adding lang attribute\n";
        elem.setAttribute("lang","en-US");//setAttribute:指定某属性的值
        results.innerHTML += "Attr value is：" +elem.getAttribute("lang")+"\n";//getAttribute:返回指定属性的值
        results.innerHTML += "Set new value for lang attribute\n";
        elem.setAttribute("lang","en-UK");
        results.innerHTML += "Value is now: "+elem.getAttribute("lang")+"\n";
    </script>
 
</body>
</html>
```
### eg2 使用dataset属性
（书28-5）
- HTML支持带data-前缀的自定义属性。DOM里可以通过dataset属性来操作这些自定义属性
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        p{
            border:medium double black;
        }
    </style>
</head>

<body>
    <p id="textblock" class="fruit numbers" data-fruit="apple" data-sentiment="like"><!-- 带data-前缀为自定义属性-->
        There are lots of different kinds of fruits - there are over 500 varieties of <span id="banana">banana</span> alone.
        By the time we add the countless types of apples,
       oranges,and other well-known fruit,we are faced with thousands of choices.
    </p>
    <pre id="results">
    </pre>

    <script>
        var results=document.getElementById("results");
        var elem=document.getElementById("textblock");
        
        for (var attr in elem.dataset) {//dataset属性返回的值数组不像通常数组那样根据位置进行索引，而采用类型Python中的循环方法索引
            results.innerHTML+=attr+"\n";
        }
        
        results.innerHTML+="Value of data-fruit attr: "+elem.dataset["fruit"];//获得data-fruit属性的值
    </script>
 
</body>
</html>
```

### eg3 通过attributes属性使用所有属性
- 可以通过attributes属性获得一个包含某元素所有属性的集合，它会返回一个由Attr对象构成的数组；
- Attr对象的属性有： name—返回属性的名称；value—获取**或设置**属性的值（感觉类型python字典）

```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        p{
            border:medium double black;
        }
    </style>
</head>

<body>
    <p id="textblock" class="fruit numbers" data-fruit="apple" data-sentiment="like"><!-- 带data-前缀为自定义属性-->
        There are lots of different kinds of fruits - there are over 500 varieties of <span id="banana">banana</span> alone.
        By the time we add the countless types of apples,
       oranges,and other well-known fruit,we are faced with thousands of choices.
    </p>
    <pre id="results">
    </pre>

    <script>
        var results=document.getElementById("results");
        var elem=document.getElementById("textblock");
        
        var attrs=elem.attributes;
        
        for(var i=0;i<attrs.length;i++){
            results.innerHTML += "Name: "+attrs[i].name+" Value: "+attrs[i].value+"\n";
        }//Attr对象数组根据位置进行索引
        
        attrs["data-fruit"].value="banana";//Atrr对象根据名称索引，修改了值
        
        results.innerHTML +="Value of data-fruit attr: "+attrs["data-fruit"].value;
    </script>
 
</body>
</html>
```

## 28.2 使用Text对象
- 元素的文本内容是由Text对象代表的，它在文档模型里表现为元素的子对象。
- p元素自身会对应一个HTMLElement对象，其内容则对应一个Text对象。

Text对象的成员
成员 |说明|返回
-----|-----|----
appendData(<string>)|把指定字符串加到文本末尾|void
data|获取或设置文本|字符串
deleteData(<offset>,<count>)|从文本中年移除字符串，第一个参数是偏移量，第二个是要移除的字符数量|void
insertData(<offset>,<string>)|在指定偏移量处插入指定字符串|void
length|返回字符的数量|数值
replaceData(<offset>,<count>,<string>)|用指定字符串替换一段文本|void
replaceWholeText(<string>)|替换全部文本|Text
splitText(<number>)|将现有Text元素在指定偏移量处一分为二|Text
substringDat(<offset>,<count>)|返回文本的字符串|字符串
wholeText|获取文本|字符串

### eg1获取Text对象
书28-8
- p中添加其他元素（如b元素/span元素），则其内容节点层级结构发生改变。其第一个子对象是p中的文本块开头到b元素的文本；第二个子对象是b元素开始、介绍标签之间的文本；第三个子对象就是b结束标签到p结束标签
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        p{
            border:medium double black;
        }
    </style>
</head>

<body>
    <p id="textblock" class="fruit numbers" data-fruit="apple" data-sentiment="like">
       There  are lots of different kinds of fruits - there are over  <b>500</b> varieties of banana alone.
        By the time we add the countless types of apples,
       oranges,and other well-known fruit,we are faced with thousands of choices.
    </p>
    
    <button id="pressme">
        Press Me
    </button>
    
    <pre id="results">
    </pre>

    <script>
        var results=document.getElementById("results");
        var elem=document.getElementById("textblock");
        
        document.getElementById("pressme").onclick =function(){
            var textElem=elem.firstChild;
                //.firstChild取出p元素的第一个Text子对象--p后到第一个b,中间空格也算
            results.innerHTML = "The element has "+textElem.length+" chars\n";
                //p元素的第一个Text子对象的字符数量
            textElem.replaceWholeText("This is a new string ");
                //replaceWholeText方法可修改这个对象的全部内容，貌似无该方法？？
        };
    </script>
 
</body>
</html>
```

## 28.3 修改Dom层级结构
修改DOM层级结构的属性和方法

成员|说明|返回
----|----|------
appendChild(HTMLElement)|将指定元素添加为当前元素的（最后一个）子元素|HTMLElement
cloneNode(boolean)|复制一个元素|HTMLElement
compareDocumentPosition(HTMLElement)|判断一个元素的相对位置|数值
innerHTML|获取或设置元素的内容|字符串
insertAdjacentHTML(<pos>,<text>)|相对于元素插入HTML|void
insertBefore(<newElem>,<childElem>)|在第二个（子元素）之前插入一个元素|HTMLElement
isEqualNode(<HTMLElement>)|判定元素是否与当前元素相同|布尔值
isSameNode(HTMLElement)|判断指定元素是否就是当前元素|布尔值
outerHTML|获取或设置某个元素的HTML和女儿|字符串
removeChild(HTMLElement)|从当前元素上移除指定的子元素|HTMLElement
replaceChild(HTMLElement,HTMLElement)|替换当前元素的某个子元素|HTMLElement
两个允许你创建新元素的方法

成员|说明|返回
----|----|---
createElement(<tag>)|创建一个属于指定标签类型的新HTMLElement对象|HTMLElement
createTextNode(<text>)|创建一个带有指定内容的新Text对象|Text

## 28.3.1 创建和删除元素
### eg1 创建和删除元素 ***Good***
(书 28-10)
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 10px;
        }
        td{
            padding: 4px 5px;
        }
    </style>
</head>

<body>
   <table border="1">
    <thead><th>Name</th><th>Color</th></thead>
    <tbody id="fruitsBody">
        <tr><td>Banana</td><td>Yellow</td></tr>
        <tr><td>Apple</td><td>Red/Green</td></tr>
    </tbody>
   </table>
   
   <button id="add">Add Element</button>
   <button id="remove">Remove Element</button>
   
   <script>
        var tableBody= document.getElementById("fruitsBody");
        
        document.getElementById("add").onclick=function(){
            var row=tableBody.appendChild(document.createElement("tr"));
                //appendChild：将指定元素添加为当前元素的子元素
                //createElement(<tag>):创建一个属于标签类型的新HTMLElement对象
            row.setAttribute("id","newrow");
                //setAttribute(<name>,<value>):应用一个指定名称和值的属性
            row.appendChild(document.createElement("td")).appendChild(document.createTextNode("Plum"));
                //createTextNode:创建一个带有指定内容的新Text对象
            row.appendChild(document.createElement("td")).appendChild(document.createTextNode("Purple"));
        }
        
        document.getElementById("remove").onclick=function(){
            var row=document.getElementById("newrow");
            row.parentNode.removeChild(row);//parentNode:先导航至父元素；removeChild:从当前元素上移除指定的子元素
        }
   </script>
 
</body>
</html>
```
## 28.3.2复制元素
### eg2 复制元素
- 使用cloneNode方法复制现有元素
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 10px;
        }
        td{
            padding: 4px 5px;
        }
    </style>
</head>

<body>
   <table border="1">
    <thead><tr><th>Multiply</th><th>Result</th></tr></thead>
    <tbody id="fruitsBody">
        <tr><td class="sum">1*1</td><td class="result">1</td></tr>
    </tbody>
   </table>
   
   <button id="add">Add Row</button>
   
   <script>
        var tableBody= document.getElementById("fruitsBody");
        
        document.getElementById("add").onclick=function(){
           var count=tableBody.getElementsByTagName("tr").length+1;
           var newElem=tableBody.getElementsByTagName("tr")[0].cloneNode(true);
            //cloneNode方法复制元素，指定其布尔值为true表示应该复制该元素的所有子元素
           newElem.getElementsByClassName("sum")[0].firstChild.data=count+"*"+count;
			//.data:Text对象成员，获取或设置文本
           newElem.getElementsByClassName("result")[0].firstChild.data=count*count;
           
           tableBody.appendChild(newElem);
        }
        
      
   </script>
 
</body>
</html>
```

- 理解后自己的写法：***Good***
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 10px;
        }
        td{
            padding: 4px 5px;
        }
    </style>
</head>

<body>
   <table border="1">
    <thead><tr><th>Multiply</th><th>Result</th></tr></thead>
    <tbody id="fruitsBody">
        <tr><td class="sum">1*1</td><td class="result">1</td></tr>
    </tbody>
   </table>
   
   <button id="add">Add Row</button>
   
   <script>
       document.getElementById("add").onclick=addcopyrow;
       
       function addcopyrow() {
         var copyobject=document.getElementById("fruitsBody");
         var count=copyobject.getElementsByTagName("tr").length+1;
         var newrow=copyobject.getElementsByTagName("tr")[0].cloneNode(true);
         newrow.getElementsByClassName("sum")[0].firstChild.data=count+"*"+count;
         newrow.getElementsByClassName("result")[0].firstChild.data=count*count;
         copyobject.appendChild(newrow);
       } 
   </script>
 
</body>
</html>
```
- 补充说明：本例为设置单元格文本采用处理Text对象的方法，更好的方法见28.3.5节

## 28.3.3移动元素
- 移动元素，即把待移动元素关联到新的父元素上，不需要让该元素脱离其初始位置
### eg:把表格的一行移至另一行
(书28-12）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 10px;
            float: left;
        }
        td{
            padding: 4px 5px;
        }
        p{
            clear: left;
        }
    </style>
</head>

<body>
   <table border="1">
        <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
        <tbody>
            <tr><td>Banana</td><td class="result">Yellow</td></tr>
            <tr id="apple"><td>Apple</td><td>Red/Green</td></tr>
        </tbody>
   </table>
   
   <table border="1">
        <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
        <tbody id="fruitsBody">
            <tr><td>Plum</td><td>Purple</td></tr>
        </tbody>
   </table>
   
   <p>
    <button id="move">Move Row</button>
   </p>
   
   <script>
        document.getElementById("move").onclick=function(){
            var elem=document.getElementById("apple");
            document.getElementById("fruitsBody").appendChild(elem);
            //在另一个id的元素上调用appendChild方法，从而实现把该行从一个表格移动至另一个表格
        }
   </script>
   
   
 
</body>
</html>
```
- 理解后自己写的script部分：
```
   <script>
       document.getElementById("move").onclick=moverow;
       
       function moverow() {
        var movedrow=document.getElementById("fruitsBody").getElementsByTagName("tr")[0];
        document.getElementsByTagName("tbody")[0].appendChild(movedrow);
       }
   </script>
```

## 28.3.4 比较元素对象
两种方式可以比较元素对象。
1. 用isSameNode方法检查它们是否代表了同一个元素
2. 用isEqualNode方法测试元素对象是否相同
### eg1用isSameNode方法
(书28-13）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 10px;
        }
        td{
            padding: 4px 5px;
        }
    </style>
</head>

<body>  
   <table border="1">
        <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
        <tbody id="fruitsBody">
            <tr id="plumrow"><td>Plum</td><td>Purple</td></tr>
        </tbody>
   </table>
   
   <pre id="results"></pre>
   
   <script>
       //两种技巧定位元素对象
       var elemByID=document.getElementById("plumrow");//1.通过id搜索
       var elemByPos=document.getElementById("fruitsBody").getElementsByTagName("tr")[0];//2.通过父元素里的标签类型搜索
       
       if (elemByID.isSameNode(elemByPos)) {//isSameNode比较这俩元素，返回true
        document.getElementById("results").innerHTML="Objects are the same";
       }
   </script>
   
   
 
</body>
</html>
```
### eg2:用isEqualNode方法
（书28-14）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 2px 0px;
        }
        td{
            padding: 4px 5px;
        }
    </style>
</head>

<body>  
   <table border="1">
        <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
        <tbody>
            <tr class="plumrow"><td>Plum</td><td>Purple</td></tr>
        </tbody>
   </table>
   
    <table border="1">
        <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
        <tbody>
            <tr class="plumrow"><td>Plum</td><td>Purple</td></tr>
        </tbody>
   </table>
   
   <pre id="results"></pre>
   
   <script>
       var elems=document.getElementsByClassName("plumrow");
       
       if (elems[0].isEqualNode(elems[1])) {//isEqualNode方法测试两元素是否相同
        document.getElementById("results").innerHTML="Elements are equal ";
       }
       else{
        document.getElementById("results").innerHTML="Elements are not equal ";
       }
   </script>
   
   
 
</body>
</html>
```

## 28.3.5使用HTML片段
- innerHTML属性,outerHTML属性,insertAdjacentHTML方法都是语法捷径，通过它们可使用HTML片段，不用再像28.3.2那样创建元素和文本对象的详细层级结构
1. innerHTML:获取或设置元素的内容
2. outerHTML:获取或设置元素的HTML和内容
### eg1查看innerHTML和outerHTML属性
(书28-15)
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 5px 2px;
            float: left;
        }
        td{
            padding: 4px 5px;
        }
        p{
            clear: left;
        }
    </style>
</head>

<body>  
   <table border="1">
        <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
        <tbody>
            <tr id="applerow"><td>Plum</td><td>Purple</td></tr>
        </tbody>
   </table>
   
   <textarea rows="3" id="results"></textarea>
   <p>
        <button id="inner">Inner HTML</button>
        <button id="outer">Outer HTML</button>
   </p>
   
   <script>
        var results=document.getElementById("results");
        var row=document.getElementById("applerow");
        
        document.getElementById("inner").onclick=function(){
            results.innerHTML=row.innerHTML;//innerHTML只返回该元素的子元素的HTML
        }
        
        document.getElementById("outer").onclick=function(){
            results.innerHTML=row.outerHTML;//outerHTML返回定义这个元素及其所有子元素的HTML
        }
   </script>
   
</body>
</html>
```
- 自己理解后重写的script部分:
```
<script>
       var buttons=document.getElementsByTagName("button");
       var results=document.getElementById("results");
       var object=document.getElementById("applerow");
       
       for (var i=0;i<buttons.length;i++){
            if (buttons[i].id=="inner") {
                buttons[i].onclick=function(){//触发事件的函数得写在确定的元素buttons[i]后面，即此处的i须在一个循环里面，而不是循环外面的变动的i
                      results.innerHTML=object.innerHTML;
                }
            }
            else if (buttons[i].id=="outer") {
                buttons[i].onclick=function(){
                  results.innerHTML=object.outerHTML;
                }
            }
        }
   </script>
```
- 自己再次重写的标准script部分
```
 <script>
        var results=document.getElementById("results");
        var row=document.getElementById("applerow");
        
        var buttons=document.getElementsByTagName("button");
        
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=addcontent2rel;
        }
        
        function addcontent2rel(e) {
            if(e.target.id=="inner"){
                results.innerHTML=row.innerHTML;
            }
            else if (e.target.id=="outer") {
                results.innerHTML=row.outerHTML;
            }
        }
   </script>
```

### eg2 使用innerHTML和outerHTML改变文档结构
(书28-16）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 10px;
            float: left;
        }
        td{
            padding: 4px 5px;
        }
        p{
            clear: left;
        }
    </style>
</head>

<body>
    <table border="1">
        <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
        <tbody>
            <tr><td>Banana</td><td>Yellow</td></tr>
            <tr id="apple"><td>Apple</td><td>Red/Green</td></tr>
        </tbody>
   </table>
    
   <table border="1">
        <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
        <tbody id="fruitsBody">
            <tr><td>Plum</td><td>Purple</td></tr>
            <tr id="targetrow"><td colspan="2">This is the placeholder</td></tr>
        </tbody>
   </table>
   
   <p>
        <button id="move">Move Row</button>
   </p>
   
   <script>
        document.getElementById("move").onclick=function(){
            var source=document.getElementById("apple");
            var target=document.getElementById("targetrow");
            var k=target.outerHTML
            target.innerHTML=source.innerHTML;
            source.outerHTML=k;
           // source.outerHTML='<tr id="targetrow"><td colspan="2">'+'This is the placeholder</td>';
        }
   </script>
 
</body>
</html>
```
- 自己理解后新写的script部分
```
 <script>
        document.getElementById("move").onclick=function(){
            var source=document.getElementById("apple");
            var target=document.getElementById("targetrow");
            var k=target.outerHTML
            target.outerHTML=source.outerHTML;
            source.outerHTML=k;
         
        }
   </script>
```
### eg3 使用insertAdjacentHTML方法插入新元素 ***Good!正确的按钮循环遍历分别触发事件的写法***
（书28-17）
- 使用inssertAdjacentHTML方法，该方法需两个参数：第一个参数是afterbegin/afterend/beforebegin/beforeend/,第二个参数是要插入的片段。具体含义看例中注释
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 10px;
            float: left;
        }
        td{
            padding: 4px 5px;
        }
        p{
            clear: left;
        }
    </style>
</head>

<body>
    <table border="1">
        <thead><tr><th>Fruit</th><th>Color</th></tr></thead>
        <tbody id="fruitsBody">
           <tr id="targetrow"><td>Placeholder</td></tr>
        </tbody>
   </table>
    
   <p>
        <button id="ab">After Begin</button>
        <button id="ae">After End</button>
        <button id="bb">Before Begin</button>
        <button id="be">Before End</button>
   </p>
   
    <script>
        var target=document.getElementById("targetrow");
        var buttons=document.getElementsByTagName("button");
        for(var i=0;i<buttons.length;i++){
            buttons[i].onclick=handleButtonPress;
        }
        
        function handleButtonPress(e) {
            if (e.target.id=="ab") {//上下两个target完全是不一样的！
                target.insertAdjacentHTML("afterbegin","<td>After Begin</td>");
                    //将片段作为当前元素的第一个子元素插入
            }
            else if (e.target.id=="be") {
                target.insertAdjacentHTML("beforeend","<td>Before End</td>");
                    //将片段作为当前元素的最后一个子元素插入
            }
            else if (e.target.id=="bb") {
                target.insertAdjacentHTML("beforebegin","<tr><td colspan='2'>Before Begin</td></tr>");
                    //将片段插入当前元素之前
            }
            else if (e.target.id=="ae") {
                target.insertAdjacentHTML("afterend","<tr><td colspan='2'>After End</td></tr>");
            }
        }
   </script>
   
</body>
</html>
```
## 28.3.6向文本块插入元素
eg
(书28-18待深入研究）
```
<!DOCTYPE html>

<html>
<head>
    <title>Chapter28</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style>
        table{
            border: solid thin black;
            border-collapse: collapse;
            margin: 10px;
            float: left;
        }
        td{
            padding: 4px 5px;
        }
        p{
            clear: left;
        }
    </style>
</head>

<body>
    <p id="textblock">There are lots of different kinds of fruit - there are over 500 varieties of banana alone.
    </p>
    
    <p>
        <button id="insert">Insert Element</button>
    </p>
    
    <script>
        document.getElementById("insert").onclick=function(){
            var textBlock = document.getElementById("textblock");
            textBlock.firstChild.splitText(10);
            var newText=textBlock.childNodes[1].splitText(4).previousSibling;
            textBlock.insertBefore(document.createElement("b"),newText).appendChild(newText);
        }
    </script>
   
</body>
</html>
```