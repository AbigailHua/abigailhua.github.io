---
layout:     post
title:      "Basic Machine Learning"
subtitle:   "人工智能课笔记（一）"
date:       2020-04-18 10:38:00
author:     "AH"
header-img: "img/post-bg-2015.jpg"
tags:
    - AI
---

注：这是上学期上人工智能课时整理的笔记，现在po到博客上来，方便浏览。

当时课程参考斯坦福（后来换成了伯克利）的人工智能课，教材理论上应该是*Artificial Intelligence: A Modern Approach*，但是某马姓老师上课质量和在我们反馈后采取的态度实在是不敢恭维，此处暂且按下不表了。

笔记总共写了四篇：

1. [Basic Machine Learning](https://abigailhua.github.io/2019/11/05/Basic-Machine-Learning/)
2. [MDP](https://abigailhua.github.io/2019/11/05/MDP/)
3. [Search](https://abigailhua.github.io/2019/11/05/Search/)
4. [Reinforcement Learning](https://abigailhua.github.io/2019/11/05/RL/)

# Machine Learning

### Linear Predictors

Feature vector:

$$
\phi(x) = [\phi_1(x), ..., \phi_d(x)] \in R^d
$$

Weight vector: 

$$
w(x) = [w_1(x), ..., w_d(x)] \in R^d
$$

Score (how confident we are in predicting +1):

$$
w \cdot \phi(x) = \sum_{j=1}^d w_j \phi_j(x)
$$

(Binary) linear classifier: 

$$
f_w(x)=sign(w \cdot \phi(x))
$$

### Loss Minimization

Margin (how correct we are):

$$
(w \cdot \phi(x))y = score \cdot y
$$

Note: a binary classifier errors when margin is negative

#### Binary Classification

Zero-one loss:

$$
Loss_{0-1}(x, y, w) = 1[f_w(x)!=y] = 1[(w \cdot \phi(x))y \leq 0] = 1[margin \leq 0]
$$

Hinge loss:

$$
Loss_{hinge}(x, y, w) = \max \left\{1-(w\cdot \phi(x))y, 0\right\} = \max \left\{1-margin, 0\right\}
$$

Logistic loss:

$$
Loss_{logistic}(x, y, w) = \log(1+e^{-(w \cdot \phi(x))y}) = \log(1+e^{-margin})
$$

<img src="\img\in-post\post-basic-machine-learning\1572593570766.png" width="400">

- Hinge loss upper bounds 0-1 loss, has non-trivial gradient.
- Logistic regression tries to increase margin even when it already exceeds 1.

#### Linear Regression

<img src="\img\in-post\post-basic-machine-learning\1572591395247.png" width="500">

Residual (the amount by which prediction $f_w(x)$ overshoots the target $y$:

$$
w \cdot \phi(x) - y
$$

Square loss (min: mean $y$):

$$
Loss_{squared}(x, y, w) = (f_w(x)-y)^2 = (w \cdot \phi(x) - y)^2 = residual^2
$$

Absolute deviation loss (min: median $y$):

$$
Loss_{absdev}(x, y, w) = |f_w(x)-y| =|w \cdot \phi(x)-y| = |residual|
$$

#### Loss Minimization Framework

$$
TrainLoss(w) = \frac{1}{|D_{train}|} \sum_{(x, y) \in D_{train}} Loss(x, y, w)
$$

$$
\min_{w \in R^d} TrainLoss(w)
$$

### Stochastic Gradient Descent (SGD)

#### Gradient Descent

<img src="\img\in-post\post-basic-machine-learning\1572592764984.png" width="350">

Objective function (train loss using square loss):

$$
TrainLoss(w) = \frac{1}{D_{train}} \sum_{(x, y)\in D_{train}} (w \cdot \phi(x) - y)^2
$$

Gradient:

$$
\nabla_w TrainLoss(w) = \frac{1}{D_{train}} \sum_{(x, y)\in D_{train}} 2(w \cdot \phi(x) - y) \phi(x)
$$

Problem: each iteration requires going over all training examples (long training time)

#### Stochastic Gradient Descent

<img src="\img\in-post\post-basic-machine-learning\1572598750127.png" width="400">

where $\eta$ is called **step size**

Strategies:

- Constant: $\eta = 0.1$
- Decreasing: $\eta = \frac{1}{\sqrt{\mbox{#updates made so far}}}$

### Summary I

Score: 

$$
w \cdot \phi(x)
$$

<img src="\img\in-post\post-basic-machine-learning\1572593777621.png" width="500">

### Features

#### Feature templates

e.g. spam:

- length greater than __ -> 10/20/30/...
- last 3 characters equals __ -> aaa/abc/xyz/...
- contains character __ -> @/#/&/...
- pixel intensity of position __ -> 0.95/0.8/0.3/...

#### Feature Vector Representations

Array representation (for dense features):

$$
[0.85, 0, 0, 0, 0, 1, 0, 0, 0]
$$

Map representation (for sparse features):

$$
{"fracOfAlpha": 0.85, "contains\_@": 1}
$$

#### Hypothesis Class

$$
F = \{f_w: w \in R^d\}
$$

#### Beyond Linear Functions

Linear classifiers can produce non-linear decision boundaries.

Quadratic functions:

$$
\phi(x) = [x, x^2]
$$

Piecewise constant function:

$$
\phi(x) = [1[0<x\leq1], 1[1<x<2], ...]
$$

### Neural Networks

Logistic function $(-\infty, \infty) \rightarrow [0, 1]$:

$$
\sigma(z) = (1+e^{-z})^{-1}\\
\nabla \sigma(z) = \sigma(z) (1-\sigma(z))
$$

Neural network:

<img src="\img\in-post\post-basic-machine-learning\1572700675975.png" width="500">

Intermediate hidden units:

$$
h_j = \sigma(v_j \cdot \phi(x)), \sigma(z) = (1+e^{-z})^{-1}
$$

Output:

$$
score = w \cdot h
$$

Difference with machine learning: features is specified in ML, but learned in NN

### Backpropagation

Basic building blocks:

<img src="\img\in-post\post-basic-machine-learning\1572701568510.png" width="400">

### Nearest Neighbors

<img src="\img\in-post\post-basic-machine-learning\1572750701188.png" width="600">

### Summary II

<img src="\img\in-post\post-basic-machine-learning\1572750752156.png" width="550">

### Generalization

Goal: to minimize **error on unseen future examples**

#### Controlling Size of Hypothesis Class

##### Keeping the Dimensionality $d$ Small

* Manual feature (template) selection: only retain features that help
* Automatic feature selection

##### Keeping the Norm (Length) $|| w || $ Small

1. Regularization:

$$
\min_w TrainLoss(w) + \frac{\lambda}{2} ||w||^2
$$

<img src="\img\in-post\post-basic-machine-learning\1572751864341.png" width="400">

Note: SVM = hinge loss + regularization

2. Early stopping:

   make T smaller (if fewer updates, $||w||$ can't get too big)

#### Development Cycle

<img src="\img\in-post\post-basic-machine-learning\1572768314284.png" width="500">

##### Validation

<img src="\img\in-post\post-basic-machine-learning\1572768372890.png" width="600">

##### Hyperparameters

<img src="\img\in-post\post-basic-machine-learning\1572768195728.png" width="600">

### Unsupervised Learning

Motivation: Unlabeled data is much cheaper

Idea: discover latent structures within data automatically

#### Clustering

Put similar points in same cluster and dissimilar in different clusters.

<img src="\img\in-post\post-basic-machine-learning\1572769279539.png" width="400">

Objective function:

$$
Loss_{kmeans}(z, \mu) = \sum_{i=1}^n || \phi(x_i)-\mu_{z_i}||^2
$$

where $\mu_{z_i}$ is the centroid of cluster $k$

#### K-Means Algorithm

Objective:

$$
\min_z \min_{\mu} Loss_{kmeans}(z, \mu)
$$

<img src="\img\in-post\post-basic-machine-learning\1572770543977.png" width="400">

Goal: given centroids $\mu_1, \mu_2,..., \mu_K$ , assign each point to the best centroid.

<img src="\img\in-post\post-basic-machine-learning\1572770329185.png" width="400">

Goal: given cluster assignments $z_1, z_2, ..., z_n$ , find the best centroids
$\mu_1, \mu_2, ... ,\mu_K$.

<img src="\img\in-post\post-basic-machine-learning\1572770486330.png" width="400">

Problem: K-means may trap in local minima.

Solution: 

- Run multiple times from different random initializations.
- Initialize with a heuristic (K-means++)

