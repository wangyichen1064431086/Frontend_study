# Chapter29 为DOM元素设置样式
## 29.1使用样式表
- document.styleSheets属性：访问文档中可用的CSS样式表，，其返回一组对象集合，这些对象代表了各个样式表

属性|说明|返回
----|----|----
document.stylesheets|返回样式表的集合|CSSStyleSheet[]

- 一个CSSStyleSheet对象代表了一个样式表，它提供了一组属性和方法来操作文档里的样式。

CSSStyleSheet对象的成员

成员|说明|返回
----|----|----

## 29.1.1获得样式表的基本信息
### eg1 使用CSSStyleSheet对象的属性来获取样式表的基本信息 ***Good!增加表格元素记录信息的方式***
(书29-1)
```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"/>
    <style title="core styles">
        p{
            border:medium double black;
            background-color:lightgray;
        }
        #block1{
            color:white;
        }
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
    <link rel="stylesheet" type="text/css" href="styles.css"/>
    <style media="screen AND (min-width:500px)" type="text/css">
        #block2{
            color:yellow;
            font-style:italic;
        }
    </style>
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <p id="block2">
        One of the most interesting aspects of fruit is the variety available in each country.
    </p>
    <div id="placeholder"/>
    
    <script>
        var placeholder=document.getElementById("placeholder");
        var sheets=document.styleSheets;
        //通过styleSheets属性可访问文档中可用的CSS样式表，其返回一组对象集合，这些对象代表了各个样式表
        
        for (var i=0;i<sheets.length;i++) {
            var newElem=document.createElement("table");//createElement:创建一个属于指定标签类型的新HTMLElement对象
            newElem.setAttribute("border","1");//setAttribute:应用一个指定名称和值的属性
            addRow(newElem,"Index",i);
            addRow(newElem,"href",sheets[i].href);//href属性：返回链接样式表的href,href属性只有在样式表由外部文件载入时才会返回值
            addRow(newElem,"title",sheets[i].title);//title属性：返回title属性的值
            addRow(newElem,"type",sheets[i].type);//type属性：返回type属性的值
            addRow(newElem,"ownerNode",sheets[i].ownerNode.tagName);//ownerNode属性：返回样式所定义的元素
            placeholder.appendChild(newElem);
        }
        
        function addRow(elem,header,value) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td></tr>";
        }
    </script>
</body>
</html>
```
- 自己写的script部分
```
 <script>
       var results=document.getElementById("placeholder");
       
       var csssheets=document.styleSheets;
       
       for(var i=0;i<csssheets.length;i++){
          var informationtable=document.createElement("table");
          informationtable.setAttribute("border","bold");
          addrow(informationtable,"index",i);
          addrow(informationtable,"href",csssheets[i].href);
          addrow(informationtable,"title",csssheets[i].title);
          addrow(informationtable,"ownerNode",csssheets[i].ownerNode.tagName);
          
          results.appendChild(informationtable);
        }
        
        function addrow(onetable,col1,col2) {
            onetable.innerHTML+="<tr><th>"+col1+"</th><td>"+col2+"</td></tr>";
        }
    </script>
```

## 29.1.2使用媒介限制
- CSSStyleSheet.media属性：返回一个MediaList对象。定义样式表是可以使用media属性类限制样式应用的场合。

### eg:使用MediaList对象
(书29-2)
```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style title="core styles">
        p{
            border:medium double black;
            background-color:lightgray;
        }
        #block1{
            color:white;
        }
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
    <link rel="stylesheet" type="text/css" href="styles.css"/>
    <style media="screen AND (min-width:500px),PRINT" type="text/css">/*PRINT: 样式用于打印预览和打印页面时*/
        #block2{
            color:yellow;
            font-style:italic;
        }
    </style>
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <p id="block2">
        One of the most interesting aspects of fruit is the variety available in each country.
    </p>
    <div id="placeholder"/>
    
    <script>
        var placeholder=document.getElementById("placeholder");
        var sheets=document.styleSheets;
        //通过styleSheets属性可访问文档中可用的CSS样式表，其返回一组对象集合，这些对象代表了各个样式表
        
        for (var i=0;i<sheets.length;i++) {
            if (sheets[i].media.length>0) {//CSSStyleSheet.media属性：返回一个MediaList对象，该对象的length成员：返回媒介的数量
                var newElem=document.createElement("table");
                newElem.setAttribute("border","1");
                addRow(newElem,"Media Count",sheets[i].media.length);
                addRow(newElem,"Media Text",sheets[i].media.mediaText);//mediaText成员：返回media属性的文本值
                for (var j=0;j<sheets[i].media.length;j++) {
                    addRow(newElem,"Media "+j,sheets[i].media.item(j));//item(<pos>)成员：返回指定索引的媒介
                }
                placeholder.appendChild(newElem);
            }
        }
        
        function addRow(elem,header,value) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td></tr>";
        }
    </script>
</body>
</html>
```

