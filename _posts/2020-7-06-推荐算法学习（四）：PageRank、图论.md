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

PageRank不仅仅是一个算法，而是一种思想，TextRank算法、EdgeRank算法以及PersonalRank算法都是基于PageRank思想提出的。**TextRank**算法是一种用于文本的基于图的排序算法，可对文本进行关键词提取；**EdgeRank**算法是 Facebook 提出的对 fb 新鲜事排序的算法, 用于区别默认的按时间逆序的 timeline；**PersonalRank**算法是基于图的推荐算法，其将将用户行为转化为图模型，计算节点影响力，从而进行商品推荐。接下来介绍一下TextRank算法原理。

TextRank算法是根据词之间的共现关系构造网络，构造的网络中的边是无向有权边，权重为词共现的次数，基于PageRank原理计算节点影响力并进行排序，即可筛选出指定词性的关键词。其具体流程如下：

* Step1，进行分词和词性标注，将单词添加到图中
* Step2，出现在一个窗口中的词形成一条边
* Step3，基于PageRank原理进行迭代（20-30次）
* Step4，顶点（词）按照分数进行排序，可以筛选出指定词性的关键词

**TextRank不仅能提取关键词，还能生成摘要**。其流程是类似的，先将每个句子作为图中的节点，若两个句子相似，则节点之间存在一条无向有权边，这里句子的相似度等于同时出现在两个句子中的单词的个数除以句子中单词个数求对数之和，根据构造的图，即可计算句子（节点）的影响力，筛选出影响力大的句子从而生成摘要。

## 社区发现：LPA

图是一个很重要的工具，其节点可以代表任何事物，因此能满足我们很多需求。如上文的节点影响力计算，社区发现以及最短路径等，都是基于图模型来处理的。本节将主要介绍一下常用的社区发现算法LPA。

现实中存在着各种**复杂网络**，如社交网络，交通网络，交易网络，食物链等。这些复杂网络的一个普遍特征就是社区结构，整个网络是由许多个社区组成的。**社区**的特点是同一社区内的节点与节点之间的连接很紧密，而社区与社区之间的连接很稀疏。**社区发现**是一种**聚类**算法，具体是指在图中确定n个社区，使得各社区的顶点集合的交集覆盖图的所有顶点。其中，若任意两个社区的顶点集合的交际均为空，则为**非重叠社区**，否则为**重叠社区**。



社区发现

重点LPA步骤

## 相关工具

**PageRank工具：**

* **igraph**：性能强大，可处理复杂网络问题，提供了Python, R, C语言接口，其效率比NetworkX高

* **NetworkX**：基于python的复杂网络库，对于Python使用者友好

**TextRank工具：**

* **jieba**：可进行中文分词；可使用TextRank/TF-IDF提取关键词，TF-IDF效果好于TextRank，因为考虑了IDF的情况，而TextRank倾向使用频繁词
* **textrank4zh**：可提取关键词，还可生成摘要；要依赖分词，分词提取结果及TextRank结果与jieba存在差异

## 实例1：使用PageRank分析希拉里邮箱丑闻关键人物

* **数据集**：<https://github.com/cystanford/PageRank>（希拉里邮件丑闻），包含了9306封邮件和513个人名

1) Emails.csv：记录了所有公开邮件的内容，发送者和接受者的信息。

2) Persons.csv：统计了邮件中所有人物的姓名及对应的ID。

3) Aliases.csv：因为姓名存在别名的情况，为了将邮件中的人物进行统一，我们还需要用Aliases文件来查询别名和人物的对应关系。

* 将希拉里邮箱丑闻中人物看作节点，人物间发邮件关系看作边，设置边权重为发邮件的次数，如此生成的网络图如下<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（四）/7.png" alt="0" style="zoom:70%;" />

上图由于涉及人物众多、关系复杂，无法清晰的呈现邮箱丑闻中的关键人物。

* 计算每个节点（人物）的影响力（PR值），设置阈值，筛选出大于阈值的核心节点，绘制核心节点网络图如下<img src="https://raw.githubusercontent.com/Alice1214/alice1214.github.io/master/_posts/image/推荐算法（四）/8.png" alt="0" style="zoom:70%;" />

上图便能清晰明了的展现希拉里邮箱丑闻中关键人物及其之间的关系。

## 实例2：使用TextRank对新闻进行关键词提取，及文章摘要输出

* **MarketBasket数据集**：Market_Basket_Optimisation.csv（该文件由超市购物订单数据构成）

* **采用Apriori算法进行频繁项集及关联规则挖掘，并针对MarketBasket数据集进行词云展示，探索TOP10的商品有哪些。**

>github地址：<https://github.com/Alice1214/RS/tree/master/RS03>

