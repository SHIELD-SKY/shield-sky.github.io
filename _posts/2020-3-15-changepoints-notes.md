## Optimal Detection of Changepoints with a Linear Computational Cost 阅读笔记

###A brief history

At the time of writing(2012):

**binary segment(BS)** proposed by Scott and Knott(1974)  is most widely used change point search method.  $\mathcal{O} (n log n)$

Serveral exact search method are base on dynamic programming.

**segment neighborhood(SN)** method proposed by Auger and Lawrence (1989) is $\mathcal{O}(Qn^2)$ .  Q 是希望搜索的变化点的最大个数。变化点随着n线性增长，计算复杂度会是cubic

An alternative **dynamic programming** algorithm is provided by **optimal partitioning (OP)** approach of Jackson et al.(2005) $\mathcal{O}(n^2)$

One commonly used approach to identify multiple change points is to minimize
$$
\sum_{i=1}^{m+1}[\mathcal{C}(y_{(\uptau)})]
$$


### 本文方法

We present a new approach to search for changepoints, which is **exact** and, **under mild conditions**, has a computational cost that is **linear** in the number of data points: **the pruned exact linear time (PELT)** method.

This approach is based on the algorithm of Jackson et al. (2005), **but involves a pruning step** within the dynamic program. This pruning reduces the computational cost of the method, but does not affect the exactness of the resulting segmentation. 

