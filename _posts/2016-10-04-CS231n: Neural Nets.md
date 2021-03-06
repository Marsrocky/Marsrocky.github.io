---
layout:     post
title:      "CS231n: Neural Nets"
subtitle:   "神经网络笔记"
date:       2016-10-04 10:36
author:     "Jianfei"
header-img: "img/post-bg-lc.jpg"
tags:        [CS231n]
category:   Course
---

## 神经网络 Neural Networks

神经网络算法的计算公式是$$s=W_2 max(0, W_1 x)$$。$$W_1$$可以将图像转化为一个相比分类个数来说高维一些的特征过渡向量。比如对于CIFAR-10中，该矩阵大小为$$[100\times 3072]$$。函数$$max(0, ~)$$为非线性，会作用到每个元素，这个非线性函数可以更换。第二个矩阵$$W_2$$则是$$[10\times100]%$$，可以得到最终的10个分类评分。两个参数矩阵可以通过随机梯度下降来学习（梯度通过反向传播的链式法则求导计算），而神经网络的关键改变在于非线性函数的引入。最终，一个三层的神经网络可以写成$$s=W_3 max(0,W_2 max(0,W_1 x))$$，其中包括三个权重矩阵需要学习，而中间隐层的尺寸是网络而超参数。

### 单个神经元

生物学中的神经元通过树突获得输入信号，然后沿着他唯一的轴突产生输出信号。从仿生的方向考虑，单个神经元的计算模式为基于其他神经元输入的突触强度($$w_i$$)，与其他神经元的树突进行乘法交互。突触的强度可以控制一个神经元对于另一个神经元的影响强度，使其兴奋或者抑制。各输入信号在胞体中进行叠加，若高于某阈值则会激活神经元，向轴突输出一个峰值信号。神经元的激活率建模为激活函数，它常常选择使用sigmoid函数，它将输入信号值压缩到0-1之间。

{% highlight python %}

{% endhighlight %}

<a href="#">
<img class="img-responsive" src="{{ site.baseurl }}/img/post-backprop.jpg" alt="" width="100%"></a>

