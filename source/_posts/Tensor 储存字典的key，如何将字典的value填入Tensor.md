---
title: Tensor 储存字典的key，如何将字典的value填入Tensor
date: 2022-06-02
tags: tensorflow
categories: 编程
thumbnail: https://tva3.sinaimg.cn/mw690/94aee95bgy1h2tsl9v8srj21hc0u0k3u.jpg
mathjax: true
---
最近遇到一个问题，假设一个Tensor的元素都是标签（字典的key），这里我们还有一个对应的字典，如果想把key对应的value填回tensor，怎么操作呢？
当然如果是在numpy中，我们可以循环遍历这个字典，然后修改元素的值，但是在tensor中是没有这么容易可以修改特定的元素的数值的。可能有些同学会自然想到把tensor转换成numpy.array再进行操作即可。这种想法不错，但是在使用keras 或者tensorflow时，在这里我默认大家在进行机器学习。显然循环操作和转换会大大减小效率。
tf和keras中的gather函数可以解决这个问题，这里我以keras为例。
***gather函数的官方解答***
`keras.backend.gather(reference, indices)`

```
在张量 reference 中检索索引 indices 的元素。
参数
reference: 一个张量。
indices: 索引的整数张量。
返回 ：与 reference 类型相同的张量。
Numpy 实现
def gather(reference, indices):
    return reference[indices]
```
说白了就是在我们需要实现的功能，只不过这里的字典的key是一些索引，所以仅限数字，而且类型还的是int64


__上实战代码：__

```
import keras.backend as K

a = [[[1],[2],[2]],[[1],[1],[2]]]
variable = K.constant(a,dtype = 'int32')
d1,d2,d3 = [1,2,3,1],[2,3,4,2],[3,4,5,5]
c = {0:d1,1:d2,2:d3}
#获取dict的所有value
value = K.constant(list(c.values()))
```
==我们来看看value的值==

```
K.eval(value)
Out[3]: 
array([[1., 2., 3., 1.],
       [2., 3., 4., 2.],
       [3., 4., 5., 5.]], dtype=float32)
属性：Tensor("Const_1:0", shape=(3, 4), dtype=float32)
```
接着操作   `K.gather(value,variable)`

```
Out[6]: 
array([[[[2., 3., 4., 2.]],
        [[3., 4., 5., 5.]],
        [[3., 4., 5., 5.]]],
       [[[2., 3., 4., 2.]],
        [[2., 3., 4., 2.]],
        [[3., 4., 5., 5.]]]], dtype=float32)
Tensor("embedding_lookup_1/Identity:0", shape=(2, 3, 1, 4), dtype=float32)
```
塔哒成功了，帮助到了你就给我评论下吧
