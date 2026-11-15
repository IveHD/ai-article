# 衡量思维链推理的忠实度

- 英文标题：Measuring faithfulness in Chain-of-Thought reasoning
- 发布日期：2023-07-18
- 官网栏目：对齐
- 原文：[Anthropic 页面](https://www.anthropic.com/research/measuring-faithfulness-in-chain-of-thought-reasoning)
- 官网署名：Anthropic
- 归档范围：官网介绍页。另站发布的论文或报告请使用下方链接。
- 官网推荐阅读：[下载论文](https://assets.claude.com/827afa7dd36e4afbb1a49c735bfbb2c69749756e.pdf)

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

### 摘要

当大型语言模型 (LLM) 在回答问题之前产生逐步的“思想链”(CoT) 推理时，其表现会更好，但尚不清楚所陈述的推理是否是对模型实际推理（即其回答问题的过程）的忠实解释。我们通过检查当我们干预 CoT 时模型预测如何变化（例如，通过添加错误或解释它），来研究 CoT 推理如何不忠实的假设。模型在预测答案时对 CoT 的影响程度方面显示出不同任务之间的巨大差异，有时严重依赖 CoT，有时则主要忽略它。CoT 的性能提升似乎不仅仅来自 CoT 增加的测试时计算，也不是来自通过 CoT 的特定措辞编码的信息。随着模型变得更大、能力更强，它们在我们研究的大多数任务上产生的推理就不那么忠实了。总体而言，我们的结果表明，如果模型大小和任务等环境经过精心选择，CoT 可以是忠实的。
