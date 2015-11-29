--- 
layout: post
title: Gradient descent
tags: 
- bigdata
- business
status: publish
type: post
published: false
---

数据

    1 1 7.97
    2 1 10.2
    3 1 14.2
    4 1 16.0
    5 1 21.2

脚本：

    A <- x[,1:2]
    b <- x[,3]
    C <- t(A) %*% A
    solve(C, t(A)%*%b)

必须将矩阵转化为方阵才能进行solve，结果同
    
    lm(b ~ A)

		A <- read.csv(textConnection("
		3 2 4 0 0 0 0
		1 0 1 0 0 0 0
		0 1 2 0 0 0 0
		0 0 0 1 1 1 1
		0 0 0 1 0 1 1
		0 0 0 3 2 2 3
		"), header = FALSE, sep = ' ')


北京京东世纪贸易有限公司

http://www.quuxlabs.com/blog/2010/09/matrix-factorization-a-simple-tutorial-and-implementation-in-python/

# 矩阵分解的数学原理

首先约定一下符号，对于用户（users）的集合 $$U$$，以及商品的集合 $$D$$，用$$R$$ 来表示用户商品信息的共现（$$U \times D $$）矩阵。我们现在想找出 K 个潜在的特征，即：找到两个新矩阵P（$$U \times K$$），Q（$$D \times K$$），使得：

$R = P \times Q^T = \hat{R}$

这时，P包含了所有的用户（U）的相关信息（特征），而Q则包含了商品的相关信息（特征）。那如何找到这两个矩阵呢？

其中的一种方法就是梯度下降（gradient descent）：首先先给P、Q一些初始值，然后计算R和$$P \times Q$$的差异，接着通过迭代最小化二者的差异。这个差异我们一般用如下的方式表示：

$e_{ij}^2 = (r_{ij} - \hat{r}_{ij})^2 = (r_{ij} - \sum_{k=1}^K p_{ik} q_{kj})^2$ 

对于上式，我们必须找到一个方向来优化$$p_{ik},q_{kj}$$。换句话说，我们需要知道当前值的梯度下降方向：

$\frac{\partial}{\partial p_{ik} e_{ij}^2} = -2(r_{ij} - \hat{r}_{ij})(q_{kj}) = -2 e_{ij}q_{kj}$
 
$\frac{\partial}{\partial q_{ik} e_{ij}^2} = -2(r_{ij} - \hat{r}_{ij})(p_{ik}) = -2 e_{ij}p_{ik}$

既然以及找到梯度，那则有

$p_{ik}^' = p_{ik} + 2\alpha e_{ij} q_{kj}$

$q_{kj}^' = q_{kj} + 2\alpha e_{ij} p_{ik}$

这里$$\alpha$$ 是一个常数，决定梯度的步长，为了避免越过局部最优值，所以$$\alpha$$一般都是一个很小的数，比如0.0002。

另外一个问题有来了：

> 如果我们求得的P和Q的乘积同R完全一致，那么未观测的值（表示为零的行为），依旧是零。

这里需要澄清一下：`我们只对原始数据不为零的元素求解二者差异，而不是全部的元素。`


# 规整化 Regularization

为了避免过拟合，我们一般会引入Regularization来作为惩罚项，一般是引入一个$$\beta$$来修改误差的平方：


$e_{ij}^2 = (r_{ij} - \sum_{k=1}^K p_{ik} q_{kj})^2 + \frac{\beta}{2} \sum_{k=1}^K(||P||^2 + ||Q||^2)$

$$\beta$$用来控制用户特征和商品特征的程度（magnitudes），保证P、Q对R的近似，但不会出现太大的数值。

这样梯度下降的规则就变成了如下：

$p_{ik}^' = p_{ik} + 2\alpha e_{ij} q_{kj} - \beta p_{ik}$

$q_{kj}^' = q_{kj} + 2\alpha e_{ij} p_{ik} - \beta q_{kj}$




