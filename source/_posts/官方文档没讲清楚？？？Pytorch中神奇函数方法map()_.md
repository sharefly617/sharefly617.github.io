---
title: 官方文档没讲清楚？？？Pytorch中神奇函数方法map()
date: 2022-06-02
tags: Pytorch
categories: 编程
thumbnail: https://tvax3.sinaimg.cn/mw690/94aee95bgy1h2tsmk5ivwj24mo334qv6.jpg
mathjax: true
---
<meta name="referrer" content="no-referrer" />
在python中有一个高级函数叫做map()
引用廖雪峰的讲解

> 如果你读过Google的那篇大名鼎鼎的论文“MapReduce: Simplified Data Processing on Large Clusters”，你就能大概明白map/reduce的概念。
> 我们先看map。map()函数接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。
> 举例说明，比如我们有一个函数$f(x)=x^2$，要把这个函数作用在一个list [1, 2, 3, 4, 5, 6, 7, 8, 9]上，就可以用map()实现如下：

![$f(x)=x2$](https://img-blog.csdnimg.cn/20200721170555544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoYXJlNzI3MTg2NjMw,size_16,color_FFFFFF,t_70#pic_center)
代码实现如下：
```
def f(x):
	 return x * x
 r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
 list(r)
 [1, 4, 9, 16, 25, 36, 49, 64, 81]
```
说的很清楚了，返回的是一个$Iterator$,那么在pytorch中相对torch.Tensor中的每一个element进行函数运算有没有torch的函数呢，我查了下，还真有。来看看官方文档的教程

```
map_(tensor, callable)
Applies callable for each element in self tensor and the given tensor and stores the results in self tensor. 
self tensor and the given tensor must be broadcastable.

The callable should have the signature:
def callable(a, b) -> number
```
等等？这是神马？完全没说清楚啊，$callable()$我可以理解，就是可调用的对象。但是这个参数$(a,b)$是哪里来的？？？？$python$中的$map$不是$f$接受$Iteratorable$里面返回的每一个对象吗，返回一个就是一个参数，返回两个就是两个参数啊，这里为啥是$(a,b)$两个参数？？？？
假设有一个tensor,我想令每一个元素的操作编程$f = 4\times{x}+10$

```python
improt torch
def f(x): return 4*x+10
torch.randint(0,3,(2,3))
print(x)
```

```python
tensor([[0, 0, 0],
        [0, 1, 2]])
```
这个时候对$x$用上$map\_()$函数

```python
x.map_(x,f)

出现的是这个：TypeError: f() takes 1 positional argument but 2 were given
```
咦，怎么还真是传入了两个参数啊，这两个参数是啥啊？？
**好了这里就不卖关子了** ，这两个参数其实就是$Iteratorable$里面返回的每一个对象的两份同样的值，也就是说$a=b$,那我们需要把函数改一下,传入两个参数，但是只要一个就行了，或者使用多参数的形式$*karg$：

```python
import torch
def f(x,*y): return 4*x+10
x = torch.randint(0,3,(2,3))
print('原来的x是\n{}'.format(x))
x.map_(x,f)
print(x)
```

```python
原来的x是
tensor([[1, 0, 0],
        [2, 2, 2]])
tensor([[14, 10, 10],
        [18, 18, 18]])
```
这样就完成了我们的目标，代码也变得简洁了很多，是不是很开心呢？

**原创不易，点个赞再走吧！**

