# 评估并缓解语言模型决策中的歧视

- 英文标题：Evaluating and mitigating discrimination in language model decisions
- 发布日期：2023-12-07
- 官网栏目：社会影响
- 原文：[Anthropic 页面](https://www.anthropic.com/research/evaluating-and-mitigating-discrimination-in-language-model-decisions)
- 官网署名：Anthropic
- 官网推荐阅读：[阅读论文](http://arxiv.org/abs/2312.03689)

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

![评估和减轻语言模型决策中的歧视](../assets/fc13e14e554493b8.webp)

### 摘要

随着语言模型 (LM) 的进步，人们越来越有兴趣将其应用于高风险的社会决策，例如确定融资或住房资格。然而，它们在这种情况下可能受到的歧视引起了道德问题，因此需要更好的方法来评估这些风险。我们提出了一种方法，用于主动评估 LM 在各种用例中的潜在歧视性影响，包括尚未部署的假设用例。具体来说，我们使用 LM 生成一系列广泛的潜在提示，决策者可以将这些提示输入到 LM 中，涵盖社会上 70 个不同的决策场景，并系统地改变每个提示中的人口统计信息。应用这种方法揭示了Claude 2.0 模型中在不采取干预措施的情况下在选定环境中的积极和消极歧视的模式。虽然我们不认可或允许使用语言模型为我们研究的高风险用例做出自动决策，但我们展示了通过仔细的提示工程显着减少积极和消极歧视的技术，提供了在可能合适的用例中更安全部署的途径。随着语言模型功能和应用程序的不断扩展，我们的工作使开发人员和政策制定者能够预测、衡量和解决歧视问题。我们发布数据集和提示 [这里](https://huggingface.co/datasets/Anthropic/discrim-eval).

### 政策备忘录

[评估和减轻语言模型决策中的歧视政策备忘录](https://assets.claude.com/f0dfb70b9b309d7c52845f73da8d964140669ff7.pdf)
