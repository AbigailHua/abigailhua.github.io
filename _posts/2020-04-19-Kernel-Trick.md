---
layout:     post
title:      "Kernel Trick"
subtitle:   "数据科学基础课笔记（二）"
date:       2020-04-19 15:30:00
author:     "AH"
header-img: "img/post-bg-2015.jpg"
tags:
    - Data Science
---

## 引言

上一章笔记介绍了在训练样本数量固定时，高维很可能导致过拟合。然而维数过低也会有欠拟合的问题。

<img src="\img\in-post\post-kernel-trick\performance.png" width="550">

例如在下图中，无法找到一个线性分类器将正负样本较好地分开。

<img src="\img\in-post\post-kernel-trick\classify.jpg" width="350">

但是如果我们能将样本投影到高维空间，数据也许就变得可分了

<img src="\img\in-post\post-kernel-trick\project.png" width="600">

这里我想到大一写笑脸识别时第一次接触SVM看到的一段介绍，大意是，黑白的围棋子散乱在桌面上，混在一起难以分开。大师在桌上猛地一拍，围棋子悬浮在空中，白的飘在上层，黑的沉在下层，大师挥手横着一劈就把两种围棋子分开了。

在这个例子里，围棋子本来分布在二维平面中（桌面），大师一拍桌，悬浮在空中，就是投射到了三维空间里。最后挥手的那一劈，即是找到了一个分类超平面。

## SVM

在介绍核方法前，首先介绍最简单的SVM。

先给出SVM的primal形式：

$$
\begin{aligned}
&\min_{w,b} \frac{1}{2} \parallel w \parallel ^2 \\
&s.t. y_i(w^Tx_i+b) \geq 1, \forall i.
\end{aligned}
$$

这被称作是hard margin，因为在primal形式中，对于所有样本，$y_i(w^Tx_i+b)$，或者说margin，必须大于1

primal形式也可以是soft margin：

$$
\begin{aligned}
\min_{w,b}\quad&\frac{1}{2} \parallel w \parallel ^2 + C \sum_i \xi_i \\
s.t.\quad &y_i(w^Tx_i+b) \geq 1-\xi_i, \forall i, \\
&\xi_i \geq 0, \forall i.
\end{aligned}
$$

这里对margin增加了松弛条件，可以允许某些样本出格一点。但这个松弛当然不是白加的，我们仍然希望这个松弛不太大，所以要添加到损失函数里。

然而这样仍然无法解决之前的问题，我们还是找不到一个线性分类平面把正负样本分开。为了解决这个问题，我们需要使用核方法。

## Kernel SVM

### Primal Form of Kernel SVM

把之前soft margin中的$x_i$换成$\phi(x_i)$，就是Kernel SVM的primal形式了

$$
\begin{aligned}
\min_{w,b}\quad&\frac{1}{2} \parallel w \parallel ^2 + C \sum_i \xi_i \\
s.t.\quad &y_i(w^T\phi(x_i)+b) \geq 1-\xi_i, \forall i, \\
&\xi_i \geq 0, \forall i.
\end{aligned}
$$

### $\phi(x_i)$与核函数

$\phi(x)$函数的作用，正如之前引言中提到的，能够将数据映射到高维空间中，从而可以用一个超平面轻松分开正负样本。

下面是一个能将样本提升到有限维度的$\phi(x)$函数：

$$
\phi(x) = \phi ([x_1, x_2]) = [1, x_1, x_2, x_1x_2, x_1^2, x_2^2]
$$

其中$1$是零阶项，$x_1, x_2$是一阶项，$x_1x_2, x_1^2, x_2^2$则是二阶项。

我们也可以把样本提升到无限维度，但这样$\phi(x)$的表示就比较困难，我们通过计算核函数$K(x, y) = \phi(x)^T \phi(y)$作为替代。以下是一些常见的核函数：

