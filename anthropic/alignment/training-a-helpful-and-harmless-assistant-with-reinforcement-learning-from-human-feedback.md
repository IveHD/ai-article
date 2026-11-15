# 用基于人类反馈的强化学习训练有用且无害的助手

- 英文标题：Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback
- 发布日期：2022-04-12
- 官网栏目：对齐
- 原文：[Anthropic 页面](https://www.anthropic.com/research/training-a-helpful-and-harmless-assistant-with-reinforcement-learning-from-human-feedback)
- 官网署名：Anthropic
- 归档范围：官网介绍页。另站发布的论文或报告请使用下方链接。
- 官网推荐阅读：[阅读论文](https://arxiv.org/abs/2204.05862)

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

### 摘要

我们应用偏好建模和基于人类反馈（RLHF）的强化学习来微调语言模型，使其充当有用且无害的助手。我们发现这种对齐训练提高了几乎所有 NLP 评估的性能，并且与 Python 编码和摘要等专业技能的训练完全兼容。我们探索迭代的在线训练模式，其中偏好模型和强化学习策略每周都会使用新的人类反馈数据进行更新，从而有效地改进我们的数据集和模型。最后，我们研究了 RLHF 训练的鲁棒性，并确定了 RL 奖励与策略及其初始化之间 KL 散度的平方根之间的大致线性关系。除了我们的主要结果之外，我们还对校准、竞争目标和 OOD 检测的使用进行外围分析，将我们的模型与人类作者进行比较，并使用最近相关工作中出现的提示提供模型中的样本。
