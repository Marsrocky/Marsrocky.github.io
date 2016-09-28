---
layout:     post
title:      "CS231n: Optimization"
subtitle:   "最优化笔记"
date:       2016-09-28 10:36
author:     "Jianfei"
header-img: "img/post-bg-lc.jpg"
tags:        [CS231n]
category:   Course
---

## 最优化 Optimization

最优化就是通过计算权重$W$来最小化损失函数。如果损失函数是SVM损失，那么这个凸函数是可以用凸优化的方法进行的。但是当损失函数扩展成为神经网络时，那么目标函数就不是凸函数了，而且损失函数中存在不可导点(kinks)，使得这些点不可微，在这些点也没有梯度的定义。但是次梯度(subgradient)依然可以使用。

我们从最基本的方案开始逐步过渡到梯度的方法。

### **随机搜索**

大量随机生成权重矩阵，然后比较它们的损失值，取最小的权重作为我们要找的权重去跑测试集。

{% highlight python %}
# 假设X_train的每一列都是一个数据样本（比如3073 x 50000）
# 假设Y_train是数据样本的类别标签（比如一个长50000的一维数组）
# 假设函数L对损失函数进行评价

bestloss = float("inf") # Python assigns the highest possible float value
for num in xrange(1000):
  W = np.random.randn(10, 3073) * 0.0001 # generate random parameters
  loss = L(X_train, Y_train, W) # get the loss over the entire training set
  if loss < bestloss: # keep track of the best solution
    bestloss = loss
    bestW = W
  print 'in attempt %d the loss was %f, best %f' % (num, loss, bestloss)

# 测试函数
# 假设X_test尺寸是[3073 x 10000], Y_test尺寸是[10000 x 1]
scores = Wbest.dot(Xte_cols) # 10 x 10000, the class scores for all test examples
# 找到在每列中评分值最大的索引（即预测的分类）
Yte_predict = np.argmax(scores, axis = 0)
# 以及计算准确率
np.mean(Yte_predict == Yte)
# 返回 0.1555
{% endhighlight %}

当随机猜测时，正确概率是10%，这里用多次筛选得到随机矩阵提升了一点。如果我们能够设计一个迭代算法，使得每次迭代后的损失值都减少一点，那么就能找到更好的$W$。

### **随机本地搜索**

设计一个迭代算法，每一次都随机尝试几个方向，如果方向是向下（能让损失值小一点），那就向该方向走一步。依然从随机的$W$开始，每次给予一个随机扰动$\delta W$，如果扰动后损失值更小了，就更新权重矩阵。

{% highlight python %}
W = np.random.randn(10, 3073) * 0.001 # 生成随机初始W
bestloss = float("inf")
for i in xrange(1000):
  step_size = 0.0001
  Wtry = W + np.random.randn(10, 3073) * step_size
  loss = L(Xtr_cols, Ytr, Wtry)
  if loss < bestloss:
    W = Wtry
    bestloss = loss
  print 'iter %d loss is %f' % (i, bestloss)
{% endhighlight %}

用同样的方法，相比上述方法准确率更高（20%），但是因为迭代过程是随机的，所以训练时间和效果都不稳定。

### **跟随梯度**

这里的迭代算法保证了每次都是走向局部最优的低处，这个方向就是损失函数的梯度(gradient)。**梯度是各个维度的斜率组成的向量(导数derivatives)**。当函数有多个参数时，梯度是每个维度上偏导数所形成的向量。


<a href="#">
    <img class="img-responsive" src="" alt="" width="100%"></a>

