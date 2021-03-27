## Chap10 python数据可视化

#### python的高级数据结构

numpy的ndarray

numpy是Python科学计算基础包, 提供了以下功能 (仅列举了几条):

* 快速高效的多维数组对象 ndarray
* 用于对数组执行元素级计算以及直接对数组执行数学运算的函数
* 用于读写硬盘上基于数组的数据集的工具
* 线性代数运算、傅里叶变换以及随机数生成
* 用于集成由C、C++、Fortran等语言编写的代码的工具
* 对于数值型数据, numpy 数组在存储和处理数据时要比内置的Python数据结构高效得多 



numpy的特性：同质化的多维数组，是多维的同类型元素组成的表，可以像matlab一样被索引

在numpy中维度称为axis（轴）

numpy的array class被称为ndarray。简称array。多维的！



**numpy的一维数组**

numpy的arange()函数

```python
>>> import numpy as np
>>> a = np.arange(5)
>>> b = np.arange(-2, 1, .5)
>>> a.dtype
dtype('int32')
>>> b.shape
(6,)
>>> a
array([0, 1, 2, 3, 4])
>>> b
array([-2. , -1.5, -1. , -0.5,  0. ,  0.5])
```



numpy的array()函数

```python
>>> b = np.array([.5*x for x in range(-4, 4)])
>>> b
array([-2. , -1.5, -1. , -0.5,  0. ,  0.5,  1. ,  1.5])
>>> c = np.random.randn(3)
>>> np.around(c, decimals=4)
array([-0.6713, -0.1772,  0.3613])
>>> c.dtype
dtype('float64')
>>> c
array([-0.67127979, -0.17715986,  0.36134807])
```



numpy的reshape函数

```python
>>> a = np.arange(5)
>>> print('a=', a)
a= [0 1 2 3 4]
>>> b = np.arange(6).reshape(2, 3)
>>> print('b=', b)
b= [[0 1 2]
 [3 4 5]]
```



一维数组支持运算：

加减乘除、点积、叉积

```python
>>> a= np.array([1, 2, 3])
>>> b = np.arange(4, 7)
>>> a
array([1, 2, 3])
>>> b
array([4, 5, 6])
>>> a+b
array([5, 7, 9])
>>> a-b
array([-3, -3, -3])
>>> a*b
array([ 4, 10, 18])
>>> a/b
array([0.25, 0.4 , 0.5 ])
>>> np.cross(a, b)
array([-3,  6, -3])
>>> np.dot(a, b)
32
```



多维数组（二维矩阵）

```python
>>> a = np.array([[1,2,3,4],[5,6,7,8]])
>>> a
array([[1, 2, 3, 4],
       [5, 6, 7, 8]])
>>> type(a)
<class 'numpy.ndarray'>
```

一个有趣的现象：

```python
>>> a = np.array([[1,2,3,4],[5,6,7,'8']])
>>> a
array([['1', '2', '3', '4'],
       ['5', '6', '7', '8']], dtype='<U11')
>>> a = np.array([[1,2,3,4],[5,6,7,None]])
>>> a
array([[1, 2, 3, 4],
       [5, 6, 7, None]], dtype=object)

>>> a = np.array([[1,2,3,4],[5,6,7,'8']])
>>> a.astype('int')
array([[1, 2, 3, 4],
       [5, 6, 7, 8]])
```

numpy会自动找各个数据的最小公共父类（？）



array of 0s and 1s

```python
>>> a0 = np.zeros((3, 2),dtype=np.int32)
>>> a1 = np.ones((3, 2),dtype=np.int32)
>>> a0
array([[0, 0],
       [0, 0],
       [0, 0]])
>>> a1
array([[1, 1],
       [1, 1],
       [1, 1]])
>>> print(a0)
[[0 0]
 [0 0]
 [0 0]]
```



ndarray的相关操作：索引

