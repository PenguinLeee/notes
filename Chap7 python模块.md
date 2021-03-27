## Chap7 Py模块

**程序的架构：**将一个程序分割为源代码文件的集合以及将这些部分链接在一起的方法

python: 用模块module或者包package来组织

module：一个py程序；

package：一个目录，包含多个子目录、py或者其他文件的

module：.py文件，其名即为文件名，可以用import导入

**内置module**：用别的语言写成，py解释器可运行

可以的文件类型：

````
*.py：普通py脚本
*.pyo：pyc的优化版本
*.pyc：编译的二进制代码。初次import时，python会建立一个.pyc文件包含.py的二进制码使得更快
#  上述三者的import并不会使程序执行速度变快或者变慢，但是后两者的加载较快
*.pyd：做成了Windows dll
*.pyw：在windowed模式下跑的，没有控制台
*.so
*.dll
````

module的导入：在一个文件中载入另一个文件，并且可以读取那个文件的内容。

当导入module时，其内部所有代码会被运行（初始化？）



**package：**包含`__init__.py`的文件。包名即目录名。

import package时，`__init__.py`的代码会被全部运行。

python3.3+下，目录中即使没有`__init__.py`也会被视为package

一般地，包是一个目录，可以使用import导入包，或者from import来导入包的模块。



**标准库模块：**python中自带的实用模块，也称为标准链接库（PSL）

这个集合体大约有200多个模块, 包含与平台不相 关的常见程序设计任务: 操作系统接口, 对象永 久保存, 文字匹配模式, 网络和Internet 脚本, GUI 建构等. 

PSL分为两类：内置模块/非内置模块

**模块的执行环境: **模块包含变量, 函数, 类 以及其他的模块 (如 果导入的话), 而函数 也有自己的本地变量. 



import语句：

`import module1[,module2[......`

**工作方式：** 解释器执行到 import 语句, 如果在搜索路径中找到了指定的模块, 就会加载它.

* 如果模块是被第一次导入, 它将被加载并执行.
* 该过程遵循 LEGB 作用域原则：如果在一个模块的顶层导入, 那么它的作用域就是 全局的；如果在函数中导入, 那么它的作用域是局部的. 



python中三种模块：
1. * 用C写成，和解释器链接；例如math库
  * 用C写成的动态链接库；可以通过`sys.builtin_module_names`查看
2. .py文件。可以用`*.__file__`查看其位置
3. package



import模块时，python做

```
python解释器检查 module registry 部分，即(sys.modules) 
if 模块事先导入：
	使用当前存在的模块创建一个对象
else:
	创建一个新的空module对象（字典元素）并放入sys.modules（字典）
	加载此模块中的对象
	在新的模块namespace中执行此代码对象
```



**导入模块module0时：**

* 创建namespace module0；
* 创建一个名为module0的对象，它引用了namespace module0，则可以通过这个对象访问module0中的变量和函数
* 在namespace module0中执行代码文件



**import语法：**

1. import module1， ...

2. import module as alias
3. from module import *
   * 只能用于一个模块的最顶层，函数中不可使用此方法
   * 不是良好的编程风格，此时namespace用的是default，可能把原有的同名变量覆盖
4. from module import attr



**import语句是怎么找到包的？**

`import modname`

1. 解释器先看内置包里有没有叫modname的：`sys.builtin_module_names`
2. 没有，则看sys.path里目录下有没有叫modname.py的，在系统设置里是PYTHONPATH（？）



包的覆盖：我写了一个math.py，但是不会被解释器加载，因为内置math库优先级高

```python
>>> sys.path
['', 'D:\\Python\\Python39\\python39.zip', 'D:\\Python\\Python39\\DLLs', 'D:\\Python\\Python39\\lib', 'D:\\Python\\Python39', 'D:\\Python\\Python39\\lib\\site-packages']
# sys.path由程序当前目录、PYTHONPATH目录（需要自己写）、标准链接库目录、*.pth文件组成
# PYTHONPATH和*.pth可以自己写，用来拓展路径
# Python在遍历已知的库文件目录过程中，如果见到一个.pth 文件，就会将文件中所记录的路径加入到 sys.path 设置中，于是 .pth 文件说指明的库也就可以被 Python 运行环境找到了。
# 这意味着在当前目录放一个.pth也挺好（
```

sys.path[0]=‘’，指当前目录

在查找模块foo时（foo在builtin中不存在时），解释器会依据表按顺序做下列查找：