$$
\begin{aligned}
&\mbox{Linear kernel: } K(x, y) = x^T y \\
&\mbox{Polynomial kernel: } K(x, y) = (x^Ty+c)^d \\
&\mbox{Gaussian kernel: } K(x, y) = exp(-\frac{\parallel x-y \parallel^2}{2\sigma^2}) \\
&\mbox{Sigmoid kernel: } K(x, y) = tanh(\alpha x^Ty+c) \\
&\mbox{Inverse multi-quadratic kernel: } K(x, y) = \frac{1}{\sqrt{\parallel x-y \parallel} 2\sigma^2 + c^2} \\
&\mbox{...} \\
\end{aligned}
$$

### Dual Form

在这节中，我们将从primal形式推导出dual形式。

Lagrangian multiplier：

$$
\mathcal{L}_{w, b, \xi_i, \alpha_i, \beta_i} = \frac{1}{2} \parallel w \parallel^2 + C\sum_i \xi_i - \sum_i \alpha_i (y_i(w^T\phi(x_i)+b)-1+\xi_i) - \sum_i \beta_i \xi_i
$$

对$w, b, \xi_i$进行偏微分：

$$
\begin{aligned}
\frac{\partial \mathcal{L}}{\partial w} = w - \sum_i \alpha_i y_i \phi(x_i) = 0 \quad &\Rightarrow \quad w = \sum_i \alpha_i y_i \phi(x_i) \\
\frac{\partial \mathcal{L}}{\partial b} = - \sum_i \alpha_i y_i = 0 \quad &\Rightarrow \quad \sum_i \alpha_i y_i = 0 \\
\frac{\partial \mathcal{L}}{\partial \xi_i} = C - \alpha_i - \beta_i = 0 \quad &\Rightarrow \quad \alpha_i + \beta_i = C
\end{aligned}
$$

将以上三个式子代入Lagrangian，可以得到Kernel SVM的dual形式（注意原问题是min，拉格朗日对偶问题是max，对式子再取相反数转化成min）

$$
\begin{aligned}
\min_{\alpha_i}\quad&\frac{1}{2} \sum_i \sum_j \alpha_i\alpha_jy_iy_jK(x_i, x_j) - \sum_i \alpha_i \\
s.t.\quad &\sum_i \alpha_iy_i=0, \\
&0 \leq \alpha_i \leq C, \forall i.
\end{aligned}
$$

### 将核方法迁移到其它机器学习算法

在神经网络之前，炼丹主要就体现在核方法。老师说当年顶会很多文章就是把以前的算法加个核，反正正确率提升很多就可以了...

这里举一个把kernel trick用在Logistic Regression里的例子：

$$
\min_{w, b} \sum_j \frac{1}{1+exp(y_j (w^T\phi(x_j)+b))}
$$

为了写成核函数的形式，可以将$w$表示成$\phi(x_i)$的线性组合（$w=\sum_i \alpha_i \phi(x_i)$）：

$$
\min_{w, b} \sum_j \frac{1}{1+exp(y_j (\sum_i \alpha_i K(x_i, x_j)+b))}
$$

### 核函数的性质

核函数一般满足以下性质：

* 对称性：

$$
K(x, y) = K(y, x)
$$

* Cauchy-Schwarz不等式性质：

$$
\begin{aligned}
&K(x, y)^2 \leq K(x, x) K(y, y) \\
&K(x, y)^2 = (x^Ty)^2 \leq \parallel x \parallel^2 \parallel y \parallel^2 = (x^Tx)(y^Ty) = K(x, x) K(y, y)
\end{aligned}
$$

* 闭包性质：

$$
\begin{aligned}
&K(x, y) = c \cdot K_1(x, y) \\
&K(x, y) = c + K_1(x, y) \\
&K(x, y) = K_1(x, y) + K_2(x, y) \\
&K(x, y) = K_1(x, y) \cdot K_2(x, y) \\
\end{aligned}
$$

