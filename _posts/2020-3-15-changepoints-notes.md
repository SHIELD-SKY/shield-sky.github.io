---
layout:     post
title:      "Optimal Detection of Changepoints with a Linear Computational Cost 阅读笔记"
subtitle:   "note of PELT"
date:       2020-03-17 16:59:30
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:

- PELT
- 动态规划
- Changepoints 



---

One commonly used approach to identify multiple change points is to minimize


$$
\sum_{i=1}^{m+1}[\mathcal{C}(y_{(\tau_{i-1}+1):\tau_{i}})] + \beta f(m)
$$


Here $$\mathcal{C}$$ is a cost function for a segment and $$\beta f(m)$$ is a penalty to guard against overfitting.

### 几种cost function：

   1. twice th negative log-likelihood

      

   2. quadratic loss

      

   3. cumulative sums

      

   4. based on both the segment log-likelihood and the length of the segment

### penalty

1. most common choice : $$\beta f(m) = \beta m$$

   * Akaike’s information criterion (AIC; Akaike 1974) (β = 2p)

   * Schwarz information criterion (SIC, also known as BIC; Schwarz 1978) (β = p log n, where *p* is the number of additional parameters introduced by adding a changepoint).

     

### A brief history

At the time of writing(2012):

**binary segment(BS)** proposed by Scott and Knott(1974)  is most widely used change point search method.  $$\mathcal{O} (n log n)$$

本质上就是寻找满足以下cost function的$$\tau$$


$$
\mathcal{C}(y_{1:\tau}) + \mathcal{C}({y_{(\tau+1):n}})+\beta < \mathcal{C}(y_{1:n})
$$


找的一个点然后二分，再在子段上执行相同操作，直到找不到changepoints为止。 

优点：快

缺点： 无法保证global minimum of cost function

#### Exact Methods

Serveral exact search method are base on dynamic programming.

**segment neighborhood(SN)** method proposed by Auger and Lawrence (1989) is $$\mathcal{O}(Qn^2)$$ .  Q 是希望搜索的变化点的最大个数。变化点随着n线性增长，计算复杂度会是cubic

***The OP Method***.  **An alternative dynamic programming** algorithm is provided by **optimal partitioning (OP)** approach of Jackson et al.(2005) $$\mathcal{O}(n^2)$$

懒得打公式了，见paper公式(3),及后边推导，及Algorithm 1

### 要想理解pelt 首先需要深刻理解OP method！

### 要想理解OP method 首先需要理解dynamic programming！

不熟悉的话，可以去翻阅《算法导论》，顺便提一句，根据算导第三版， 这里的programming 指的是一种表格法，而非编程。



总数据是$$y_{1:n}$$， 对于数据$$y_{1:s}$$, OP method 考虑当前点s下，上一个变化点是谁。

OP method 将上一个变化点 之前的cost 与 变化点之后的cost 联系起来。原文中这样描述：Following Jackson et al. (2005), the OP method begins by first conditioning on the last point of change. It then relates the optimal value of the cost function to the cost for the optimal partition of the data prior to the last changepoint plus the cost for the segment from the last changepoint to the end of the data. 

那么如何考察对于s下，上一个变化点是谁呢？

根据文章中，公式（3）后的推导，可得出递归方程。可知，想得到s下满足cost $$F(S)$$的last point of change， 我们可以先解决子问题$$F(t)$$.

这正是动态规划的思想。**我们这里假设$$F(t)$$情况下，变化点序列已知**，为求$$F(s)$$ 我们需要不断针对已知的变化点序列中的待选变化点计算相应的 cost（**注意这里就是PELT以后要优化的地方**）。正如前边提到，cost 是计算待选变化点之前的cost 1 加上。变化点至数据尾端，也就是s的cost2.

通过不断的筛选，我们最终会计算到$$F(n)$$，此时可知最优的cost值。

另外通过额外数组的记录，我们最终可以构造出最终的变化点序列。这里的技术细节涉及到动态规划的构造解。可以结合论文及其他人的[代码](https://github.com/ruipgil/changepy/blob/master/changepy/pelt.py)理解



### 本文方法

We present a new approach to search for changepoints, which is **exact** and, **under mild conditions**, has a computational cost that is **linear** in the number of data points: **the pruned exact linear time (PELT)** method.

This approach is based on the algorithm of Jackson et al. (2005), **but involves a pruning step** within the dynamic program. This pruning reduces the computational cost of the method, but does not affect the exactness of the resulting segmentation. 

PELT 在OP的基础之上加入了 **pruning**

重点理解Theorem 3.1. 

作者先假设：**we assume there exists a constant *K* such that for all t <s <T,**


$$
\mathcal{C}(y_{(\tau+1):s}) + \mathcal{C}(y_{(s+1):T}) +K \leqslant \mathcal{C}(y_{(t+1):T})
$$


Then , if

 
$$
F(t) + \mathcal{C}(y_{(t+1):s}) + K \geqslant F(S)
$$


Holds, at a future time T > s, *t* can never be the optimal last changepoint prior to *T*.

在假设的前提下，结合Section 5 of the online supplementary materials.

我们可以看到，满足原文公式（5）的t，对于s 以后的T，不可能是最优的 last changepoint。

最终如Algrorithm 2 所示，就是加了第四行伪代码，用于剪枝。

接下来的 计算复杂度分析，暂时没看懂。

### 感想

1. 传统经典算法就是经典，一开始忘记了动态规划的思想，论文代码一点都没通。疫情在家，翻了翻算导，重新拾起动态规划，发现其实OP method的核心idea 就是动态规划。每一个经典算法之上成果真是枝繁叶茂。

2. 再次强调一下英语阅读和语文能力。在学校读这论文时好像正是期末考试，也是心烦意乱，没能领略作者的idea。 感谢这疫情超长假期，在家读完了*word power made easy* ，感觉再回来读论文，信息的接收理解比以前流畅了不少。

