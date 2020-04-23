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



两点之间、两灰度图之间距离能够计算，如何比较两种纹理呢？

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

$$
Sim_{cos}(x, y) = \frac{x^Ty}{\parallel x \parallel \parallel y \parallel}
$$

与欧氏距离关系：

当$\parallel x \parallel = \parallel y \parallel =1$时，$d_{euc}(x, y) = \sqrt{2-2x^Ty} = \sqrt{2-2sim_{cos}(x, y)}$



#### 如果不同维度的度量方式不同呢？

（如$Person = [height(cm), weight(kg)]$，$L_p$范数没有意义）

-> **Z-Score变换**（见[第二章的笔记](https://abigailhua.github.io/2020/03/29/Feature-Engineering/#z-score%E5%BD%92%E4%B8%80%E5%8C%96)）



#### 再如果两个样本都完全不相似呢？

（如图片和文本）

### **度量学习** (Metric Learning)

idea: 将位于两个不同空间的样本映射到一个共同的空间里，即$d(x, y) \Rightarrow d(Px, Py)$

<img src="/img/in-post/post-distance-metric/metric-learning.png" width="500">

马氏距离 (Mahalanobis Distance)可以实现映射，它是欧氏距离的推广：
$$
d(Px, Py) = \parallel Px - Py \parallel = \sqrt{(x^T-y^T)P^TP(x-y)} = \sqrt{(x^T-y^T)M(x-y)}
$$


### 搬土距离 (EMD, Earth Mover's Distance)



# 两个分布间距离的度量方法