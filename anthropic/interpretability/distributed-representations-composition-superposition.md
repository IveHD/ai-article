# 分布式表征：组合与叠加

- 英文标题：Distributed representations: Composition & superposition
- 发布日期：2023-05-04
- 官网栏目：可解释性
- 原文：[Anthropic 页面](https://www.anthropic.com/research/distributed-representations-composition-superposition)
- 官网署名：Anthropic
- 官网推荐阅读：[阅读论文](https://transformer-circuits.pub/2023/superposition-composition/index.html)

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

### 摘要

分布式表示是神经科学和人工智能联结主义方法中的经典思想。我们经常被问到我们的叠加工作与它有何关系。自从发布我们的 [原纸](https://transformer-circuits.pub/2022/toy_model/index.html) 在叠加上，我们有更多时间思考主题之间的关系并与人们讨论，并希望扩展我们的 [先前的讨论](https://transformer-circuits.pub/2022/toy_model/index.html#related-codes) 在相关工作部分并分享一些想法。 （我们非常关心分布式表示的叠加和结构，因为将表示分解为独立的组件 [是逃离维数诅咒所必需的](https://transformer-circuits.pub/2022/mech-interp-essay/index.html) 并理解神经网络。）  

在我们看来，“分布式表示”可能被理解为包含两种不同的想法，我们将其称为“组合”和“叠加”。1 分布式表示的这两种不同概念在泛化以及可以从它们线性计算哪些函数方面具有非常不同的属性。虽然表示可以同时使用两者，但需要进行权衡，使它们从根本上处于紧张状态！2  

为了使这一点具体化，我们将考虑神经元表示不同颜色形状的几种可能方式。这些可爱的例子是借用的 [索普 (1989)](https://persee.fr/doc/intel_0769-4113_1989_num_8_2_873)，他创建它们是为了演示神经科学中“本地代码”和“分布式代码”概念之间的各种可能性。索普提供了四个示例代码——“本地”、“半本地”、“半分布式”和“高分布式”。传统上，这些可能被视为介于“本地”和“分布式”之间。我们将再次考虑这些例子，并提供另一种观点，其中例子在叠加和合成的两个不同维度上有所不同。  

继索普之后，本笔记将重点关注神经元具有二元激活的示例。这大大简化了可能性的空间，但它仍然是一个足够丰富的空间来提出有趣的问题
