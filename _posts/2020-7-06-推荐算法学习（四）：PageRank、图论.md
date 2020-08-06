---
published: true
title: 推荐算法学习（四）：PageRank、图论
category: 推荐算法
tags: 
  - 推荐系统
layout: post
---

在PageRank提出之前，搜索引擎面临着返回结果质量不高、容易被作弊等问题。如何找到幼稚的网页？首先需要匹配用户查找的内容，再按照权重排序输出给用户。1998年，Stanford大学博士生Larry Page和Sergey Brin创立了Google，受到论文影响力的启发，使用基于图论的PageRank模型计算海量网页的重要性，并按照重要性对网页进行排序。



PageRank是什么 ，节点影响力

发展

应用、与推荐系统的关系

人们对PageRank进行个各种改动，基于相关算法在推荐、社会网络分析、自然语言处理等领域推出了很多实用的解决方案，比如用于文本摘要的TextRank算法。PageRank算法是怎么来的呢？怎么计算？

## PageRank简化模型

* **支持度**：指的是某个商品组合出现的次数与总次数之间的比例

* **置信度**：指的是当你购买了商品A，会有多大的概率购买商品B（条件概念）

* **提升度**：商品A的出现，对商品B的出现概率提升的程度，提升度(A→B)=置信度(A→B)/支持度(B)

## PageRank随机浏览模型

**频繁项集**：支持度大于等于最小支持度(Min Support)阈值的项集

**非频繁项集**：支持度小于最小支持度的项集

**Apriori算法流程**：

* Step1，K=1，计算K项集的支持度；

* Step2，筛选掉小于最小支持度的项集；

* Step3，如果项集为空，则对应K-1项集的结果为最终结果；否则K=K+1，重复1-3步。


下表中用ID来代表商品，ID1-6分别代表牛奶、面包、尿布、可乐、啤酒、鸡蛋。<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（三）/0.png" alt="0" style="zoom:80%;" />

利用Apriori算法查找频繁项集(frequent itemset)的过程如下：



## TextRank算法

TextRank算法

另外两种算法EdgeRank、PersonalRank



## 社区发现：LPA

图论的作用:节点影响力：PageRank;社区发现;最短路径：Dijkstra，Floyd；

一切事物都可看做节点



社区发现

重点LPA步骤

## 实例1：希拉里邮箱丑闻关键人物分析



根据节点影响力阈值筛选，可视化呈现

PageRank工具







## 实例2：使用TextRank对新闻进行关键词提取，及文章摘要输出

* **MarketBasket数据集**：Market_Basket_Optimisation.csv（该文件由超市购物订单数据构成）。

* **采用Apriori算法进行频繁项集及关联规则挖掘，并针对MarketBasket数据集进行词云展示，探索TOP10的商品有哪些。**

>github地址：<https://github.com/Alice1214/RS/tree/master/RS03>