1. foo是package
2. foo.so, foomodule.so, foomodule.sl, 或 foomodule.dll (已编译扩展) 
3. foo.pyo (只在使用 -O 或 -OO 选项时) 
4. foo.pyc 
5. foo.py

上述步骤只有在模块第一次执行时才会执行. 在这之后, 导入相同模块时, 会跳过这些步骤, 而只提取内存中已加载的模块对象。

这是特意设计的结果，因为导入-编译-执行开销大

若需要在同一个session中再一次运行module则需要reload()（返回一个模块对象）



sys.path的构成

1. 程序的主目录（当前目录）
2. PYTHONPATH环境变量目录（自己设置）
3. 标准链接库目录（系统自带）
4. 以上目录中所有的.pth文件

可以临时加入目录：sys.path.append() or sys.path.insert()



**`if __name__ == "__main__":`的运行原理**

一个python文件通常有两种使用方法，第一是作为脚本直接执行，第二是 import 到其他的 python  脚本中被调用（模块重用）执行。因此 `if __name__ == '__main__':` 的作用就是控制这两种情况执行代码的过程，在 `if  __name__ == '__main__'`: 下的代码只有在第一种情况下（即文件作为脚本直接执行）才会被执行，而 import  到其他脚本中是不会被执行的。举例说明如下：

每个python module都包含内置的变量`__name__`，是Python的内置变量，用于指代当前模块，也可以指示当前的包结构。

如果一个模块被直接运行，则其没有包结构，其 `__name__` 值为 `__main__`。

被import的模块`__name__=文件名`，作为脚本运行的模块`__name__ == '__main__'`

`if __name__ == '__main__'` 就相当于是 Python **模拟的程序入口**。Python 本身并没有规定这么写，这只是一种编码习惯。由于模块之间相互引用，不同模块可能都有这样的定义，而入口程序只能有一个。到底哪个入口程序被选中，这取决于 `__name__` 的值。



**`__main__.py`与python -m**

Python 的 `-m` 参数用于将一个模块或者包作为一个脚本运行，而 `__main__.py` 文件则相当于是一个包的”入口程序“。



**包（package）**



**Python函数**

函数不设返回值则返回None



**参数类型：**

本质上仅两种：位置参数和关键字参数

功能的区分：位置参数、默认参数、可变长参数、命名关键字参数、关键字参数



位置参数：f(1,2,3,4,5)

默认参数：f(1,2,z=3,a=4,b=5) 默认参数必须放在最后，必须指向不变对象

可变长参数：为了解决事先不确定需要传入参数的个数问题，引入了可变长参数；当传递的实参数目比函数的形参更多时，一般会报错；函数中的可变长参数可以用来吸收多余的参数。

```python
def sum(x, *args): 
	s = x
	for i in args:
		s += i
	return s
```

当你希望一个函数可以接受处理比当初声明定义时更多的参数, 可以使用不定长参数, 即实现了函数的参数冗余. 

不定长参数的另外一种实现方法: 使用iterable变量 

如果输入是一个iterable变量L, 其实可以用 *L 的方式传入，也可以不用

原理：

函数定义时, `*`可以将按位置将传递进来的参数 打包成元组,

函数调用时, `*`以解包待传递到函数中的元组、列表、集合、字符串等类型，并按位置传递到函数中 



关键词传入：f(1,2,*z=3*)

若前面的参数用关键词传入，则后面的所有都要用关键词传入



__可变长关键字参数：**kwargs__

函数定义时与调用时参数的传入：

**可以将按关键字传递的参数打包成dict类型



```python
def sum(x, y):
	print(x + y)
d = dict(x=1, y=2)
print(d)
sum(**d)
```









**命名关键字参数**

对于可变长的关键字参数，函数的调用者可以传入任意不受限制的关键字参数；

如果要限制关键字参数的名字，就可以用命名关键字参数：例如只接收city和salary作为关键字参数。

```
def regform(gender, *, city, salary = 10000):
	print(gender, city, salary)

def regform2(gender, *args, city, salary=10000):
	print(gender, args, city, salary)

# 写了*之后只能加入gender,city,salary
```

**命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错。**

定义命名关键字参数在没有可变参数的情况下 不要忘了写分隔符*, 否则定义的将是位置参数. 



