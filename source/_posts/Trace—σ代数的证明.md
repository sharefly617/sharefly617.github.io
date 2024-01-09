---
title: Trace—σ代数的证明
date: 2022-06-02
tags: 统计学
categories: 信号
thumbnail: https://tva3.sinaimg.cn/mw690/94aee95bgy1h2tsk2u7lej22bc1jk7nx.jpg
mathjax: true
---
<meta name="referrer" content="no-referrer" />
本文旨在证明 Trace $\sigma$ algebra 是一个algebra。Trace $\sigma$ algebra 的定义是：

> 假设$E\subseteq  X$，且$\mathscr{A}$是X上的$\sigma$代数，那么
$\mathscr{A}_{E}$ $\vcentcolon=$ $\{E \cap{A} , A\in\mathscr{A}\}$是一个$E$上的$\sigma$ algebra。

>证明，根据定义的三条，第一条$E\in\mathscr{A}_{E}$明显满足。
>第二条，需要证明$E$的子集$A\in\mathscr{A}_{E}$那么$E\setminus A\in\mathscr{A}_{E}$
>假设一些S=E$\cap$A$,$A$\in$$\mathscr{A}_E$，因此，$X\setminus S\in\mathscr{A}$,那么根据定义$E\cap(X\setminus S)\in\mathscr{A}_E$而$E\cap(X\setminus S)=(E\cap X)\setminus S=E\setminus S$,所以$E\setminus S \in \mathscr{A}_E$。

第三条需要证明的条件是：$A_i\in \mathscr{A}_E,那么\cup _{i\in\mathbb{N}}A_i\in\mathscr{A}_E$。
证明过程如下：
根据定义假设$S_i=E\cap A_i$其中$A_i \in \mathscr{A}$,那么$\cup S_i=E\cap \{\cup A_i\}$，由于$\mathscr{A}$是$\sigma$-algebra，所以$\{\cup A_i\} \in \mathscr{A}$ ,根据定义$\cup S_i \in \mathscr{A}_E$.