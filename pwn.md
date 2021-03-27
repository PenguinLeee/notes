# pwn

50年前，栈溢出的基本思路：不check输入的话可能被攻击

## 20年前，栈溢出的第一个payload，之后被经常利用；在之后保护机制：NX：堆&栈上数据不可被当成代码执行/Canary保护机制（防止篡改ebp和esp，通过在ebp上面放一个东西达成。），之后栈溢出逐渐匿迹
基本思路：通过溢出修改局部变量，返回地址，ebp等等

leave指令：将函数栈帧清掉，把栈弹回去

函数调用过程：

```
main -> func(a, b)

b
a
main_next_addr
#以下正常执行func(a, b)
main_ebp #为了使func自己正常开辟空间。找参数时向上，找局部变量时向下
	<- func_ebp = func_esp
s1	
s2	
s3	
s4	
...	
	<- func_esp
	
# leave时，把自己的空间删掉，将main_ebp还原
# ret时会找到main_next_addr

# leave：恢复主调函数的栈帧准备返回（清栈）。等价于：
# mov %ebp, %esp（恢复原esp值，指向被调函数栈帧开始处）
# pop %ebp（恢复原ebp值，即主调函数栈基指针）

ret=pop eip（取回栈顶元素）
```

32位和64位程序的区别：

X86：参数在函数返回地址的上方

X64：前六个参数在rdi, rsi, rdx, rcx, r8和r9寄存器里。如果有更多参数才会上栈



栈溢出：把栈顶改掉，则ret时会返回到一个被改掉的地址？

### 栈溢出原理

结论：局部变量地址相邻，且和ebp, eip有关系

程序向栈中某个变量中写入字节数超过变量本身的字节数，导致相邻变量被改变。

轻则程序崩溃，重则被控制

基本前提：

* 程序向栈上写入数据
* 写入数据的大小没有被控制


## ROP：面向返回编程

## ASLR
## 格式化字符串

## 堆利用

### 15年前unlink

### bin的拆链过程造成恶意地址写入

### glibc更新之后check：P->fd->bk == P即不易利用



### The malloc fiarum

### 6个House（比较新颖的利用方式）

1年之后才出payload

### glibc更新之后只可利用house of force,house of spirit，条件较苛刻

### fastbin attack / unsorted bin attack

#### house of lore

### off by null/one，构造堆溢出

## IO FILE

### fread(0, buf, size)

可以修改stdin/stdout，来做虚表劫持





