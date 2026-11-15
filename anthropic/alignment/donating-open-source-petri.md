# 捐赠我们的开源对齐工具

- 英文标题：Donating our open-source alignment tool
- 发布日期：2026-05-07
- 官网栏目：对齐
- 原文：[Anthropic 页面](https://www.anthropic.com/research/donating-open-source-petri)
- 官网署名：Anthropic

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

![捐赠我们的开源对齐工具](../assets/dde5c70199d5aba6.webp)

2025 年 10 月，我们推出 [佩特里](https://www.anthropic.com/research/petri-open-source-auditing)，一个开源的对齐测试工具箱，可以应用于任何大型语言模型。Petri 是我们人类研究员计划的一部分而开发的，可用于快速、轻松地测试人工智能模型是否存在欺骗、阿谀奉承以及与有害请求合作等倾向。这是我们开发对整个人工智能开发社区开放且有用的对齐工具的努力的一部分。

自 Claude Sonnet 4.5 以来，Petri 一直是我们对每个 Claude 模型进行对齐评估的一部分。它比较了新模型在由单独的“审核器”模型模拟的一系列与对齐相关的场景中的行为方式。然后，进一步的“判断”模型会对结果记录中的不一致行为进行评分。

我们很高兴看到 Petri 被外部组织使用：例如，英国人工智能安全研究所 (AISI) 将其列为 [主要部分](https://arxiv.org/abs/2604.00788) 他们如何评估模型破坏人工智能研究的倾向。

我们现在正在将 Petri 更新到第三个版本。以下是一些最大的变化：

- *适应性。* Petri 3.0 涉及重大的架构更改，允许用户使其适应更多用途，特别是通过将审计模型和目标模型拆分为可以单独调整的单独组件；
- *现实主义。* 尽管对齐研究人员试图使测试看起来更真实，但模型通常可以从设置中的各种人为因素中推断出它实际上是测试的一部分。如果模型知道它正在被评估，研究人员就无法再看到模型的行为 *一般而言*。Petri 的一个附加组件（我们称之为“Dish”）使设置更加真实，例如，通过使用模型的真实系统提示符和真实模型部署中使用的真实“支架”（围绕模型以帮助其实现目标的软件）来运行测试；
- *深度*。我们现在已将 Petri 与我们的其他开源对齐工具集成， [布鲁姆](https://www.anthropic.com/research/bloom)，它可以对特定选择的行为进行更深入的评估（与 Petri 更广泛的方法相比）。

我们还给佩特里一个新家。我们已将其开发移交给 [子午线实验室](https://meridianlabs.ai/)，一个人工智能评估非营利组织。这个动作——类似于我们 [捐赠的](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation) Linux 基金会的模型上下文协议 (MCP) 将有助于确保 Petri 保持独立于任何人工智能实验室，以便其结果被整个行业内外的人士视为中立和可信。

作为 Meridian Labs 的一部分，Petri 加入了其他工具，例如 [检查](https://inspect.aisi.org.uk/) 和 [侦察兵](https://meridianlabs-ai.github.io/inspect_scout/)，构建一个向实验室、独立研究人员和政府开放的技术堆栈，此时对人工智能模型行为的可靠测试比以往任何时候都更加重要。

您可以阅读有关 Petri 3.0 的更多信息 [Meridian 实验室博客](https://meridianlabs.ai/blog/posts/introducing-petri-3/).

安装和使用 Petri 的说明可以在 [培养皿网站](https://meridianlabs-ai.github.io/inspect_petri/).
