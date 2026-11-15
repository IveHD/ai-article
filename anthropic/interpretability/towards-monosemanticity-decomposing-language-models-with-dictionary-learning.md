# 迈向单义性：用字典学习分解语言模型

- 英文标题：Towards monosemanticity: Decomposing language models with dictionary learning
- 发布日期：2023-10-05
- 官网栏目：可解释性
- 原文：[Anthropic 页面](https://www.anthropic.com/research/towards-monosemanticity-decomposing-language-models-with-dictionary-learning)
- 官网署名：Anthropic
- 归档范围：官网介绍页。另站发布的论文或报告请使用下方链接。
- 官网推荐阅读：[阅读论文](https://transformer-circuits.pub/2023/monosemantic-features/index.html)

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

### 摘要

在我们最新的论文中， [*迈向单义性：通过字典学习分解语言模型*](https://transformer-circuits.pub/2023/monosemantic-features)，我们概述了存在比单个神经元更好的分析单元的证据，并且我们已经构建了可以让我们在小型变压器模型中找到这些单元的机制。这些单位称为特征，对应于神经元激活的模式（线性组合）。这提供了一条将复杂的神经网络分解为我们可以理解的部分的途径，并建立在之前解释神经科学、机器学习和统计学中高维系统的努力的基础上。在 Transformer 语言模型中，我们将包含 512 个神经元的层分解为 4000 多个特征，这些特征分别表示 DNA 序列、法律语言、HTTP 请求、希伯来语文本、营养声明等等。当单独观察单个神经元的激活时，大多数模型属性是不可见的。
