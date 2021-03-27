## Chap8 python面向对象

类定义的说明：

```python
class Student():
	def __init__(self, name):
		self.name = name
		self.score = score
	def print_score(self):
		print("%s %s" %self.name, self.score)
		
stu_a = Student("吴明", 80)
stu_a.print_score()
```

初始方法`__init__()`

**Python的封装**

Python语言没有严格意义的封装性！！！！！

全指望程序员自己注意看

如果我们希望某些内部属性不被外部访问, 我们可以在属性名称前加上两个下划线__, 表示将该属性成员私有化, 该成员在内部可以被访问, 但 是在外部是不能够访问的.

成员私有化并不是代表完全不能够从外部访问成员, 而是提高了访问的门槛, 防止意外或者随意改变成员, 引发错误. 我们仍然可以通过

```
_类名+私有变量
```

 对变量进行访问.

双下划线开头的实例变量是不是一定不能从外 部访问呢? 其实也不是. 不能直接访问`__score`是因为Python解释器对外把`__score`变量改成了 `_Student__score`，所以仍然可以通过 `_Student__score` 来访问`__score`变量

数据封装的作用：可以通过方法来对属性来进行检测和修改。

python中以双下划线开头并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是 private变量, 所以不能用`__score__`这样的变量名

方法的私有化：也可以通过在方法名前加上"__"来完成。



**类属性绑定**

Python 是动态语言，可以在运行时增加属性，删除属性。但从面向对象的角度来看, 还是在定义类的时候将属性确定下来更佳。

````python
class Dog:
	kind = 'canine'
Dog.age = 7
print(Dog.kind, Dog.age)
del Dog.age
print(Dog.kind)
# print(Dog.age)
````

**实例方法、类方法、静态方法**

实例方法不需要加东西来说明；

类方法需要加上@classmethod

静态方法需要加上@staticmethod



**类的继承**

在编程语言中，如果一个新的类继承另外一个类，那么这个新的类称为子类 (subclass)，被子类继承的类称为父类 (base class or super class)

每一个学生的实例，自然拥有学生的属性学号、分数，以及其父类人类的属性姓名、年龄、性别等；

类可以根据继承关系形成一棵树状的结构，每个节点代表一个类；

子类的示例，包含了根节点以来的所有属性和方法



注意：子类也定义了同名方法，则会覆盖父类的方法



**Python语言的异常**

assert语句

`assert Expression, e`

Expression若为真，则可以正确执行，否则抛出后面的e错误提示



自主控制异常：raise

```python
class MyError(Exception):
	pass
raise MyError("My Error Test")
```

更详细的例子：

```python
class NotIntError(Exception):
	def __init__(self, value):
		self.value = value
	def __str__(self):
		return repr(self.value)
    # repr(object)返回一个对象的string形式

a = [1, '', 2, 'ab', [3,4]]
for i in range(len(a)):
	try:
		if type(a[i]) != int:
			raise NotIntError("非整型")
	except NotIntError as e:
		print(e.value, end = '')
	finally:
		print(a[i])
```



**Python迭代器**

迭代是使用Iterator对象迭代Iterable的所有元素的过程。 在python中，我们可以使用for循环或while循环进行迭代

Iterable对象：可以用for循环走完一遍的东西

Iterator：是Iterable里面的元素

iterable使用了`__iter__()`之后就成了迭代器，迭代器总是可迭代的

**什么是迭代器**
     迭代器是访问可迭代对象的工具
     迭代器是指用iter(obj)函数返回的对象(实例)
     迭代器是指用next(it)函数获取可迭代对象的数据

Iterable：

1. 有一个`__iter__()`方法返回一个Iterator，或者
2. 有一个`__getitem__()`方法使得序列索引从0开始且在超限之后抛出IndexError

Iterable也不一定是有限的

有`__iter__()`的并不一定是iterable！万一：

```python
def __iter__(self):
	return self
```

则返回的不是iterator！原类不是iterable



把一个类作为一个迭代器使用的话，需要在类中实现`__iter__()`和`__next__()`

`__iter__(self, start=0)` 方法返回一个特殊的迭代器对象， 这个迭代器对象实现了 `__next__()` 方法并通过 StopIteration 异常标识迭代的完成。

`__next__()`方法返回下一个迭代器对象。

```python
class MyNumbers:
	def __iter__(self):
		self.a = 1
		return self
	
	def __next__(self):
		x = self.a
		self.a += 1
		return x

myclass = MyNumbers()
myiter = iter(myclass)

print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))

```

StopIteration用于标识迭代的完成，防止无限循环。

`__next__()`中可以设置完成指定次数之后触发StopIteration来结束迭代

