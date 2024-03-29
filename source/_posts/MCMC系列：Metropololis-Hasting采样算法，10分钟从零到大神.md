﻿---
title: MCMC系列：Metropololis-Hasting采样算法，10分钟从零到大神
date: 2022-06-02
tags: 统计学
categories: 信号
thumbnail: /images/MCMC/Thumbnail.jpg
mathjax: true
---
# MCMC

面对已知的复杂分布时，仅知其表达式，对其进行采样变得格外棘手（甚至无法采样）。在仿真中，各工具包会提供一些标准分布，MCMC采样方法允许我们通过构造一个马尔科夫链，并借助标准分布，使得以某种接受概率采样的新马尔科夫链的平稳分布为我们的目标复杂分布。

## Metropolis-Hasting

MCMC中很重要且很实用的一个采样算法就是MH算法。

### MH算法流程(怎么做)

![在这里插入图片描述](https://img-blog.csdnimg.cn/da80fe5bb4c94854b8a02a35b5cb1702.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoYXJlNzI3MTg2NjMw,size_16,color_FFFFFF,t_70#pic_center)




### 这么做为什么行(MH推导)

#### 从马尔可夫链谈起(重点版)

如何构建一个平稳分布为目标分布马尔科夫链？可在此简要介绍会用到的马尔可夫链的重要性质(假设读者了解马尔科夫链的基本概念)：

*稳态分布*  **什么是稳态分布呢(stationary/invariant distribution)?** 定义$(X_n)_{n\geq 0}$为马氏链$(\lambda,P)$。当马尔可夫链$(X_n)_{n\geq 0}$的状态分布为$\lambda$时，此时若$\pi$满足
$$\pi P=\pi$$
那么$pi$则是该马尔科夫链的平稳分布。通俗理解即为某一时刻马尔科夫链的状态服从某一个分布之后，再进行转移，其状态仍然服从此分布(服从分布不是状态保持不变)。至此，已经阐明为何可以使用马尔可夫链进行采样。

但是如何找到平稳分布呢？MH中利用了马尔科夫链的细致平衡方程(detailed
balance equations),满足细致平衡方程的的分布就是马氏链的平稳分布。


**定理 1.1**. 
*如果存在一个分布$\pi$满足$$\pi_i P_{ij} = \pi_j P_{ji},$$ 那么$\pi$就是该马氏链的平稳分布。*


**Proof.** *$$\sum_{i} \pi_i P_{ij}=\sum_{i}\pi_j P_{ji}=\pi_j$$ ◻*

#### 回到MH算法（Metropolis Hastings algorithm） 

介绍完马氏链的平稳分布后，可以发现，MH算法就是在构造马氏链满足细致平稳条件的转移概率，使其平稳分布为我们的目标分布。在中的第5行构造了转移的接受概率，马氏链的转移分布由提议分布$q$决定，此时$P_{ij}=q(x_j|x_i)$。考虑$\alpha_{ij}<1$的情况，有
$$p_i Q_{ij} = p_i P_{ij} \alpha_{ij} =p_i q(x_j|x_i) \frac{p_jq(x_i|x_j)}{p_i q(x_j|x_i)}=p_jq(x_i|x_j)\alpha_{ji}=p_j Q_{ji}$$
可以发现这样构造出来的新马氏链(实际上叫做MH链或者MH马氏链)的平稳分布就是需要被采样的目标分布了。那么对这个MH链采样一段时间后(burn
in)，MH链会收敛到平稳分布。

#### 问题再现，再次回到马氏链

读者不免会产生一定的疑问:对于一个马氏链来说平稳分布一定存在且唯一吗？会不会存在一个马氏链有多个平稳分布的情况呢？老规矩，先说结论：对于有限空间的马尔科夫链，平稳分布一定存在，对于不可约的(状态互通)马尔科夫链，平稳分布就是唯一的。


**Proof.** 
**声明！本证明已掏空笔者！！**
首先解释何为**不可约(状态互通)**，即状态之间能够互通。从状态$i$经过$n$步之后可以到达状态$j$，即$Pr(x_n=j|x_0=i,n<-\infty)>0$。状态矩阵$P^n$中的每一个元素都大于$0$，所以，不可约马氏链的状态矩阵是一个正则矩阵(regular matrix)。\[*$A$ is called regular if for some $k>1$,$A^k>0$.*\]根据Perron-Frobenius定理，若一个矩阵$A$是正则矩阵，则

1.  $A$存在一个正实特征值$\lambda_{PF}$，其对应的左、右特征向量也均为正特征向量。

2.  $A$的所有其他特征值都存在$\lambda_i < \lambda_{PF}$。

3.  $\lambda_{PF}=1$，且其左、右特征向量的模为$1$。

那么马氏链的转移矩阵$P$只有一个特征向量$\pi$能够满足
$\pi P=1\cdot\pi$ ◻
证明了不可约的马氏链只一个单独的平稳分布之后，就容易理解为什么满足细致平衡方程的分布就是批评翁分布。至此，MH为什么可行就得到了证明。

#### 真的结束了吗？补充证明

真的结束了吗。不妨思考，需要被采样的空间是一个有限空间吗？对目标分布进行采样，明明可以得到无数的采样值。这样之前证明的前提便全部被推翻了呀！
在此，引入新的概念，马尔科夫链的另一种分类，countable-state
chain，旨在描述类似连续分布采样过程的离散时间无限空间的马尔科夫链。那无限空间的马氏链仍然只有唯一的平稳分布吗。结论：对于不可约的马尔科夫链，如果
$$\pi P=\pi$$有解，那么这个解是唯一的。相反，如果马氏链是正常反的，那么就一定有解。在此不对上述结论进行证明，有意者可以参考\<Stochastic
Processes:Theory for Applications>的第298页的定理6.3.8.
