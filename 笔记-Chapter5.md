# Chapter5 初探JavaScript

## 5.4使用变量和类型
## 5.4.2创建对象
### 方法1 使用new Object()方法
```
  <script>
        var myData=new Object();
        myData.name="Adam";
        myData.weather="sunny";
        
        document.writeln("Hello "+myData.name+".");
        document.writeln("Today is "+myData.weather+".");
    </script>
```
### 方法二 使用对象字面量
- 属性的名称和值之间用（：）分隔，各个属性之间用（,）分隔
- 对象中，可以添加属性，也可以添加函数（属于一个对象的函数叫做方法）
- this关键字：代表其所属对象
```
   <script>
        var myData={
            name:"Adam",
            weather:"sunny",
            printMessages:function(){//对象中可以添加函数
                document.writeln("Hello "+this.name+".");
                document.writeln("Today is "+this.weather+".");
            }
            
            
        };
        
        document.writeln("Hello "+myData.name+".");
        document.write("Today is "+myData.weather+".");
        myData.printMessages();
    </script>
```

## 5.4.3使用对象
### 1.读取和修改属性值
- 两种形式：(1)maData.name="Joe" (2)myData["name"]="Joe"
```
 <script>
        var myData={
            name:"Adam",
            weather:"sunny"
        };
        
        myData.name="Joe";
        myData["weather"]="raining";
        
        var propName="weather"
        myData[propName]="snowing";
        
        document.writeln("Hello "+myData.name+".");
        document.write("Today is "+myData.weather+".");
    </script>
```

### 2.枚举对象属性
```
 <script>
        var myData={
            name:"Adam",
            weather:"sunny",
            printMessage: function(){
                document.write("Hello"+this.name+", today is "+this.weather+".");
            }
        };
        
        for(var prop in myData){//每次迭代过程要处理的属性名会赋给prop变量
            document.write('<pre>');//必须加上显示计算机源码的pre元素，writeln的显示才能换行
            document.writeln("Name:"+prop+" Value:"+myData[prop]+"\n");
            document.write('</pre>');
        }
      
    </script>
```

### 3.增删属性和方法
```
    <script>
        var myData={
            name:"Adam",
            weather:"sunny",
            printMessage: function(){
                document.write("Hello"+this.name+", today is "+this.weather+".");
            }
        };
        
        myData.dayOfWeek="Monday";//为对象添加新属性、新方法
        myData.sayHello=function(){
            document.writeln("Hello");
        }
       
       delete myData.name;//删除对象的属性
       delete myData["sayHello"];
       
       for(var prop in myData){
            document.write("<pre>");
            document.writeln("name:"+prop+" value:"+myData[prop]);
       }
    </script>
```

### 4. 判断某个对象是否具有某属性
```
   <script>
        var myData={
            name:"Adam",
            weather:"sunny",
            printMessage: function(){
                document.write("Hello"+this.name+", today is "+this.weather+".");
            }
        };
        
       
       var hasName="name" in myData;//判断对象myData中是否有name属性
       var hasDate="date" in myData;
       document.write("hasName:"+hasName+" hasDate:"+hasDate);
    </script>
```
## 5.5使用JavaScript运算符
### 5.5.1相等和等同
- 相等运算符==会把变量转换为同一类型再判断是否相等，即只判断值，不管类型；等同运算符===则判断值和类型是否都相同
- JavaScript对象的比较是引用的比较，这里==和===貌似没什么区别了；JavaScript基本类型变量的比较是值的比较，==和===就有上述区别了
```
  <script>
       var myData1={
            name:"Adam",
            weather:"sunny"
        }
        
        var myData2={
            name:"Adam",
            weather:"sunny"
        }
        
        var myData3=myData2;
       
        var t1=myData1==myData2;//JavaScript对象的比较是引用的比较，这里==和===貌似没什么区别了
        var t2=myData2==myData3;
        var t3=myData1===myData2;
        var t4=myData2===myData3;
        
        document.writeln(t1+" "+t2+" "+t3+" "+t4);
        
        
        var value1=5;
        var value2="5";
        var value3=value2;
        
        var v1=value1==value2;//JavaScript基本类型变量的比较是值的比较，==和===就有区别了:相等运算符==会把变量转换为同一类型再判断是否相等，即只判断值，不管类型；等同运算符===则判断值和类型是否都相同
        var v2=value2==value3;
        var v3=value1===value2;
        var v4=value2===value3;
        
        document.writeln(v1+" "+v2+" "+v3+" "+v4);
    </script>
```