将数据转换成ndarray对象后，会需要按照某种方式抽取数据

* 切片索引

* 布尔值索引

```python
>>> a = np.array([
...     [1,2,3,4],
...     [5,6,7,8],
...     [9,0,1,2]
... ])
>>> print('a=',a)
a= [[1 2 3 4]
 [5 6 7 8]
 [9 0 1 2]]
>>> print(a[:,1])
[2 6 0]
>>> print(a[2,:])
[9 0 1 2]
```



索引之后还可以对该位置重新赋值

```python
>>> a = np.array([
... [1,2,3],
... [4,5,6],
... [7,8,9]
... ])
>>> a[:,:2]=0
>>> a
array([[0, 0, 3],
       [0, 0, 6],
       [0, 0, 9]])
```



shape：可以用来获得数组的行数和列数，也可以被设定从而改变矩阵

```python
a.shape = 1,9
```



布尔值索引

```python
>>> a = np.array([
... [1,2,3,4],
... [5,6,7,8]
... ])
>>> b = a>=2
>>> print(a>=2)
[[False  True  True  True]
 [ True  True  True  True]]
>>> print(a[a>2])
[3 4 5 6 7 8]
>>> print(b)
[[False  True  True  True]
 [ True  True  True  True]]
```

逐位运算（element-wise）

加减乘除模



广播

当两个ndarray维度不一致时，则没有对齐的维度上会分别执行对位运算，称为广播。



矩阵的数乘

```python
>>> A = np.array([
... [1,2,3],
... [4,5,6],
... [7,8,9]
... ])
>>> print(3*A)
[[ 3  6  9]
 [12 15 18]
 [21 24 27]]
```



矩阵的乘法：使用@或者dot()

```python
>>> B=A
>>> A@B
array([[ 30,  36,  42],
       [ 66,  81,  96],
       [102, 126, 150]])
>>> A.dot(B)
array([[ 30,  36,  42],
       [ 66,  81,  96],
       [102, 126, 150]])
>>> np.dot(A, B)
array([[ 30,  36,  42],
       [ 66,  81,  96],
       [102, 126, 150]])
```



**numpy内置操作函数**

* 数学函数
* 运算函数
* 统计函数



统计最大值

```python
>>> a = np.array([
[1,2,3],[4,5,6],[7,8,9]
])
>>> print(np.amax(a))
9
>>> print(np.amax(a, axis=0))
[7 8 9]
>>> print(np.amax(a, axis=1))
[3 6 9]
```



axis=0, axis=1

指代着哪些东西？（不知道）



numpy常用的内置操作函数（略一下）

#### 用python做描述性统计



#### python科学计算生态圈：pandas

pandas使得我们可以快速处理结构化数据的大量数据结构





#### 用python做数据可视化

**用matplotlib画图**

前置要求：

```python
import numpy as np
import matplotlib.pyplot as plt
import pylab as pl
```



一个示例：

```python
import numpy as np
import matplotlib.pyplot as plt
import pylab as pl

x = np.linspace(-1, 10, 2000)
y = np.sin(x)
z = np.exp(-x)
a = []
for i in x:
	if i < 0:
		a.append(0)
	else:
		a.append(1)
a = np.array(a)
b = z*y
c = np.sin(x)/x
plt.figure(figsize=(8, 3))
plt.plot(x, y, label="$Sine$", color='blue', linewidth=2)
plt.plot(x, z, label='$Exponential$', color='red', linewidth=2)
plt.plot(x, a, label='$0-1Step$', color='purple',linewidth=2)
plt.plot(x, b, label='SineExpDecaying', color='orange', linewidth=2)
plt.plot(x, c, label='Testing',color='green', linewidth=2)
plt.xlabel('t')
plt.ylabel('x')
plt.xlim((-1, 10))
plt.ylim((-1.5, 1.5))
plt.legend() # 图例
plt.show() # 开始绘图
```

画出各种信号的图像











