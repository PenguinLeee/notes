# HyperTextMarkupLanguage

**HTML basic document**

```html
<html>
<head>
<title>Document name goes here</title>
</head>
<body>
visible text here
</body>
</html>
```

------------------------------

**Text elements**

````html
<p>This is a paragraph</p>
<br> (line break)
<hr> (horizontal rule)
<pre>This text is preformatted</pre>
````

（段落、换行、水平线、预格式etc）

--------------------

**Logical Styles**

```html
<em>This text is emphasized</em>
<strong>This text is strong</strong>
<code>This is some computer code</code>
```

（强调、计算机代码）

------

**Links, Anchors, Image elements**

```html
<a href="http://www.example.com/">This is a Link</a>
<a href="http://www.example.com/"><img src="URL" alt="Alternate Text"></a>
<a href="mailto:webmaster@example.com">Send e-mail</a>

<!--A named anchor:-->
<a name="tips">Useful Tips Section</a>
<a href="#tips">Jump to the Useful Tips Section</a>
```

---

**Unordered list**

```html
<ul>
<li>First item</li>
<li>Next item</li>
</ul>
```

效果：

<ul>
<li>First item</li>
<li>Next item</li>
</ul>

md是真的方便...

---

**Ordered list**

```html
<ol>
<li>First item</li>
<li>Next item</li>
</ol>
```

效果如下：

<ol>
<li>First item</li>
<li>Next item</li>
</ol>

---

**Definition list**

```html
<dl>
<dt>First term</dt>
<dd>Definition</dd>
<dt>Next term</dt>
<dd>Definition</dd>
</dl>
```

效果如下：

<dl>
<dt>First term</dt>
<dd>Definition</dd>
<dt>Next term</dt>
<dd>Definition</dd>
</dl>
---

**Tables**

```html
<table border="1">
<tr>
  <th>someheader0</th>
  <th>someheader1</th>
</tr>
<tr>
  <td>sometext</td>
  <td>sometext</td>
</tr>
</table>
<!--td为tabledata, tr标签之内描述一行的情形-->
```

效果如下：

<table border="1">
<tr>
  <th>someheader0</th>
  <th>someheader1</th>
</tr>
<tr>
  <td>sometext</td>
  <td>sometext</td>
</tr>
</table>

---

**Frames**

```html
<frameset cols="25%,75%">
  <frame src="page1.htm">
  <frame src="page2.htm">
</frameset>
```

效果如下...个j8

<frameset cols="25%,75%">
  <frame src="page1.htm">
  <frame src="page2.htm">
</frameset>

---

**Forms**

```html
<form action="http://www.example.com/test.asp" method="post/get">
<input type="text" name="lastname"
value="Nixon" size="30" maxlength="50">
<input type="password">
<input type="checkbox" checked="checked">
<input type="radio" checked="checked">
<input type="submit">
<input type="reset">
<input type="hidden">
<select>
<option>Apples
<option selected>Bananas
<option>Cherries
</select>
<textarea name="Comment" rows="3"
cols="10"></textarea>
</form>
```

<form action="http://www.example.com/test.asp" method="post/get">
<input type="text" name="lastname"
value="Nixon" size="30" maxlength="50">
<input type="password">
<input type="checkbox" checked="checked">
<input type="radio" checked="checked">
<input type="submit">
<input type="reset">
<input type="hidden">
<select>
<option>Apples
<option selected>Bananas
<option>Cherries
</select>
<textarea name="Comment" rows="3"
cols="10"></textarea>
</form>

-----

## 什么是 XHTML？

- XHTML 指的是可扩展超文本标记语言
- XHTML 与 HTML 4.01 几乎是相同的
- XHTML 是更严格更纯净的 HTML 版本
- XHTML 是以 XML 应用的方式定义的 HTML
- XHTML 是 [2001 年 1 月](https://www.w3school.com.cn/w3c/w3c_xhtml.asp)发布的 W3C 推荐标准
- XHTML 得到所有主流浏览器的支持

------

#### HTML表单

搜集不同类的用户输入

**input元素**

最重要的表单元素。控制用户输入，输入的类型由type来决定。

````html
<form>
    <input type="radio" name="sex" value="male" checked>male
    <br />
    <input type="radio" name="sex" value="female">female
</form>
````

<form>
    <input type="radio" name="sex" value="male" checked>male
    <br />
    <input type="radio" name="sex" value="female">female
</form>
**提交按钮**

用于向表单处理程序提交表单

```html
<form action='action_page.php'>
    First name:<br />
<input type="text" name="firstname" value='mickey'>
    <!--上文的value='mickey'是下面显示的值-->
<input type="submit" value="Submit">
</form>
```

<form action='action_page.php'>
    First name:<br />
<input type="text" name="firstname" value='mickey'>
<input type="submit" value="Submit">
</form>

----------------

**Action属性**

定义提交表单时的动作。通常，表单会被提交到web server上的网页。

若省略action, 则action会被设置为当前页面。

---------

**Method属性**

规定在提交表单时的http方法

GET, POST etc

表单提交是被动的，没有敏感信息时用GET（eg 提交搜索关键词）

when GET？GET适用于少量数据提交

when POST？更新数据（大量数据提交），或者有敏感信息（例如密码）

POST安全性更好，因为地址栏中被提交的数据不可见

-------------

**Name属性**

要想正确提交，则每个`<input>`中需要有name属性。

-----------

**用`<fieldset>`组合表单数据**

````````html
<form>
<fieldset>
    <legend>Personal information</legend>
    First name:<br />
    <input type="text" name="fn" >
    <input type="submit" value="sbmt">
    </fieldset></form>
````````

<form>
<fieldset>
    <legend>Personal information</legend>
    First name:<br />
    <input type="text" name="fn" >
    <input type="submit" value="sbmt">
</fieldset>
</form>

#### HTML表单元素

**`<input>`元素**

最重要的表单元素，根据不同的type属性可以变化成多种形态

----------

**`<select>`元素**

定义下拉列表：

```html
<select name="cars">
    <option value="benz">benz</option>
    <option value="volkswagen">vw</option>
    <option value="audi" selected>audi</option>
</select>
```

<select name="cars">
    <option value="benz">benz</option>
    <option value="volkswagen">vw</option>
    <option value="audi" selected>audi</option>
</select>

------------

