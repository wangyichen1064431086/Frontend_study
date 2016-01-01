# Chapter17/18

### eg:使用CSS计数器特性
（书17-18）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style type="text/css">
        body{
            counter-reset: paracount;
        }
        p:before{
            content: counter(paracount) ". ";
            counter-increment:paracount 1;
        }
    </style>
</head>

<body>
    <p>Fourscore and seven years ago our father brought forth on
    this continent a new nation,conceived in liberty ,and dedicated to
    the proposition that all men are created equal.</p>
    
    <a id="apressanchor" class="class1 class2" href="http://apress.com">Visit the Press website</a>
    <p>I like <span lang="en-gb" class="class2">apples</span> and oranges.</p>
    <a id="w3canchor" href="http://w3c.org">Visit the W3C website.</a>
    <a href="http://google.com">Visit Google</a>
   

</body>
</html>
```

### eg:使用:check选择器
（书18-9）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style type="text/css">
       :checked+span{
            background-color: red;
            color: white;
            border: medium solid black;
            padding: 4px;
        }
    </style>
</head>

<body>
    <form method="post" action="http://www.wetryer.com:8080/form">
        <p>
            <label for="apples">
                Do you like apples:
            </label>
            <input type="checkbox" id="apples" name="apples"/>
            <span>This will go red when checked</span>
        </p>
        <input type="submit" value="Submit"/>
    </form>
   

</body>
</html>
```
### eg: :valid和:invalid选择器
(书18-11）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style type="text/css">
       p :invalid{
            outline: medium solid red;
       }
       p :valid{
            outline:medium solid green;
       }
    </style>
</head>

<body>
    <form method="post" action="http://www.wetryer.com:8080/form">
        <p>
           <label for="name">Name:<input required id="name" name="name"/></label>
        </p>
        <p>
            <label for="city">City:<input required id="city" name="city"/></label>
        </p>
        <button type="submit">Submit Vote</button>
    </form>
   

</body>
</html>
```

### eg:使用:in-range和:out-of-range选择器***Good***
（书18-14）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style type="text/css">
       :in-range{
            outline: medium solid green;
       }
      :out-of-range{
            outline:medium solid red;
       }
    </style>
</head>

<body>
    <form method="post" action="http://www.wetryer.com:8080/form">
        <label for="price">
            $ per unit in your area:
            <input type="number" min="0" max="100" value="1" id="price" name="price"/>
        </label>
        <button type="submit">Submit Vote</button>
    </form>
   

</body>
</html>
```

### eg:选择必须和可选的input元素
(书18-13）
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style type="text/css">
       label>:required{
            outline: medium solid green;
       }
       label>:optional{
            outline:medium solid red;
       }
    </style>
</head>

<body>
    <form method="post" action="http://www.wetryer.com:8080/form">
        <label for="price1">
            $ per unit in your area:
            <input type="number" min="0" max="100" required value="1" id="price1" name="price1"/>
        </label>
        <label for="price2">
            $ per unit in your area:
            <input type="number" min="0" max="100" value="1" id="price2" name="price2"/>
        </label>
        <button type="submit">Submit Vote</button>
    </form>
   

</body>
</html>
```

### eg:使用:link和:visited选择器
```
<!DOCTYPE html>

<html>
<head>
    <title>Example</title>
    <style type="text/css">
       :link{
            border: thin black solid;
            background-color: lightgrey;
            padding: 4px;
            color: red;
       }
       :visited{
            background-color: grey;
            color: white;
       }
    </style>
</head>

<body>
    <a id="apressanchor" class="class1 class2" href="http://apress.com">Visit the Press website</a>
    <p>I like <span lang="en-gb" class="class2">apples</span> and oranges.</p>
    <a id="w3canchor" href="http://w3c.org">Visit the W3C website.</a>
   

</body>
</html>
```