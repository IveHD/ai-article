# 用红队测试减少语言模型的危害：方法、规模效应与经验

- 英文标题：Red teaming language models to reduce harms: methods, scaling behaviors, and lessons learned
- 发布日期：2022-08-22
- 官网栏目：社会影响
- 原文：[Anthropic 页面](https://www.anthropic.com/research/red-teaming-language-models-to-reduce-harms-methods-scaling-behaviors-and-lessons-learned)
- 官网署名：Anthropic
- 归档范围：官网介绍页。另站发布的论文或报告请使用下方链接。
- 官网推荐阅读：[阅读论文](https://arxiv.org/abs/2209.07858)

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

![减少危害的红队语言模型：方法、扩展行为和经验教训](../assets/1f379c74859fe7a6.webp)

### 摘要

我们描述了我们对红队语言模型的早期努力，以便同时发现、测量并尝试减少其潜在的有害输出。我们做出了三项主要贡献。首先，我们研究了红队在 3 种模型大小（2.7B、13B 和 52B 参数）和 4 种模型类型中的扩展行为：简单语言模型 (LM)； LM 被提示要乐于助人、诚实且无害；带有拒绝采样的 LM；以及一个使用人类反馈强化学习（RLHF）训练成有益无害的模型。我们发现 RLHF 模型随着规模的扩大越来越难以红队，并且我们发现其他模型类型的规模呈平坦趋势。其次，我们发布了包含 38,961 次红队攻击的数据集，供其他人分析和学习。我们提供自己的数据分析，并发现各种有害的输出，从攻击性语言到更微妙的有害非暴力不道德输出。第三，我们详尽地描述了我们的指示、流程、统计方法以及红队的不确定性。我们希望这种透明度能够加快我们作为一个社区的合作能力，以便为如何红队语言模型制定共享规范、实践和技术标准。

### 政策备忘录

[红队政策备忘录](https://assets.claude.com/82564d4ec2451b2eed2e0796b7c658fc989f0c1a.pdf)
