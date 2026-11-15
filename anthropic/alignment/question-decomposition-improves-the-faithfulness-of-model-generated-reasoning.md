# 拆解问题能提高模型生成推理的忠实度

- 英文标题：Question decomposition improves the faithfulness of model-generated reasoning
- 发布日期：2023-07-18
- 官网栏目：对齐
- 原文：[Anthropic 页面](https://www.anthropic.com/research/question-decomposition-improves-the-faithfulness-of-model-generated-reasoning)
- 官网署名：Anthropic
- 归档范围：官网介绍页。另站发布的论文或报告请使用下方链接。
- 官网推荐阅读：[下载论文](https://assets.claude.com/8154fb1d828cdc390dc1fa442d84034948679c47.pdf)

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

### 摘要

随着大型语言模型（LLM）执行更困难的任务，验证其行为的正确性和安全性变得更加困难。帮助解决这个问题的一种方法是促使法学硕士将他们的推理具体化，例如，让他们在回答问题时生成逐步推理（思想链；CoT）。推理可以使我们检查模型用于执行任务的过程。然而，这种方法依赖于忠实反映模型实际推理的陈述推理，但情况并非总是如此。为了提高 CoT 推理的准确性，我们的模型通过将问题分解为子问题来生成推理。基于分解的方法在问答任务上实现了强大的性能，有时接近 CoT，同时提高了模型在最近提出的几个指标上陈述的推理的可信度。通过强制模型在不同的上下文中回答更简单的子问题，我们大大提高了模型生成推理相对于 CoT 的忠实度，同时仍然实现了 CoT 的一些性能提升。我们的结果表明，提高模型生成推理的可信度是可能的；持续的改进可能会导致推理，使我们能够验证 LLM 行为的正确性和安全性。
