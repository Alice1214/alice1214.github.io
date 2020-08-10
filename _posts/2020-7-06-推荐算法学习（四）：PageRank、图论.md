---
published: true
title: 推荐算法学习（四）：PageRank、图论
category: 推荐算法
tags: 
  - 推荐系统
layout: post
---

在PageRank提出之前，搜索引擎面临着返回结果质量不高、容易被作弊等问题。如何找到优质的网页？首先需要匹配用户查找的内容，再按照权重排序输出给用户。1998年，Stanford大学博士生Larry Page和Sergey Brin创立了Google，受到论文影响力的启发，使用基于图论的PageRank模型计算海量网页的重要性，并按照重要性对网页进行排序。这一方法对Google现在数千亿的市值起到了不可磨灭的作用。

而PageRank在网页之外，也具有惊人的大范围的实用性，几乎涵盖所有领域。如社交网络领域，利用PageRank计算博主影响力；生物领域，通过PageRank研究基因、蛋白质，确定了七个与遗传有关的肿瘤基因；交通网络领域，用于预测城市的交通流量和人流动向；此外，在推荐系统中，将用户行为转化为图的形式，基于PageRank进行商品推荐。

PageRank的思想很简单，在互联网中如果一个网页被许多其他网页所链接，说明它普遍受到信任和依赖，因此到该页面的超链接可看作对其投了一票。一个页面的PageRank是由所有链向它的页面的重要性经过递归算法得到的。基于PageRank的思想，人们在推荐、社会网络分析、自然语言处理等领域推出了很多实用的解决方案，比如用于文本摘要的TextRank算法。接下来详细介绍一下PageRank及其相关算法。

## PageRank简化模型

* **出/入链**：链接出去/进来的链接
* **一个网页$u$的影响力定义为所有入链集合的页面的加权影响力之和**

<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（四）/0.png" alt="0" style="zoom:40%;" />

其中$B_u$为$u$的入链集合，对于$B_u$中的任意页面$v$，它能给$u$带来的影响力是其自身的影响力$PR(v)$除以$v$页面的出链数量，即页面$v$把影响力$PR(v)$平均分配给了它的出链。这样统计所有能给$u$带来链接的页面$v$，得到的总和即为$v$的影响力$PR(u)$.

<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（四）/1.png" alt="0" style="zoom:60%;" />

对于上面的图，其**转移矩阵**（统计网页对于其他网页的跳转概率）表示为：

<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（四）/2.png" alt="0" style="zoom:35%;" />

**PageRank计算过程**如下：

* Step1，假设A、B、C、D的初始影响力相同
* Step2，进行第一次转移之后，页面的影响力变为

<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（四）/3.png" alt="0" style="zoom:40%;" />** **<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（四）/4.png" alt="0" style="zoom:40%;" />

* Step3，进行n次迭代后，直到页面的影响力不再发生变化，也就是页面的影响力收敛=>最终的影响力

<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（四）/5.png" alt="0" style="zoom:45%;" />

在实际应用中，并非所有的节点都满足既有出度又有入度，因此PageRank简化模型可能会出现Rank Leak（等级泄露）或Rank Sink（等级沉没）的问题。**等级泄露**是指一个网页没有出链，就像是一个黑洞一样，吸收了别人的影响力而不释放，最终会导致其他网页的PR值为0；**等级沉没**是指一个网页只有出链，没有入链，一直迭代，会导致这个网页的PR值为0。为了解决这一问题，Larry Page提出了PageRank的随机浏览模型。

## PageRank随机浏览模型

用户在浏览网页过程中，并不都是按照跳转链接的方式来上网，假设还有一种可能是不论当前处于哪个页面，都有概率访问到其他任意的页面。根据这一假设场景，PageRank随机浏览模型引入**阻尼因子**d（通常取值为0.85），定义网页$u$的影响力为：

<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（四）/6.png" alt="0" style="zoom:27%;" />

## TextRank算法

TextRank算法

另外两种算法EdgeRank、PersonalRank



## 社区发现：LPA

图论的作用:节点影响力：PageRank;社区发现;最短路径：Dijkstra，Floyd；

一切事物都可看做节点



社区发现

重点LPA步骤

## 实例1：希拉里邮箱丑闻关键人物分析



* 根据节点影响力阈值筛选，可视化呈现



* PageRank工具：igraph：处理复杂网络问题，提供Python, R, C语言接口

性能强大，效率比NetworkX高

NetworkX：基于python的复杂网络库

对于Python使用者友好







## 实例2：使用TextRank对新闻进行关键词提取，及文章摘要输出

* **MarketBasket数据集**：Market_Basket_Optimisation.csv（该文件由超市购物订单数据构成）。

* **采用Apriori算法进行频繁项集及关联规则挖掘，并针对MarketBasket数据集进行词云展示，探索TOP10的商品有哪些。**

>github地址：<https://github.com/Alice1214/RS/tree/master/RS03>