```python
# 命名关键字参数
# 对于关键字参数，函数的调用者可以传入任意不受限制的关键字参数
# 至于到底传入了哪些，就需要在函数内部通过kw检查

# 仍以person()函数为例，我们希望检查是否有city和job参数


def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数
        pass
    if 'job' in kw:
        # 有job参数
        pass
    print('name:', name, 'age:', age, 'other:', kw)


# 但是调用者仍可以传入不受限制的关键字参数
person('Jack', 24, city='Beijing', addr='朝阳', zipcode=123456)

# 如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收city和job作为关键字参数


def person(name, age, *, city, job):
    print(name, age, city, job)


# 和关键字参数**kw不同，命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数
# 调用方式如下
person('Jack', 24, city='Beijing', job='Engineer')

# 命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错
# TypeError: person() takes 2 positional arguments but 4 were given
# 由于调用时缺少参数名city和job，Python解释器把这4个参数均视为位置参数，但person()函数仅接受2个位置参数
# person('Jack', 24, 'Beijing', 'Enginner')

# 如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了


def person(name, age, *args, city, job):
    print(name, age, args, city, job)

person("yel", 23, 'a', 'b', 'c', city='chongqing', job='Eng')

# 命名关键字参数可以有缺省值，从而简化调用


def person(name, age, *, city='Beijing', job):
    print(name, age, city, job)
# 由于命名关键字参数city具有默认值，调用时，可不传入city参数
person('Jack', 24, job='Python')

# 使用命名关键字参数时，要特别注意，如果没有可变参数，就必须加一个*作为特殊分隔符
# 如果缺少*，Python解释器将无法识别位置参数和命名关键字参数


def person(name, age, city, job):
    # 缺少*，city 和job被视为位置参数
    pass
```





**排序：sorting**

`sort()`in-place sorting

`sorted()`out-of-place sorting



Practice 2: 五种类型的参数集于一个函数 完成如下函数的定义, 对所有输入的整数参数求 和. args本身和kwargs的值(value)都为整数. 

```
def f(a, b=2, *args, c=3, **kwargs):
	sum = a + b + c
	for i in args:
		sum += i
	m = kwarg.keys()
	for i in m:
		sum += kwargs[i]
	return m	
```



**变量作用域**

函数内的局部变量作用域仅限于def内。

在关键词def定义函数的范围内，新定义 or 赋值的变量都是局部变量，在该函数之外引用函数内命名的变量的时候会报错。

函数外的变量，如果不声明的话也看不到，在局部区域引用

```python
a = 1
def addtwo():
	sum = 5
	a = a - sum
	return sum
print(addtwo())

# 这里会报错，因为a需要预先指定, 上来就a = a - num的话，a会被当成局部变量；而使用sum = sum/a之类的式子可以确保a是全局变量
```

**局部变量和全局变量**

可以在局部上用global来进行标记

**同名变量引用**

当某局部变量和全局变量有相同的变量名时，函数内引用该变量会直接调用函数内定义的局部变量。

问题: 如果有嵌套函数, 并且有多个同名变量该 怎么办? 

LEGB原理：

一个函数体内需要引用变量时，查找：

1. 局部变量
2. 找外层的局部变量（嵌套的过程）
3. 找全局变量
4. 找内置库



**Python闭包**

将数据绑定到函数的技术称为闭包。

一般情况下，在我们认知当中，如果一个函数结束，函数的内部所有东西都会释放掉，还给内存，局部变量都会消失。但是闭包是一种特殊情况，如果外函数在结束的时候发现有自己的临时变量将来会在内部函数中用到，就把这个临时变量绑定给了内部函数，然后自己再结束。

* 数据不是作为参数传递
* 当函数调用完成的时候，它们（这些数据）还在



还有一点需要注意：使用闭包的过程中，一旦外函数被调用一次返回了内函数的引用，虽然每次调用内函数，是开启一个函数执行过后消亡，但是闭包变量实际上只有一份，每次开启内函数都在使用同一份闭包变量。

```python
#coding:utf8
def outer(x):
    def inner(y):
        nonlocal x
        x+=y
        return x
    return inner


a = outer(10)
print(a(1)) //11
print(a(3)) //14
```



````python
def outer_func():
	msg = 'out'
	def inner_func():
		nonlocal msg
		msg = 'in'
		print(msg)
	inner_func()
	print(msg)
outer_func()

# in
# in
````

当使用nonlocal时，msg变量的作用域跳出了inner_func到了outer_func

什么时候使用Python closures?

* Closures可以避免使用全局值，并提供某种形式的数据隐藏



**Python正则表达式**

KMP算法

正则表达式引擎 + 正则表达式文本 -> 正则表达式对象 ->匹配结果

​																	 文本



re.Match 对象

正则表达式语法：







