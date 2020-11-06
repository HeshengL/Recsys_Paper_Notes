## Modeling Delayed Feedback in Display Advertising

作者：Olivier Chapelle
公司：Criteo Labs
时间：2014 KDD

### 一、概述
本文介绍了CVR预估模型中，处理样本标签时间延迟的方法。<br>

CVR预估模型相对于CTR预估模型有两个难点：
（1）CVR是用户点击后购买的概率，而点击样本相对于展现样本要稀疏的多，且存在训练和预估样本分布不一致的问题，因此CVR模型更加难以学习；<br>
（2）购买行为可能发生在点击行为之后的较长一段时间内，因此训练时不得不面对大量标签缺失的样本；<br>
本文主要是针对第二个问题进行改进的。

### 二、CVR模型相关的基本概念


