# Transformer 残差流中的特权基

- 英文标题：Privileged bases in the transformer residual stream
- 发布日期：2023-03-16
- 官网栏目：可解释性
- 原文：[Anthropic 页面](https://www.anthropic.com/research/privileged-bases-in-the-transformer-residual-stream)
- 官网署名：Anthropic
- 归档范围：官网介绍页。另站发布的论文或报告请使用下方链接。
- 官网推荐阅读：[阅读论文](https://transformer-circuits.pub/2023/privileged-basis/index.html)

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

### 摘要

我们的 Transformer 架构的数学理论表明，残差流中的各个坐标应该没有特殊意义（也就是说，基本方向在某种意义上应该是“任意的”，并且不会比随机方向更可能编码信息）。最近的工作表明，这种观察在实践中是错误的。我们研究了这一现象，并初步得出结论：Adam 优化器中的每维度标准化器是造成这种影响的原因。  

我们探索 Transformer 中基础依赖性的另外两个明显来源：层归一化和有限精度浮点计算。我们自信地将这些排除为观察到的基础对齐的来源。