## 29.1.3禁用样式表
- CSSStyleSheet.disabled属性：一次性启用和禁用某个样式表里的所有样式
### eg:使用disabled属性启用和禁用样式表 ***Good!不断点击按钮在两个状态间切换的好方法***
(书29-3）
```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style title="core styles">
        p{
            border:medium double black;
            background-color:lightgray;
        }
        #block1{
            color:white;
        }
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
    <link rel="stylesheet" type="text/css" href="styles.css"/>
    <style media="screen AND (min-width:500px),PRINT" type="text/css">/*PRINT: 样式用于打印预览和打印页面时*/
        #block2{
            color:yellow;
            font-style:italic;
        }
    </style>
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <div>
        <button id="pressme">Press Me</button>
    </div>
    <script>
        document.getElementById("pressme").onclick=function(){
            document.styleSheets[0].disabled=!document.styleSheets[0].disabled;
            //disabled:获取或设置样式表的禁用状态，返回布尔值
            //此处即交替禁止或启用styleSheets[0](即第一个样式表)的所有样式
        }
    </script>
</body>
</html>
```
- 自己版script:
```
<script>
       document.getElementById("pressme").onclick=disableornot;
       
       var itstyle=document.styleSheets[0];
       
       function disableornot(e) {
          itstyle.disabled=!itstyle.disabled;
        }
    </script>
```


## 29.1.4 CSSRuleList对象的成员
- CSSStyleSheet.cssRules属性：返回一个CSSRuleList对象，代表样式表里的各种样式——即CSSRuleList对象是一个总体。

<center>CSSRuleList对象的成员</center>

成员|说明|返回
----|----|----
item(<pos>)|返回指定索引的CSS样式|CSSStyleRule
length|返回样式表里的样式数量|数值

- CSSStyleSheet.cssRules.item(i):返回一个CSSStyleRule对象，代表其中的一种CSS样式——即CSSStyleRule对象是一个个体
<center>CSSStyleRule对象的成员</center>

成员|说明|返回
----|----|----
cssText|获取或设置样式的文本（包括选择器）|字符串
parentStyleSheet|获取此样式所属的样式表|CSSStyleSheet
selectorText|获取或设置样式的选择器文本|字符串
style（详见29.3节）|获取或设置一个代表具体样式属性的对象|CSSStyleDeclaration

- CSSStyleSheet对象和CSSRuleList对象和CSSStyleRule对象区别

对象|说明|获取方式
----|----|----
CSSStyleSheet|代表样式表，是样式表集合|document.styleSheets
CSSRuleList|代表样式表中的样式，是样式的集合|document.styleSheets[i].cssRules
CSSStyelRule|代表样式表中的样式集合中的一条样式|document.styleSheets[i].cssRules.item(i)

### eg:使用CSSRuleList对象和CSSStyleRule对象 ***Good!移除一个元素子元素的方法***
（书29-4）
```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style title="core styles">
        p{
            border:medium double black;
            background-color:lightgray;
        }
        #block1{
            color:white;
            border:thick solid black;
            background-color:gray;
        }
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
    <!--
    <link rel="stylesheet" type="text/css" href="styles.css"/>
    <style media="screen AND (min-width:500px),PRINT" type="text/css">/*PRINT: 样式用于打印预览和打印页面时*/
        #block2{
            color:yellow;
            font-style:italic;
        }
    </style>-->
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <p id="block2">
        One of the most interesting aspects of fruit is the variety available in each country.
    </p>
    <div>
        <button id="pressme">Press Me</button>
    </div>
    <div id="placeholder"></div>
    
    <script>
        var placeholder=document.getElementById("placeholder");
        processStyleSheet();//获取已定义的样式的信息
        
        document.getElementById("pressme").onclick=function(){
            document.styleSheets[0].cssRules.item(1).selectorText="#block2";
            //选择第2个样式，将其选择器文本变换为"#block2"
            
            if (placeholder.hasChildNodes()) {//hasChildNodes()方法：如果当前元素节点拥有子节点，则返回 true
                var childCount=placeholder.childNodes.length;//childNodes.length：子节点个数
                for (var i=0;i<childCount;i++) {
                    placeholder.removeChild(placeholder.firstChild);//removeChild方法：删除指定节点
                }
            }
            processStyleSheet();//重新获取修改后的样式的信息,可以看到第二个样式信息表变为了#block2
            
        }
        
        function processStyleSheet() {
            var rulesList=document.styleSheets[0].cssRules;
            //CSSStyleSheet.cssRules属性：返回一个CSSRuleList对象，它可访问样式表里的各种样式
            
            for (var i=0;i<rulesList.length;i++) {//CSSRuleList对象的成员 length：返回样式表里的样式数量
                var rule=rulesList.item(i);//CSSRuleList对象的成员item(<pos>)：返回指定索引的CSS样式
                var newElem=document.createElement("table");
                newElem.setAttribute("border","1");
                addRow(newElem,"parentStyleSheet",rule.parentStyleSheet.title);
                //CSSStyleRule对象的成员 parentStyleSheet:获取此样式所属的样式表
                addRow(newElem,"selectorText",rule.selectorText);
                //CSSStyleRule对象的成员 selectorText:获取或设置样式的选择器文本
                addRow(newElem,"cssText",rule.cssText);
                ////CSSStyleRule对象的成员 cssText:获取或设置样式的文本（包括选择器）
                placeholder.appendChild(newElem);
            }
        }
        
        function addRow(elem,header,value) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td></tr>";
        }
    </script>
</body>
</html>
```
- 自己重写script部分：
```
<script>
       var results=document.getElementById("placeholder");
       showinformation();
       
       var mybutton=document.getElementById("pressme");
       
       mybutton.onclick=whenclickmybutton;
       
       function whenclickmybutton() {
           document.styleSheets[0].cssRules.item(1).selectorText="#block2";
           
           if (results.hasChildNodes()) {
                var childCount=results.childNodes.length;
                for(var i=0;i<childCount;i++){//不能直接用i<results.childNodes.length,因为这样的话每循环一次就会重新计算一次，会出错
                   results.removeChild(results.lastChild);
                }
            }
            showinformation();
            
        }
       
       
       function showinformation() {
            var mycssrulelist=document.styleSheets[0].cssRules;
       
            for (var i=0;i<mycssrulelist.length;i++){
                var myshowtable=document.createElement("table");
                myshowtable.setAttribute("border","2");
                
                var mycssrule=mycssrulelist.item(i);
                addrow(myshowtable,"cssText",mycssrule.cssText);
                addrow(myshowtable,"parentStyleSheet",mycssrule.parentStyleSheet.title);
                addrow(myshowtable,"selectorText",mycssrule.selectorText);
                
                results.appendChild(myshowtable);//不可用results.innerHTML=myshowtable
            }
           
        
       }
       
       function addrow(table,col1,col2) {
          table.innerHTML+="<tr><th>"+col1+"</th><td>"+col2+"</td></tr>";
        }
    </script>
```
- 一点感悟：
当在一个元素里面写入东西的时候，如果写入的是一个元素对象则不可以用results.innerHTML=newelement,因为这样仅仅会输出一个“object HTML xx (如Table)Element”字样；而一个用results.appendChild(newelement)

## 29.2使用元素样式
- 要获取某元素style属性所定义的样式属性，需读取HTMLELement对象里定义的style属性值。
- HTMLElement.style属性：返回一个CSSStyleDeclaration对象
- HTMLElement.style.cssText属性：显示和修改应用到某元素上的style属性
### eg:从HTMLElement上获取CSSStyleDeclaration对象，并修改其属性
（书29-5）
```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
</head>
<body>
    <p id="block1" style="color: white;border: thick solid black;background: gray">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    
    <div>
        <button id="pressme">Press Me</button>
    </div>
    <div id="placeholder"></div>
    
    <script>
        var placeholder=document.getElementById("placeholder");
        var targetElem=document.getElementById("block1");
        displayStyle();
        
        document.getElementById("pressme").onclick=function(){
            targetElem.style.cssText="color:black";//.style.cssText属性：显示和修改应用到某元素上的style属性
            displayStyle();
        }
        
        function displayStyle() {
            if (placeholder.hasChildNodes()) {
                placeholder.removeChild(placeholder.firstChild);
            }
            var newElem=document.createElement("table");
            addRow(newElem,"Element CSS",targetElem.style.cssText);
            placeholder.appendChild(newElem);
        }
        
        function addRow(elem,header,value) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td><tr>";
        }
    </script>
</body>
</html>
```
- 说明：解析cssText属性这种方法并不好，29.3里的方法更为推崇。

## 29.3使用CSSStyleDeclaration对象
- 不管处理样式表or某个元素的style属性，要通过DOM完全控制CSS,必须使用**CSSStyleDeclaration对象**
- CSSStyleDeclaration对象成员：item(<pos>).索引和数组风格表示法都支持，即document.styleSheets[0].cssRules.item(4)和item[4]都可以 **Chrome并不支持数组方法**,可以使用**document.styleSheets[0].cssRules[4]**
## 29.3.1使用便捷属性
### eg:使用CSSStyleDeclaration对象的便捷属性 ***Good!***
（29-6）
- 便捷属性：border/color/padding对应于各自同名CSS属性。
- 便捷属性paddingTop对应与CSS的padding-top属性，多单词CSS属性命名规则：去掉连字符，大写第二个及之后单词的首字母

```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style title="core styles">
        #block1{
            color:white;
            border:thick solid black;
            background-color:gray;
        }
        p{
            border:medium double black;
            background-color:lightgray;
        }
       
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    <p id="block2" style="border: medium dashed blue;color: red;padding: 2px">
        One of the most interesting aspects of fruit is the variety available in each country.
    </p>
    
    <div>
        <button id="pressme">Press Me</button>
    </div>
    <div id="placeholder"></div>
    
    <script>
        var placeholder=document.getElementById("placeholder");

        displayStyle();
        
        document.getElementById("pressme").onclick=function(){
            document.styleSheets[0].cssRules.item(1).style.paddingTop="10px";
            document.styleSheets[0].cssRules.item(1).style.paddingRight="12px";
            document.styleSheets[0].cssRules.item(1).style.paddingLeft="5px";
            document.styleSheets[0].cssRules.item(1).style.paddingBottom="5px";
            displayStyle();
        }
        
        function displayStyle() {
            if (placeholder.hasChildNodes()) {
                var childCount=placeholder.childNodes.length;
                for(var i=0;i<childCount;i++){
                    placeholder.removeChild(placeholder.firstChild);
                }
            }
            displayStyleProperties(document.styleSheets[0].cssRules.item(1).style);//读取自从样式表获得的对象
            displayStyleProperties(document.getElementById("block2").style);//读取自元素的style属性
        }
        
        function displayStyleProperties(style) {
            var newElem=document.createElement("table");
            newElem.setAttribute("border","1");
            addRow(newElem,"border",style.border);
            addRow(newElem,"color",style.color);
            addRow(newElem,"padding",style.padding);
            addRow(newElem,"paddingTop",style.paddingTop);
            
            placeholder.appendChild(newElem);
            
        }
        
        function addRow(elem,header,value) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td><tr>";
        }
    </script>
</body>
</html>
```
## 29.3.2使用常规属性
- CSSStyleDeclaration对象的成员getPropertyValue(<name>):获取某CSS属性的值
- CSSStyleDeclaration对象的成员setProperty(<name>,<value>,<priority>):定义新值
### eg1使用CSSStyleDeclaration对象的成员getPropertyValue和setProperty **VeryGood!获取和设置css样式的最常用方法**
（书29-7）

```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style title="core styles">
        p{
            color: white;
            border:medium double black;
            background-color:gray;
            padding-top: 5px;
        }
       
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    
    <div>
        <button id="pressme">Press Me</button>
    </div>
    <div id="placeholder"></div>
    
    <script>
        var placeholder=document.getElementById("placeholder");

        displayStyle();
        
        document.getElementById("pressme").onclick=function(){
            var styleDeclr=document.styleSheets[0].cssRules[0].style;
            styleDeclr.setProperty("background-color","lightgray");
            //CSSStyleDeclaration对象的成员setProperty(<name>,<value>,<priority>):定义新值
            styleDeclr.setProperty("padding-top","20px");
            styleDeclr.setProperty("color","black");
            displayStyle();
        }
        
        function displayStyle() {
            if (placeholder.hasChildNodes()) {
                var childCount=placeholder.childNodes.length;
                for(var i=0;i<childCount;i++){
                    placeholder.removeChild(placeholder.firstChild);
                }
            }
            
            var newElem=document.createElement("table");
            newElem.setAttribute("border","1");
            
            var style=document.styleSheets[0].cssRules[0].style;
            
            addRow(newElem,"border",style.getPropertyValue("border"));
            //CSSStyleDeclaration对象的成员getPropertyValue(<name>):获取某CSS属性的值
            addRow(newElem,"color",style.getPropertyValue("color"));
            addRow(newElem,"padding-top",style.getPropertyValue("padding-top"));
            addRow(newElem,"background-color",style.getPropertyValue("background-color"));
            
            placeholder.appendChild(newElem);
            
        }
        
        function addRow(elem,header,value) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td><tr>";
        }
    </script>
</body>
</html>

```

### eg2 以程序方式探索CSS属性 ***Very Very Good!***
(书29-8）
- document.styleSheets[0].cssRules[0].style可以枚举第一个样式表第一条样式所有属性
```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style title="core styles">
        p{
            color: white;
            border:medium double black;
            background-color:gray;
            padding-top: 5px;
        }
       
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    

    <div id="placeholder"></div>
    
    <script>
        var placeholder=document.getElementById("placeholder");

        displayStyles();
        
        function displayStyles() {
            var newElem=document.createElement("table");
            newElem.setAttribute("border","1");
            
            var style=document.styleSheets[0].cssRules[0].style;
            //枚举样式表第一条样式所有属性
            for(var i=0;i<style.length;i++){
                addRow(newElem,style[i],style.getPropertyValue(style[i]));
            }
            
            placeholder.appendChild(newElem);
        }
        
        function addRow(elem,header,value) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td><tr>";
        }
    </script>
</body>
</html>
```
### eg3:style.getPropertyPriority(<name>)获知某个属性的重要性
(书29-9）
- getPropertyPriority方法：对高优先级的值返回important，没有指定重要性则返回空白字符串
- setProperty(<name>,<value>,<priority>)方法：设置属性的值和优先级，可将important作为第三个参数指定给setProperty方法
```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style title="core styles">
        p{
            color: white;
            border:medium double black;
            background-color:gray;
            padding-top: 5px !important;/*!important:表示浏览器优先使用这个值显示元素*/
        }
       
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    

    <div id="placeholder"></div>
    
    <script>
        var placeholder=document.getElementById("placeholder");

        displayStyles();
        
        function displayStyles() {
            var newElem=document.createElement("table");
            newElem.setAttribute("border","1");
            
            var style=document.styleSheets[0].cssRules[0].style;
            //枚举样式表第一条样式所有属性
            for(var i=0;i<style.length;i++){
                addRow(newElem,style[i],style.getPropertyPriority(style[i]));
            //getPropertyPriority方法：对高优先级的值返回important，没有指定重要性则返回空白字符串
            }
            
            placeholder.appendChild(newElem);
        }
        
        function addRow(elem,header,value) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td><tr>";
        }
    </script>
</body>
</html>
```

## 29.3.3使用细粒度的CSS DOM对象
eg1:使用CSSPrimitiveValue对象
（书29-10）有错误，说“无style.getPropertyCSSValue()函数”
```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style title="core styles">
        p{
            color: white;
            background-color:gray;
            padding: 7px !important;/*!important:表示浏览器优先使用这个值显示元素*/
        }
       
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    

    <div id="placeholder"></div>
    
    <script>
        var placeholder=document.getElementById("placeholder");

        displayStyles();
        
        function displayStyles() {
            var newElem=document.createElement("table");
            newElem.setAttribute("border","1");
            
            var style=document.styleSheets[0].cssRules[0].style;
           
            for(var i=0;i<style.length;i++){
                var val=style.getPropertyCSSValue(style[i]);
                //CSSStyleDeclaration.getPropertyCSSValue方法：可获取CSSPrimitiveValue对象成员
                //CSSPrimitiveValue对象最关键的成员为primitiveType：获取属性值的单位表达
                
                if (val.primitiveType==CSSPrimitiveValue.CSS_PX) {
                    addRow(newElem,style[i],val.getFloatValue(CSSPrimitiveValue.CSS_PX),"pixels");
                    //CSSPrimitiveValue对象的成员getFloatValue(<type):获得一个数值
                    addRow(newElem,style[i],val.getFloatValue(CSSPrimitiveValue.CSS_PT),"points");
                    addRow(newElem,style[i],val.getFloatValue(CSSPrimitiveValue.CSS_IN),"inches");
                }
                else if (val.primitiveType==CSSPrimitiveValue.CSS_RGBCOLOR) {
                    var color=val.getRGBColorValue();
                    //CSSPrimitiveValue对象的成员getRGBColorValue():获得一个颜色值
                    addRow(newElem,style[i],color.red.cssText+" "+color.green.cssText+" "+color.blue.cssText,"(color)");
                }
                else{
                    addRow(newElem,style[i],val.cssText,"(other)");
                }
            }
            
            placeholder.appendChild(newElem);
        }
        
        function addRow(elem,header,value,units) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td><td>"+units+"</td><tr>";
        }
    </script>
</body>
</html>
```

## 29.4使用计算样式
- 计算样式：浏览器用于显示某个元素的CSS属性值集合
- document.defaultView.getComputedStyle方法：获取包含某一元素计算样式的CSSStyleDeclaration对象
### eg 使用某个元素的计算样式
```
<!doctype html>

<html lang="en">
<head>
    <title>Chapter29</title>
    <meta name="author" content="Adam Freeman"/>
    <meta name="description" content="A simple example"/>
    <link rel="shortcut icon" href="wetryer.ico" type="image/x-icon"/>
    <style title="core styles">
        p{
            padding: 7px !important;/*!important:表示浏览器优先使用这个值显示元素*/
        }
       
        table{
            border:thin solid black;
            border-collapse:collapse;
            margin:5px;
            float:left;
        }
        td{
            padding:2px;
        }
    </style>
</head>
<body>
    <p id="block1">
        There are lots of different kinds of fruit- there are over 500 varieties of banana alone.
    </p>
    

    <div id="placeholder"></div>
    
    <script>
        var placeholder=document.getElementById("placeholder");

        displayStyles();
        
        function displayStyles() {
            var newElem=document.createElement("table");
            newElem.setAttribute("border","1");
            
            var targetElem=document.getElementById("block1");
            var style=document.defaultView.getComputedStyle(targetElem);
            addRow(newElem,"Property Count",style.length);
            addRow(newElem,"margin-top",style.getPropertyValue("margin-top"));
            addRow(newElem,"font-size",style.getPropertyValue("font-size"));
            addRow(newElem,"font-family",style.getPropertyValue("font-family"));
            
            placeholder.appendChild(newElem);
        }
        
        function addRow(elem,header,value) {
            elem.innerHTML+="<tr><td>"+header+":</td><td>"+value+"</td><tr>";
        }
    </script>
</body>
</html>
```