iter()函数：

如果对象x有`__iter__()`，则`iter(x)`返回一个由`__iter__()`生成的iterator

如果x没有`__iter__()`则试着用x[0], x[1], ....来索引

如果还不行就抛出TypeError异常



**Iterator**

迭代器是一个有着多个元素且支持遍历的东西

迭代器可以：

1. 记得迭代到哪里了
2. 有`__next__()`方法，可以：
   * 返回迭代器的下一个元素
   * 记住下一个元素在哪
   * 到尽头抛出stopIteration
3. 可以自我迭代（`__iter__`返回自己），把自己做成迭代器

以上称为迭代器协议（Iterator protocol）

有上述协议的可以通过内置`iter()`函数获得迭代器



**内置next()函数**

```python
next(iterator[, default])
```

调用了`__next__()`

若default给定，则在iter耗尽时返回之，否则异常



for循环：

```python
L = [1, 2, 3]
for x in L:
	print(x)
	
L = [1, 2, 3]
itr = iter(L)
while 1:
	try:
		print(next(itr))
	except StopIteration:
		break
```

**Python map() 函数**
map() 会根据提供的函数对指定序列做映射。

第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。
```python
>>> def square(x) :         # 计算平方数
...     return x ** 2
...
>>> map(square, [1,2,3,4,5])    # 计算列表各个元素的平方
<map object at 0x100d3d550>     # 返回迭代器
>>> list(map(square, [1,2,3,4,5]))   # 使用 list() 转换为列表
[1, 4, 9, 16, 25]
>>> list(map(lambda x: x ** 2, [1, 2, 3, 4, 5]))   # 使用 lambda 匿名函数
[1, 4, 9, 16, 25]
```



**用生成函数方法来创建iterator对象**

创建迭代器的简单方法是用生成器

方法有二：

* 生成器函数
* 生成器表达式

生成器函数有yield语句

```python
# generatorDemo.py
from collections.abc import Iterator
L = list(range(1, 11))
def cubes(numbers):
	for x in numbers:
		yield x**3

g = cubes(L)
print(type(g))
print(isinstance(g, Iterator))
print(next(g))
print(g.__next__())
for x in g:
	print(x, end = ' ')
print(next(g)) 
```



函数中若有yield陈述语句，则成为生成器函数。

一旦被调用则返回一个generator对象

在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

```python
# yieldTest.py
def output3():
	print("输出3")
	yield 3
o3 = output3()
print(o3)
print(type(o3))
n = next(o3)
print(n)

>>> <generator object output3 at 0x000001E8E58123C0>
>>> <class 'generator'>
>>> 输出3
>>> 3


#!/usr/bin/python3
 
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()

```



generate function的相关性质：

* 包含>=1个yield
* 被调用则返回一个迭代器，但是并不会立即执行
* `__iter__()`和`__next__()`方法自动生成，因此可以用`next()`遍历
* yield时函数暂停，控制交还给caller
* 局部变量在call之间被存储
* 生成函数结束时自动起StopIteration，不论再call多少次都是一样

```python
def output():
	num = 0
	while True: 
		print("输出"+str(num))
		yield num
		num += 1
		if num == 4:
			break
	return

o = output()
try:
	print(next(o))
except StopIteration:
	print("stopped")
try:
	print(next(o))
except StopIteration:
	print("stopped")
try:
	print(next(o))
except StopIteration:
	print("stopped")
try:
	print(next(o))
except StopIteration:
	print("stopped")
try:
	print(next(o))
except StopIteration:
	print("stopped")
try:
	print(next(o))
except StopIteration:
	print("stopped")
try:
	print(next(o))
except StopIteration:
	print("stopped")
    
输出0
0
输出1
1
输出2
2
输出3
3
stopped
stopped
stopped  
```

使用generator表达式来创建迭代器



```python
def gfunc(n):
    print("begin")
    for i in range(n):
        print(i, end='')
        yield i*i
        print("+"*(2*i+2))
    print("end")

for x in gfunc(3):
    print(f"^2 = {x}")
    print("-"*15)
```

yield和return的区别

yield：

* 暂停执行过程
* 发回一个值yield，但是不清除这个函数的东西。
* 随后继续执行之

return：

* 中止执行函数
* 发回值或者None
* 摧毁所有的局部变量



**Python装饰器**

python中万物皆对象：内置类型、类、函数etc

```python
>>> a = 1
>>> dir(a)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'as_integer_ratio', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
```



```python
a = a + 3
# 可以认为是调用了a.__add__(3)
abs(a)
# 可以认为是调用了a.__abs__()
a.conjugate()
# 求a的共轭复数
a.real, a.lmag
```



