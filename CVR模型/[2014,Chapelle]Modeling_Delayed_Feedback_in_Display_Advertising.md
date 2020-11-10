# Modeling Delayed Feedback in Display Advertising

作者：Olivier Chapelle<br>
公司：Criteo Labs<br>
时间：2014 KDD<br>

## 概述
本文介绍了CVR预估模型中，处理样本标签时间延迟的方法。<br>

CVR预估模型相对于CTR预估模型有两个难点：<br>
（1）CVR是用户点击后购买的概率，而点击样本相对于展现样本要稀疏的多，且存在训练和预估样本分布不一致的问题，因此CVR模型更加难以学习；<br>
（2）购买行为可能发生在点击行为之后的较长一段时间内，因此训练时不得不面对大量标签缺失的样本；<br>

本文主要是针对第二个问题进行改进的。

## CVR模型相关的基本概念
### 常见计费方式
- CPV(Cost Per View)：按展示计费
- CPM(Cost Per Mille)：按千次展现计费
- CPC(Cost Per Click)：按点击收费
- CPA(Cost Per Action)：按成果数计费（例如按转化计费）

### 常用术语
- CTR(Click Through Rate)：点击率 = 点击 / 展现
- CVR(Click Value Rate)：转化率 = 转化率 / 点击
- ROI(Return Of Interest)：投资回报率
- GMV(Gross Merchandise Volumn)：网站成交金额
- PV(Page View)：页面浏览量
- UV(Unique Visitor)：独立访客

### 归因
归因（Attribution）是确定某次转化激发原因的一种机制，分为以下两种形式：

- Post-view Attribution：将转化归因到一个或多个展现
- Post-click Attribution：将转化归因到一个点击

在展示广告场景中，一般Post-click Attribution更加可靠。

## 延迟建模思路

由于转换行为可能在点击后的较长一段时间内发生，因此当某一个点击未发生相对应的转化时，无法确定该点击样本是正样本还是负样本。对于这个问题，有以下思路和解决方案：

1. 时间窗口

一个比较简单的想法是设定一个时间窗，丢弃点击后时间还未到达时间窗的样本。该方法会有如下问题：

- 如果window设置太小，会有很多转化被误判为负样本；
- 如果window设置太大，近期的样本无法参与训练，相当于模型始终处于延迟状态；

对于该问题的解决方案是：将已转化的样本标记为positive，将未转化的样本标记为unlabeled。

1. 对unlabeled样本出现概率进行建模

一般对于unlabeled数据的处理方式假设label丢失概率为常数，而延迟场景显然不满足这个假设，因为点击发生时间越近，样本的label缺失概率越大。

对于该问题的解决方案是：对expected delay进行建模。

1. 处理consored delay

对于expected delay可以使用survival time analysis的方法，类似于预估病人从患病到死亡的时间。而会存在一部分病人在观察结束时尚未死亡，对于这部分病人我们无法得知他的具体survival time，称这种现象为censored。类比于转化率预估场景，部分点击最终会转化，但尚未发生，因此也存在censored问题。

对于该问题的解决方案是：我们既要预估是否转化，也需要预估延迟时间，本项目采用两个模型联合训练的方式。

### 模型公式推导





