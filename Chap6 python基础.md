## Chap6 Py基础

**布尔型**：&, |, not
**空值**：（none type）python里的一个特殊的值，可用来填充表格中的缺失值
isinstance()：获取数据类型
列表添加元素：append, insert——在某个位置添加

删除元素：remove某个值；pop按照索引弹出

判断列表长度：len()

列表切片：```list[start:end:step]```，其中end不包括

考察子串存在性：`"__" in "__main__"`

list的特殊方法：`__mul__, __ __`

```python
>ls = [1,2,3]
>print(ls.__mul__(2))
[1,2,3,1,2,3]
```

**特殊方法（dunder method）**：一个类可以通过定义具有特殊名称的方法来实现由特殊语法所引发的特定操作 （例如算术运算或下标与切片）。这是 Python 实现**操作符重载**的方式，允许每个类自行定义基于操作符的特定行为。例如：

`x.__getitem__(y) <==> x[y]`

**字典(HashMap)**

字典创建：

zip：将两个列表组建称为键和值；产生新的元组列表[(,), (,), ...]

之后dict()

for循环：将值放入键中

得到值：get(key, default=?)，索引（不存在则报错），in判断是否在里面，dict.keys()/dict.values()删减值：del, pop, clear

集合：创建使用set()或者{}不可改变
for循环range类型表示数字的不可变(immutable)序列，通常用于在for循环中循环特定次数
break：过早的premature（中断）
计算运行时间：import timet0 = time.time()t1 = time.time() - t0
字典推导式
enumerate类
ascii和序号的转换：chr(65), ord('a')
Unicode是字符集，utf8是编码规则广义的 Unicode 是一个标准，定义了一个字符集以及一系列的编码规则，即 Unicode 字符集和 UTF-8、UTF-16、UTF-32 等等编码Unicode 字符集为每一个字符分配一个码位，例如「知」的码位是 30693，记作 U+77E5（30693 的十六进制为 0x77E5）。unicode的编码对于英文字母有大量冗余，因此改良后使用utf-8则可以将多余字母用2-4B来表示
str.join(iterable)：将iterable对象的每个元素聚合在一个字符串里

iterable对象：有`__iter__()`或者`__getitem__()`方法的东西
str.strip([chars])去掉str首尾的char得到的新对象

str.replace(old, new[, count])

str.split(separator[, count])

str.zfill(width)——左填充0，遇上正负号则在正负号之后填充

print：print(*objects, sep=' ', end='\n', file=sys.stdout)sep多个元素时的分隔符，file：要写入的文件对象

**格式化字符串**

f-strings

```
print(f"{2*34}")
name = "Qiwen Li"
print(f"name={name}")

```





Python文件open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
此处的newline用来区分换行符：不同的操作系统换行方式不一样：   linux:\n  windows:\r\n  mac:\r本机测试：In[15]: os.linesepOut[15]: '\r\n'写文件时newline=''则进入全局newlines模式，输入的各种换行方式都将被显示成标准换行符newline=None时换行方式不变
读文件时newline='\r'时，将\r视为换行符；'\n'时将\n视为换行符；''时将三者都视为换行符但是原样输出；None时将三者都视为换行符，但是自动转换成现在的标准换行符
写文件时，''和\n时原样写入，None时将\n转化成os.linesep；其他情况下将\n转换成给定的东西
readline(size=-1)read(size=-1)#if $size is specified, at most $size chars will be read. 
readlines读取得到一个字符串列表
读取时换行符会保留，可以使用strip()去掉
写入：write()：write本身不写入换行符。

csv文件的操作
DictReader()和DictWriter()：读取csv成为字典/把字典写入csv



**Bytes and Binary data processing**

由bytes创造的对象不可以更改

字符串转换而来的，使用默认构造方法“bytes”可以构造其它编码的文字，但是b"blabla"种只能包含合法的ascii字符

```python
bytes.fromhex(String)
bytes(String, encode='foo')
b'种' #illegal
b'bar'.hex() #to hex string
```

```python
>>> bytes(range(97,255))
b'abcdefghijklmnopqrstuvwxyz{|}~\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe'
```

**bytearray**

可以修改的二进制串类(mutable counterpart of bytes objects)

```python
bytearray.fromhex(String) 
#其中String需要是若干十六进制数表示的字节串（即偶数个hex字符）
>>> ba1 = bytearray.fromhex("eda4b6b9\n\n\n\n")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: non-hexadecimal number found in fromhex() arg at position 8
>>> ba1 = bytearray.fromhex("eda4b6b")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: non-hexadecimal number found in fromhex() arg at position 7
    
>>> ba1.decode("utf8")
'中'
>>> ba2 = b'abc'
>>> ba2[1]
98
>>> ba2[1] = 0x42
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'bytes' object does not support item assignment

>>> ba3 = bytearray(b'abc')
>>> ba3[1]
98
>>> ba3[1] = 0x42
>>> ba2[3] = 66
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: bytearray index out of range
>>> ba2[2] = 66
>>> ba2
bytearray(b'abB')

# index out of range似乎存在于各种可以动态变化的数据结构中，例如：
>>> l=[]
>>> l[0]=1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list assignment index out of range
```



**Struct module**

看介绍，是实现C的struct和python的bytes对象互相转换的东西

使用format strings 来做C的struct的描述

可以使用struct来判断littleEndian或者bigEndian：struct.pack()

```python
import struct
endianness = 'little'
if struct.pack("@i", 1) == bytes.fromhex('0001'):
	endianness = 'big'
print(endianness)
```

或者使用sys

```python
print(sys.byteorder)
```

struct.pack(String format, v1, v2, ...) -> bytes：将数据使用指定的格式进行编码和拼贴

<a href="https://blog.csdn.net/weiwangchao_/article/details/80395941">对于struct.pack()和struct.unpack()的解释</a>





