```python
def add2(x):
	'''返回 x+2'''
	return x+2
	
print(type(add2))
print(isinstance(add2, object))
print(add2.__doc__())
```

函数是对象，函数也可以有属性和方法

函数def的时候的名字只是一个引用（类似于遥控器）：

```python
def add3(x):
	'''返回 x+3'''
	return x+3
g = add3
del add3
print(g(4))
```

我们可以给函数设置新的属性或者更新旧有属性的值

```python
def foo():
	pass
setattr(foo, 'name', 'Unknown')
setattr(foo, 'age', 22)
print(getattr(foo, 'age'))
print(foo.name, foo.age)
delattr(foo, 'age')
print(vars(foo))
```

**first-class object**

一等对象 `first-class object`（第一类对象）有如下“特权”：

- 可以被赋值给一个变量
- 可以嵌入到数据结构中
- 可以作为参数传递给函数
- 可以作为值被函数返回

一个一等公民(即类型、对象、实体、或值等等)在一门特定的编程语言中能够支持其他大多数实体所支持的操作。

**int、字符串、字典、函数是第一类对象**

嵌套定义函数：

```python
def team():
	print("Our team:")
	welcome = '欢迎'
	def member1():
		print(f'{welcome} 第1个')
	def member2():
		print(f'{welcome} 第2个')
	member2()
	member1()
team()
# 直球member1()会报错
```

只有team()调用了才会定义member1,2，team结束后member1,2直接被销毁

member1,2以局部变量的形式存在



函数可以被赋值给变量，可以嵌到数据结构中，可以作为参数传递，也可以被函数返回



**函数装饰器**

装饰器是一个可调用的对象，可以用来装饰函数或者类

装饰器本质上是一个python函数，可以让其他函数在不变动代码的前提之下增加额外功能。

返回值也是一个函数对象。

它经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景。装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码并继续重用。



两种装饰器：函数装饰器和类装饰器

直球实现：

```python
def decorator(func):
	def wrapper():
		print("start")
		func()
		print("end")
	return wrapper

def sayHello():
	print("hello v1")

say_hello = decorator(say_hello)
print(say_hello)
say_hello()
```

装饰器包装一个函数、修饰其行为然后返回之。

python糖：

```python
def decorator(func):
	def wrapper():
		func()
	return wrapper

@decorator
def say_hello():
	'''say_hello v2'''
	print("hello v2")
	
print("say_hello")
print(say_hello.__doc__)
print(say_hello.__name__)
say_hello()
```



**Introspection内省**

让python的对象干事之前知道自己有什么方法、属性

```python
f = callable
print(f)
print(f.__doc__, f.__name__)
```

注意：callable的函数需要有`__call__()`方法

```python
import functools
def decorator(func):
	@functools.wraps(func)	# @functools.wraps使用functools.update_wrapper()来更新特殊属性的值，例如__name__, __doc__。
	def wrapper():
		func()
	return wrapper
	
@decorator
def say_hello():
    '''say_hello v3'''
    print("say_hello v3")

print(say_hello, say_hello.__doc__, say_hello.__name__)
say_hello()
```

在修饰时使用参数：

```python
import functools
def greet(func):
	@functools.wraps(func)
    def wrapper_greet(*args, **kwargs):
        func(*args, **kwargs) # 收到什么参数就传什么参数
    return wrapper_greet

@greet
def say_hello(name):
    ''' say_hello v4'''
    print("say_hello {}".format(name))

say_hello("li")
```

装饰器装饰的函数如果还需要返回值的时候：

```python
import functools
def greet(func):
	@functools.wraps(func)
    def wrapper_greet(*args, **kwargs):
        ret = func(*args, **kwargs)
        return ret
    return wrapper_greet

@greet
def say_hello(name):
    print(f'hello {name}')
    return f'hello {name}'
ret = say_hello('Ming')
print(ret)
```

一般地，由修饰函数决定被修饰函数的返回值，但是我们默认是返回wrapper的返回值。



修饰函数的模板：

```python
import functools
def decname(func):
    @functools.wraps(func)
    def wrapper_decname(*args, **kwargs):
        # do sth before
        ret = func(*args, **kwargs)
        # do sth after
        return ret
    ret wrapper_decname
    
#调用时：
@decname
func()
```



**Chaining/nesting decorators**

反复装饰，但是理解了装饰的原理就不难

**有状态decorator**

装饰器可以对状态进行跟踪（可以自行定义）

（待续）



**类装饰器**

可以装饰类中的方法

若是想装饰整个类？

一个失败的案例：