## 5.5.2 显式类型转换
```
<script>
        var d1=5+5;
        var d2=5+"5";//连接符+比加法运算符+优先级更高
        var d3=(5).toString()+String(5);//两种方法都是将number类型的值转换为string类型
        var d4=Number("5")+Number("5");
        document.write(d1+d2+d3+d4)
    </script>
```
## 5.6使用数组
### 5.6.1创建数组
- 两种方法创建数组：(1)使用newArry()方法；（2）使用数组字面量
```
<script>
       var myArray=new Array();//使用newArry()方法
       myArray[0]=100;
       myArray[1]="Adam";
       myArray[2]=true;
       
       var myNewArray=[100,"Adam",true];//使用数组字面量
    </script>
```
### 5.6.2读取、修改、枚举数组内容
```
 <script>
       var myArray=new Array();
       myArray[0]=100;
       myArray[1]="Adam";
       myArray[2]=true;
       
       var myNewArray=[100,"Adam",true];
       
       document.writeln(myNewArray[0]);//读取指定位置数组索引
       
       myNewArray[0]="abc";//修改数组内容
       document.writeln(myArray[0]);
       
       for(var i=0;i<myNewArray.length;i++){//枚举数组内容
            document.writeln("Index"+i+":"+myNewArray[i]);
       }
    </script>
```

### 5.6.3使用Array对象的内置数组方法
```
 <script>
       document.write("<pre>");
       var myArray=[100,"Adam",true];
       var otherArray=[3,2,1];
       
       var new1=myArray.concat(otherArray);//合并数组（可为多个数组）
       document.writeln(new1);
       
       var new2=myArray.join("\t");//将所有数组元素连接为一个字符串，各元素内容用参数指定的字符分隔。
       document.writeln(new2);
       
       var new3=myArray.pop();//把数组当作栈使用，删除并返回数组的最后一个元素
       document.writeln(new3+" "+myArray);
       
       var new4=myArray.push("love");//把数组当作栈使用，将指定数据添加到数组中，返回void
       document.writeln(new4+" "+myArray);
       
       var new5=myArray.reverse();//就地反转数组元素次序,主要是“就地”，即原数组也反转了
       document.writeln(new5+" "+myArray);
       
       var new6=myArray.shift();//类似pop()，但操作的是数组第一个元素
       document.writeln(new6+" "+myArray);
       
       var new7=myArray.slice(0,1);//返回一个子数组,slice(start,end)返回第start到end-1的索引的子数组
       document.writeln(new7+" "+myArray);
       
       var new8=otherArray.sort();//就地对数组元素排序
       document.writeln(new8+" "+otherArray);
       
       var new9=myArray.unshift("wang","yi","chen");//类似push()，但是新元素被插到数组开头,返回void
       document.writeln(new9+" "+myArray);
       
       document.write('</pre>');
    </script>
```

### 5.7处理错误
```
 <script>
       try{//可能会引发错误的代码被包装在try子句中，没发生错误就忽略catch子句；发生错误，控制权就转移到catch子句
            var myArray;
            for(var i=0;i<myArray.length;i++){
                document.writeln("Index "+i+": "+myArray[i]);
            }
        }
        catch(e){//发生的错误由error对象描述
            document.writeln("<pre>Error:"+e+"\n"+e.message+"\n"+e.name+"\n"+e.number+"</pre>");
        }
        finally{//无论发生错误都执行finally子句
            document.writeln("<pre>Always be executed.</pre>");
        }
    </script>
```
