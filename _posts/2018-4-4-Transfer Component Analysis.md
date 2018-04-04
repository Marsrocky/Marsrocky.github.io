---
layout:     post
title:      "Transfer Component Analysis"
subtitle:   "迁移成分分析"
date:       2018-04-04 21:53
author:     "Jianfei"
header-img: "img/post-bg-tr.jpg"
tags:        [TCA]
category:   Transfer Learning
---

## 引子
这篇文章诞生于2009年的IJCAI，也是Transfer Learning中公认的经典之作。迁移学习解决的问题是在数据不足导致Model的泛化能力不足时，应当如何通过unlabeled data提高模型的泛化能力。

#### 具体问题
TCA想要解决的问题是迁移学习中的一个有趣问题，叫做Domain Adaptation。 说有两个domain，分别是已知类标的Source Domain $$D_S$$和未知类标的Target Domain $$D_T$$，sample数量分别是$$n_1$$和$$n_2$$。那么输入数据的边缘分布$$P(X_S)$$和$$Q(X_T)$$是不同的，那么TCA的目的就是用labelled source data去预测unlabelled target data。

#### 基本假设

$$ P \neq Q $$

$$ P(Y_S|X_S)=P(Y_T|X_T)$$

## 正文
要想在两个domain的数据上使用传统机器学习方法，需要让他们的数据分布满足$$P=Q$$。既然他们本身是不相等的，那么是否可以寻找一个nonlinear transformation $$\phi$$，使得$$P'(\phi(X_S))=Q'(\phi(X_T))$$，映射后的分布就满足大多数分类回归方法的条件了！

那么上述的假设就变成了：

$$ P(Y_S|\phi(X_S))=P(Y_T|\phi(X_T))$$

而如何将两个分布拉到一起呢，这可以等同于一个优化问题，最小化两个分布的距离。用什么距离呢，这里用的是Maximum Mean Discrepancy (MMD)，这里不使用很多其他衡量方法例如KL散度，因为这些方法往往都是参数化的，很难处理。MMD是NIPS2006的一篇文章提出的，文章使用了引理“在选定函数$$f \in C(X)$$为continuous bounded functions时，two sample test中的分布$$p=q$$与$$E_p(f(x))=E_q(f(x))$$是充要条件”提出了使用reproducing kernel Hilbert space (RKHS)中的核函数，并证明了他们的estimate是相当成功的。

那么衡量两个新的domain在kernel下的分布距离度量可以写成：

$$ Dist(X'_S,X'_T) = ||  \frac{1}{n_1}\sum_{i=1}^{n_1}\phi(x_{S_i})-\frac{1}{n_2}\sum_{i=1}^{n_2}\phi(x_{T_i})  ||^2_H $$

在做这个优化问题的时候，之前的工作Maximum Mean Discrepancy Embedding (MMDE)使用了SDP半正定规划去学习一个kernel matrix，但是相当复杂。为了避免显式地计算映射$$\phi$$，作者将其转化为一个kernel learning problem，应用kernel trick $$k(x_i,x_j)=\phi(x_i)' \phi(x_j)$$，MMD距离可以被写成新的形式：

$$ Dist(X'_S,X'_T) = tr(KL) $$

其中

$$ K = \begin{bmatrix} 
K_{S,S} & K_{S,T} \\
K_{T,S} & K_{T,T}
\end{bmatrix} $$

而L就是用来凑系数的矩阵。

基本思路已经明白了，但是这时便是作者展现数学功底的时候了，只见他三下五除二，先用一个降维后的矩阵$$W$$使得

$$ K = (KK^{-1/2}\tilde{W})(\tilde{W}^T K^{-1/2}K)=KWW^TK $$

代入原问题，于是问题转化成了更简单的形式：

$$ min_W \quad tr(W^TW)+\mu tr(W^TKLKW) $$

$$ s.t. \quad W^TKHKW=I $$

这里我就要问了，H是啥玩意？莫名其妙就说是一个center matrix，这个条件就是TCA的第二个目的，在保持各自数据特征的前提下做domain拉近，而保持的散度特征正是$$AHA^T$$，这里和PCA有点类似，降维后保持数据特性不变。

下面解这个问题的时候，因为条件非凸，需要先做一个转化，再求它的dual problem就可以解了，解的答案简单明了，W就是求$$(I+\mu KLK)^{-1}KHK$$的前$$m$$个特征向量，而W的维度正是$(n_1+n_2) \times m$，相当于把所有数据都在分布拉近的同时，降维到了$$m$$维度上，后面就可以很简单的使用各种方法进行分类和回归了。

## 后记

不得不说，很有趣的问题转化，思路也清楚明白，就是要让我解这个优化问题，那真是老费劲了。科科。