```python
import functools
def star(func):
	@functools.wraps(func)
	def wrapper_star(*args, **kwargs):
		print('*'*30)
		ret = func(*args, **kwargs)
		print('*'*30)
		return ret
	return wrapper_star
	
@star
class Human(object):
	def __init__(self, name, age):
		self.name = name
		self.age = age
		print('__init__ called')
	def print_info(self):
	print(self.age, self.name)
	
h = Human("Ming Wu", 30)
h.print_info()

>>>******************************
>>>__init__ called
>>>******************************
>>>30 Ming Wu
```

装饰一个类并不意味着它装饰所有方法

仅仅装饰了`__init__`



装饰整个类：

```python
from dataclasses import dataclass
@dataclass
class Human:
	name: str
	age: int
	def print_info(self):
		print(self.name, self.age)
h = Human('Ming wu', 30)
h.print_info()
print(repr.(h))
```

@dataclass自动为Human类写了`__init__`, `__repr__`等特殊方法



**callable**

callable是任何可以调用的东西。

函数、类、方法

带有`__call__()`方法的类的成员，等等

都是callable的

都可以用括号来调用运行之。



内置的callable类和函数



```python
class Mult:
	def __init__(self, a, b):
		self.a = a
		self.b = b
	def __call__(self):
		return self.a + self.b
		
m = mult(3, 4)
print(m())
print(callable(m))
# 此处m是Mult的一个实例，m通过m()被调用
```



**把一个类作为装饰器**

```python
@decorator 
#等价于
y = decorator(y)
```

因此，如果decorator是一个类，则需要将func作为参数传到`__init__`里

类中的实例需要是callable的，使得这个实例可以代表修饰后的函数



将类作为装饰器时需要保证以下几点：

1. 类的实例是可调用的
2. 类需要一个地方讲被装饰的函数传入到类的实例里

第一条可以通过`__call__`实现，第二条可以通过`__init__`实现。



```python
import functools
class Profiled:
	def __init__(self, func):
		@functools.wraps(func)
		self.func = func
	
	def __call__(self, *args, **kwargs):
		print("call")
		return self.func(*args, **kwargs)
	
def add(x, y):
	return x + y
	
print(add)
add = Profiled(add)
print(add)
result = add(1, 2)
print(result)
```

实现类装饰器，可以blablabla



**类的内置装饰器**

@classmethod, @staticmethod, @property

@staticmethod

将类中的方法设置为静态方法，就是在不需要创建实例对象的情况下，可以通过类名来进行直接引用，来达到将函数功能与实例解绑的效果。

@classmethod

类方法的第一个参数是一个类，是将类的本身作为操作的方法。类方法是被哪个对象调用的，就传入哪个类作为第一个参数进行操作。

```python
class Car:
    car = 'audi'

    @classmethod
    def value(cls, category):
        print('%s is the %s' % (category, cls.car))

class Bmw(Car):
    car = 'Bmw'


class Benz(Car):
    car = 'Benz'


print('通过实例进行调用')
b = Bmw()
b.value('normal')
print('直接用类名进行调用')
Benz.value('NOnormal')
```



@property 使调用类中方法像引用类中的字段属性一样。被修饰的特性方法，内部可以实现处理逻辑，但是对外部提供统一的调用方式，遵循了统一的访问的原则。

```python
class Demo:
	@property
	def index(self):
		print("Hello world")
		
	def index0(self):
		print("hw")

a = Demo()
a.index
a.index0()
```



**修饰器的作用**

计时

```python
import functools
import time
def timer(func):
	@functools.wraps(func)
	def wrapper_timer(*args, **kwargs):
		start_time = time.perf_counter()
		ret = func(*args, **kwargs)
		run_time = time.perf_counter() - start_time
		print(f'Finished {func.__name__} in {run_time}s')
		return ret
	return wrapper_timer

@timer
def waste_time(num_times):
	for i in range(num_times):
		sum([x**2 for x in range(100000)])

waste_time(10)
```



debug

```

```



Python 单例模式(singleton)

只有一个实例的类

例如：python中的`None, False, True`

1. 单例模式是一种软件设计模式，而不是专属于某种编程语言的语法；
2. 单例模式只有一个实例存在；
3. 单例模式有助于协调系统的整体性、统一性；



logging

````python
from functools import wraps
def logit(func):
    @wraps(func)
    def wrapper_logit(*args, **kwargs):
        if func.__name__ == 'debug':
            msg = f"debug {args[0]}"
        elif func.__name__ == 'info':
            msg = f"info {args[0]}"
        else:
            msg = f"unknown {args[0]}"
        return func(msg, **kwargs)
    return wrapper_logit

@logit
def debug(msg):
    print(msg)
    
debug('123')
````



