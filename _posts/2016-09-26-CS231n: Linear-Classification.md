---
layout:     post
title:      "CS231n: Linear Classification"
subtitle:   "线性分类笔记"
date:       2016-09-26 15:00
author:     "Jianfei"
header-img: "img/post-bg-lc.jpg"
tags:        [CS231n]
category:   Course
---

## CS231n Introduction
[Stanford CS231n-Convolutional Neural Networks for Visual Recognition(Winter 2016)](http://cs231n.stanford.edu/)是一门讲述CNN在图像识别和分类中应用的课程，由Prof. Feifei Li授课。它包括两个模块，分别是Neural Networks和Convolutional Neural Networks，课程可用资源为[Course Notes](http://cs231n.github.io/)与[知乎翻译版](https://zhuanlan.zhihu.com/intelligentunit)。另外在Youtube中有完整版的[Lecture Video](https://www.youtube.com/playlist?list=PLLvH2FwAQhnpj1WEB-jHmPuUeQ8mX-XXG)。使用的数据集来自多伦多大学的[Cifar-10](https://www.cs.toronto.edu/~kriz/cifar.html)。

选择这门MOOC来自学，也是想在做其他领域的同时不抛弃原本做过的计算机视觉领域，并将机器学习与神经网络的知识融会贯通，说不定会在自己的领域有新的想法。

前几章铺垫的部分讲述了**Python与numpy库**的基本用法，并简单阐述了**图像分类**这一概念的目标与简单实现方法（KNN）。笔记我将直接从线性分类器开始写起。

### 线性分类
图像分类是从已有的固定标签中选择一个合适的标签给一张新的图像。k-Nearest Neighbor分类器通过比较新图像$$I_{test}$$与所有训练集$${I_{train}}$$的距离来判断它的标签。但是k-NN需要将所有的训练数据存储用于比较，存储与计算的复杂度高。

我们需要实现一种新的方法来解决图像分类问题。它包括两个部分：一个是**评分函数(score function)**，是从原始图像到类标的映射；另一个是**损失函数(loss function)**，用来量化预测结果与真实结果的一致性。于是该方法可以被转化为一个**最优化问题**，通过更新评分函数的参数来最小化损失函数的结果。

### 评分函数
评分函数将图像的像素值映射到各个类别的得分，而得分的高低代表了属于该类别的可能性高低。我们假设一个包含很多图像的训练集$$x_i \in R^D$$，对应一个分类标签$$y_i$$且$$y_i \in 1, ..., K$$。这里我们有个$$N$$训练样本，图像维度为$$D$$，有$$K$$种分类标签。在CIFAR-10中，$$N=50000$$，$$D=32\times32\times3$$，$$K=10$$，定义评分函数$$f: R^D\\to R^K$$。

最简单的概率函数就是线性映射，也就是线性分类器的评分函数：

$$f(x_i,W,b)=Wx_i+b$$

其中$$W$$被称为权重，$$b$$被称为偏差向量。输入图像$$x_i$$被拉长为一个长度为D的列向量。

这样训练后得到的权值矩阵就代表了训练数据给出的分类信息，而不用再保留大量的训练数据。而计算复杂度只是一个矩阵乘法和加法就够了。

<a href="#">
    <img class="img-responsive" src="http://cs231n.github.io/assets/imagemap.jpg" alt="" width="100%"></a>
首先将图像像素拉伸为一个列向量，与W进行矩阵乘，然后得到各个分类的分值。这个过程实际上是在高维空间当中的线性二值分类。线性分类器中的权重矩阵每一行对应着一个分类的模板，而图像不同分类的得分是通过计算图像与模板的内积得到的。模板是通过训练集训练得到的，而内积衡量了距离（相似度）。
<a href="#">
    <img class="img-responsive" src="http://cs231n.github.io/assets/templates.jpg" alt="" width="100%"></a>
另外，为了便于计算，我们把偏差向量放到权重矩阵的最后一列，并在测试图像的列向量中增加一个单位末尾元素，
<a href="#">
    <img class="img-responsive" src="http://cs231n.github.io/assets/wb.jpeg" alt="" width="100%"></a>

**数据预处理：**机器学习中我们常对特征值做normalization，而再图像分类时我们常常对每个特征值减去平均值来中心化数据。上述图像在中心化后将分布在$$[-127,127]$$之间，然后再根据某些规则使区间变为$$[-1,1]$$。总之，zero mean centering是非常重要的。

### 损失函数
损失函数(Loss function)也被称为代价(cost)函数和目标(objective)函数，用来衡量计算的得分(score)与图像真实标签的差异。损失越大，差异越大，评分函数越差。损失函数有很多种，这里学习的是多类支持向量机损失(Multiclass Support Vector Machine Loss)。

SVM损失是让正确分类始终比错误分类的得分高出一个阈值$$\Delta$$。给出第$$i$$个数据包括$$x_i, y_i$$，评分函数得到第$$j$$个类别的评分$$s_j=f(x_i,W)_j$$。那么这个数据的损失函数就是：

$$L_i=\sum_{j\neq y_i}max(0,s_j-s_{y_i}+\Delta)$$

就是得分s中每个分量，减去正确分类的分量再加上阈值。这就相当于统计了第i个数据下，所有其他得分与正确得分的安全距离，如果大于$$\Delta$$我们认为安全距离是0，如果距离小于阈值我们就会得到一个正数，就会增大损失函数。在线性评分函数下，损失函数可以展开为：

$$L_i=\sum_{j\neq y_i}max(0,w_j^T x_i-w_{y_i}^T x_i+\Delta)$$

而这里的$$max(0,-)$$是折叶损失(hinge loss)，将小于阈值的部分归为0。还有一种叫做L2-SVM，公式为$$max(0,-)^2$$用平方惩罚边界值，增大了惩罚。总的来说，损失函数形象地将不满意程度量化，使得正确类标得分与其他得分之间有一个满意的区间。
<a href="#">
    <img class="img-responsive" src="http://cs231n.github.io/assets/margin.jpg" alt="" width="100%"></a>

通过损失函数优化的评分函数$$W$$此时并不唯一，比如$$\lambda W$$的效果与其一模一样，都可以正确分类。我们通过引进一个正则化惩罚(regularization penalty $$R(W)$$)来抑制大数值的权重，下面是一个常用的L2范式正则化惩罚，它将所有的元素的平方和作为惩罚：

$$R(W)=\sum_k \sum_l W_{k,l}^2$$

最终的SVM损失函数由数据损失$$L_i$$和正则化损失组成：

$$L=\frac{1}{N}\sum_i L_i + \lambda R(W)$$

其中N是训练集数据量，$$\lambda$$需要通过交叉验证来获取。通过引入正则化损失，SVM就有了最大边界(max margin)。
>正则化为了解决目标函数会发生过拟合的问题，所以对模型的参数做一定的限制，使得模型偏好简单的参数。L1正则化使得优化后的参数向量比较稀疏，而L2正则化使得正则化项处处可导。

通过正则化，权重向量会变得更加分散，没有过大的权重值。也就是鼓励分类器更多的利用所有维度（像素）的特征，而不是依赖于少数几个元素进行分类（从而发生过拟合）。这称为提升了权重矩阵的泛化能力。偏差不需要进行正则化。

在实践使用时：
* 最大边界通常被设置成$$\Delta=1.0$$，因为它与另一个超参数$$\lambda$$同时控制了数据损失与正则化损失的权衡比例。这就决定了实际边界的大小。所以我们只需要确定一个来训练另一个正则化强度参数就可以了。

* 损失函数的最优化不限制初始条件。虽然损失函数大多不可微，但是通常可以使用次梯度(subgradient)求解。

* 这里的SVM是多种SVM公式中的一种，还有常见的是One-Vs-All(OVA) SVM，它对每个类与其他类训练独立的二元分类器。还有一种更少见的All-Vs-All(AVA) SVM。有一种有趣的SVM称为Structured SVM，它将正确分类的分类分值和非正确分类中的最高分值的边界最大化。

以下是一个数据损失的简单实现。
{% highlight python %}
def L_i(x, y, W):
  """
  unvectorized version. Compute the multiclass svm loss for a single example (x,y)
  - x is a column vector representing an image (e.g. 3073 x 1 in CIFAR-10)
    with an appended bias dimension in the 3073-rd position (i.e. bias trick)
  - y is an integer giving index of correct class (e.g. between 0 and 9 in CIFAR-10)
  - W is the weight matrix (e.g. 10 x 3073 in CIFAR-10)
  """
  delta = 1.0 # see notes about delta later in this section
  scores = W.dot(x) # scores becomes of size 10 x 1, the scores for each class
  correct_class_score = scores[y]
  D = W.shape[0] # number of classes, e.g. 10
  loss_i = 0.0
  for j in xrange(D): # iterate over all wrong classes
    if j == y:
      # skip for the true class to only loop over incorrect classes
      continue
    # accumulate loss for the i-th example
    loss_i += max(0, scores[j] - correct_class_score + delta)
  return loss_i

def L_i_vectorized(x, y, W):
  """
  A faster half-vectorized implementation. half-vectorized
  refers to the fact that for a single example the implementation contains
  no for loops, but there is still one loop over the examples (outside this function)
  """
  delta = 1.0
  scores = W.dot(x)
  # compute the margins for all classes in one vector operation
  margins = np.maximum(0, scores - scores[y] + delta)
  # on y-th position scores[y] - scores[y] canceled and gave delta. We want
  # to ignore the y-th position and only consider margin on max wrong class
  margins[y] = 0
  loss_i = np.sum(margins)
  return loss_i

def L(X, y, W):
  """
  fully-vectorized implementation :
  - X holds all the training examples as columns (e.g. 3073 x 50,000 in CIFAR-10)
  - y is array of integers specifying correct class (e.g. 50,000-D array)
  - W are weights (e.g. 10 x 3073)
  """
  # evaluate loss over all examples in X without using any for loops
  # left as exercise to reader in the assignment
{% endhighlight %}

### Softmax分类器
Softmax分类器可以理解为逻辑回归分类对多类问题的一般化归纳。保持评分函数不变，将评分视为每个分类未归一化的对数概率。折叶损失换为交叉熵损失(cross-entropy loss)，数据集的损失依然表示为损失值和正则化损失之和。交叉熵损失公式如下：

$$L_i=-log(\frac{e^{f_{y_i}}}{\sum_j e^{f_j}})$$

其中函数被称为**softmax函数**，输入值是评分向量，函数对其进行压缩，输出为一个元素为0-1之间的向量，且所有元素之和为1。下面从几个视角对交叉熵损失进行解释：
* **信息理论：**在信息论中，真实分布$$p$$与估计分布$$q$$之间的交叉熵定义为：

$$H(p,q)=-\sum_x p(x) logq(x)$$

比较这个函数与softmax损失函数，我们发现softmax函数是最小化估计分类概率$$\frac{e^{f_{y_i}}}{\sum_j e^{f_j}}$$与真实分布之间的交叉熵。（真实分布是一个除了真实位置是1其他都为0的向量）另外，交叉熵可以写作熵与相对熵（Kullback-Leibler divergence差异，衡量相同事件空间中两个概率分布的差异）的和：

$$H(p,q)=H(p)+D_{K,L}(p||q)$$

也就是最小化两个分布之间的相对熵，使得预测分布的概率密度都在正确分类上。

* **概率论：**评分函数我们看作是没有归一化的对数概率：

$$P(y_i|x_i,W)=\frac{e^{f_{y_i}}}{\sum_j e^{f_j}}$$

softmax函数将其转化为了归一化的概率分布，最小化正确分类的负对数概率相当于最大似然估计（MIE），而正则化部分$$R(W)$$可以被看作权重矩阵W的高斯先验，那么这里做的就是最大后验估计（MAP）。

* **实践技巧：**在实际编程时，会遇到指数和过大的问题，所以要将数值进行归一化，也就是在分式的分子分母都乘以一个常熟C，使得数值计算更加稳定。通常我们取$$logC=-max_j f_j$$，将向量$$f$$中的数值进行平移使得最大值为0。

$$\frac{Ce^{f_{y_i}}}{C\sum_j e^{f_j}}=\frac{e^{f_{y_i}}+logC}{\sum_j e^{f_j}+logC}$$

{% highlight python %}
f = np.array([123, 456, 789]) # 例子中有3个分类，每个评分的数值都很大
p = np.exp(f) / np.sum(np.exp(f)) # 不妙：数值问题，可能导致数值爆炸
# 将f中的值平移到最大值为0：
f -= np.max(f) # f becomes [-666, -333, 0]
p = np.exp(f) / np.sum(np.exp(f)) # 现在OK了，将给出正确结果
{% endhighlight %}

### 对比SVM与Softmax
SVM给出是否是该分类的判断，而softmax给出了对于不同分类准确率的把握。而可能性分布的集中与离散程度由正则化参数$$\lambda$$决定。当正则化参数越大，那么权重W会被惩罚的越多，那么它的权重数值就会更小，而输出的概率分布就会更加均匀离散。

另外，SVM在超过安全距离后就满足了，W此时就不再变化；而softmax对分数永远不会满意，总是可以对损失值进行最小化。

<a href="#">
    <img class="img-responsive" src="http://cs231n.github.io/assets/svmvssoftmax.png" alt="" width="100%"></a>

线性分类器的相关内容就是这些，后面将学习如何高效得到使损失值最小的参数，称为最优化过程。
