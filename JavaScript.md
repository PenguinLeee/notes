# JavaScript

**intro**

js可以改变html内容

```javascript
document.getElementById("demo").innerHTML = "hello world";
```

<p id="demo">JavaScript 能够改变 HTML 内容。</p>

<button type="button" onclick='document.getElementById("demo").innerHTML = "Hello JavaScript!"'>点击我！</button>

JS可以改变HTML属性。

<button onclick="document.getElementById('myImage').src='/i/eg_bulbon.gif'">开灯</button>

<img id="myImage" border="0" src="/i/eg_bulboff.gif" style="text-align:center;" alt="bubble">

<button onclick="document.getElementById('myImage').src='/i/eg_bulboff.gif'">关灯</button>

JS可以隐藏HTML元素。

通过改变display样式来隐藏

````javascript
<button action='document.getElementById("demo").style.display="none";'>
````

<button action='document.getElementById("demo").style.display="none";'>dis</button>

-------------

**JS使用**

* `<script type="text/javascript">`标签内的被自动执行
* 函数和事件，例如当发生事件时调用函数（按钮点击）
* `<body>`、`<head>`中可以放代码段，定义函数，然后button被点击时可以调用。
* 外部脚本：`<script src="script.js">`





**JS输出**

JS不提供任何的内建的打印或者显示函数。

显示方案：

* `window.alert()`写入警告框
* `document.write()`写到HTML输出
* `innerHTML`更改HTML元素
* `console.log()`写到控制台

以下是示例：

InnerHTML

```javascript
<p id="demo">hhhhhhh</p>
<script>document.getElementById("demo").innerhtml = 5+6;
```



document.write()

*注意：在HTML完全加载之后使用它的话，将会删除所有已有的HTML*

例如：

```javascript
<button type="button" onclick="document.write("11111111")">
```



console.log("hhhhhhhhhhhhhhhhhhhhhhhh")

-----------

**正式的JS编程：JS语句**

JS是由web浏览器执行的指令。

```javascript
var x, y, z;
x = 22;
y = 11;
x = x + y;
```

----------

JS使用Unicode编码

-------

重复声明JavaScript变量，则其值不会被丢弃

```javascript
var a = 1;
a = 2;
var a;
a
```

JS的变量类型是动态的：

```javascript
var a = 1;
a = '123'
```



## JavaScript 数据类型

JavaScript 变量能够保存多种**数据类型**：数值、字符串值、数组、对象等等：

```
var length = 7;                             // 数字
var lastName = "Gates";                      // 字符串
var cars = ["Porsche", "Volvo", "BMW"];         // 数组
var x = {firstName:"Bill", lastName:"Gates"};    // 对象 
```

**JS数值**

JS仅有一个数值类型

写数值时有无小数点均可

科学计数法

布尔值：true, false

**JS数组**

`var cars = ['a', 'b', 'c']`

`cars[0]='d'`

**JS对象**

```javascript
var person = {firstName:"Bill", lastName:"Gates", age:62, eyeColor:"blue"};
typeof person
```

**Undefined**

JS中无值变量值为undefined：

```javascript
var person;
typeof person; // undefined
```

**Null**

在 JavaScript 中，null 是 "nothing"。它被看做不存在的事物。

不幸的是，在 JavaScript 中，null 的数据类型是对象。

您可以把 null 在 JavaScript 中是对象理解为一个 bug。它本应是 null。

您可以通过设置值为 null 清空对象：

```javascript
var person = null; //值null，但是类型仍然是对象
typeof null // object
```

----------

**Undefined 与 Null 的区别**

Undefined 与 null 的值相等，但类型不相等：

```javascript
typeof undefined              // undefined
typeof null                   // object
null === undefined            // false - 类型
null == undefined             // true -值
```

-----------

**原始数据**

原始数据值是一种没有额外属性和方法的单一简单数据值。

typeof 运算符可返回以下原始类型之一：

- string
- number
- boolean
- undefined

**实例**

```
typeof "Bill"              // 返回 "string"
typeof 3.14                // 返回 "number"
typeof true                // 返回 "boolean"
typeof false               // 返回 "boolean"
typeof x                   // 返回 "undefined" (假如 x 没有值)
```