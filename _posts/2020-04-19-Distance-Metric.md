---
layout:     post
title:      "Distance Metric"
subtitle:   "数据科学基础课笔记（五）"
date:       2020-04-19 20:30:00
author:     "AH"
header-img: "img/post-bg-2015.jpg"
tags:
    - Data Science
---



容易计算两点之间、两灰度图之间的距离，那如何计算两种纹理之间的距离呢？

<img src="/img/in-post/post-distance-metric/how2measure.png" width="500">

# 两个样本间距离的度量方法

### 闵可夫斯基距离 (Minkowski Distance)

$$
\begin{aligned}
d(x, y) &= (\sum_i \mid x_i - y_i \mid^p)^\frac{1}{p} \\
&= (\sum_i \mid d_i \mid^p)^\frac{1}{p}
\end{aligned}
$$

$L_p$范数本质上就是向量和零向量之间的闵可夫斯基距离，再次祭出这张图：

<img src="/img/in-post/post-feature-engineering/Lp.jpg" width="500">

一些特殊情况：

* 当$p=\infty$时，是切比雪夫距离$d(x, y) = \max_i \mid x_i - y_i \mid$
* 当$p=2$时，是欧氏距离$d(x, y) =\sqrt{\sum_i (x_i - y_i)^2}$
* 当$p=1$时，是曼哈顿距离$d(x, y) = \sum_i \mid x_i - y_i \mid$

### 余弦距离 (Cosine Distance)

距离和相似是相关的概念，距离越大，相似度越低；距离越小，相似度越高。

可以定义余弦相似度

$$
Sim_{cos}(x, y) = \frac{x^Ty}{\parallel x \parallel \parallel y \parallel}
$$

它与欧氏距离关系：

当$\parallel x \parallel = \parallel y \parallel =1$时，$d_{euc}(x, y) = \sqrt{2-2x^Ty} = \sqrt{2-2sim_{cos}(x, y)}$

由于上面余弦相似度的式子取值范围在$[-1, 1]$，通常也会将它归一化到$[0, 1]$的区间内，也就是

$$
\frac{1}{2}[Sim_{cos}(x, y)+1]
$$

#### 如果不同维度的度量方式不同呢？

（如$Person = [height(cm), weight(kg)]$，$L_p$范数没有意义）

-> **Z-Score变换**（见[第二章的笔记](https://abigailhua.github.io/2020/03/29/Feature-Engineering/#z-score%E5%BD%92%E4%B8%80%E5%8C%96)）



#### 再如果两个样本都完全不相似呢？

（如图片和文本）

👇

### **度量学习** (Metric Learning)

idea: 将位于两个不同空间的样本映射到一个共同的空间里，即$d(x, y) \Rightarrow d(Px, Py)$

<img src="/img/in-post/post-distance-metric/metric-learning.png" width="500">

与之前在数据降维中介绍的feature projection类似，我们需要学习一个投射矩阵$P$

马氏距离 (Mahalanobis Distance)是一种经典的度量学习中的投射方案，它是欧氏距离的推广：

$$
d(Px, Py) = \parallel Px - Py \parallel = \sqrt{(x^T-y^T)P^TP(x-y)} = \sqrt{(x^T-y^T)M(x-y)}
$$

我们可以把度量学习写成一个优化问题：

$$
\begin{aligned}
\max_M &\quad \sum_{(x_i, x_j)\in \mathcal{D}} d_M (x_i, x_j) \\
s.t. &\quad \sum_{(x_i, x_j)\in \mathcal{S}} d_M (x_i, x_j) \leq 1 \\
&\quad M \succeq 0
\end{aligned}
$$

其中$(x_i, x_j)\in \mathcal{D}$指两个样本分别属于不同的类别，$(x_i, x_j)\in \mathcal{S}$指的是两个样本属于同一个类别。我们优化的目的是使得不同类之间的距离最大，同时限制类内的距离。此外，$M$是一个半正定矩阵。

### 搬土距离 (EMD, Earth Mover's Distance)

首先看一个例子：两张图片A和B，如何计算它们之间的距离？

当然可以先分别提取两张图的特征，然后计算特征向量间的距离。但还有一种思路，对两张图分别统计颜色直方图。具体方法是，将图片按RGB分成多个bin，然后对每个bin进行投票，做成颜色直方图。

<img src="/img/in-post/post-distance-metric/hist.png" width="500">

这个问题可以形式化为计算$\mathcal{S}$和$\mathcal{T}$之间的距离，其中
$$
\begin{aligned}
&\mathcal{S} = \{(x_1^s, w_1^s), (x_2^s, w_2^s), ..., (x_m^s, w_m^s)\} \\
&\mathcal{T} = \{(x_1^t, w_1^t), (x_2^t, w_2^t), ..., (x_n^t, w_n^t)\}
\end{aligned}
$$
这里用$m,n$表示两张图bin的数量，因为它们可能不一样。$w$是对bin的投票结果，所以每个bin都有权重。不同数量的、各自带有权重的bin计算距离，就可以用EMD

可以形象地把$\mathcal{S}$和$\mathcal{T}$分别想象成一些土堆和一些土坑，每个土堆到土坑的距离不一定相同。搬运工要做最小的功将土填到坑里，直到土用完或者坑填完。
$$
\begin{aligned}
\min_{f_{ij}} &\quad \sum_{i=1}^m \sum_{j=1}^n f_{ij}d_{ij} \\
s.t. &\quad f_{ij}\geq 0, 1\leq i \leq m, 1\leq j \leq n, \\
&\quad \sum_{j=1}^n f_{ij} \leq w_i^s, 1 \leq i \leq m, \\
&\quad \sum_{i=1}^m f_{ij} \leq w_j^t, 1 \leq j \leq n, \\
&\quad \sum_{i=1}^m \sum_{j=1}^n f_{ij} = \min(\sum_{i=1}^m w_i^s, \sum_{j=1}^n w_j^t) \\
\end{aligned}
$$


# 两个分布间距离的度量方法