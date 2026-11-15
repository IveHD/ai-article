# “金门大桥”版 Claude

- 英文标题：Golden Gate Claude
- 发布日期：2024-05-23
- 官网栏目：可解释性
- 原文：[Anthropic 页面](https://www.anthropic.com/research/golden-gate-claude)
- 官网署名：Anthropic

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

![金门Claude](../assets/eef66d2b6bddf45f.webp)

*更新：Golden Gate Claude 作为研究演示在线 24 小时，并且不再可用。如果您想了解更多关于我们对 Claude 内可解释性和功能激活的研究，请参阅 [这篇文章](https://www.anthropic.com/news/mapping-mind-language-model) 或我们的 [完整的研究论文](https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html).*

周二，我们 [发布了一篇重要的新研究论文](https://www.anthropic.com/research/mapping-mind-language-model) 关于解释大型语言模型，我们开始绘制人工智能模型 Claude 3 Sonnet 的内部运作方式。在Claude的“头脑”中，我们发现了数以百万计的概念，当模型读取相关文本或看到相关图像时，这些概念就会被激活，我们称之为“特征”。

其中之一就是金门大桥的概念。我们发现Claude的神经网络中有一种特定的神经元组合，当它遇到这个最著名的旧金山地标的提及（或图片）时，就会激活。

我们不仅可以识别这些特征，还可以上下调整它们的激活强度，并识别Claude行为的相应变化。

而正如我们 [在我们的研究论文中解释](https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html#:~:text=For%20instance%2C%20we%20see%20that%20clamping%20the%20Golden%20Gate%20Bridge%20feature%2034M/31164353%20to%2010%C3%97%20its%20maximum%20activation%20value%20induces%20thematically%2Drelated%20model%20behavior)，当我们调大“金门大桥”特征的强度时，Claude的反应开始集中在金门大桥上。它对大多数查询的答复都开始提到金门大桥，即使它不直接相关。

如果你问这位“金门Claude”如何花掉10美元，他会建议用它开车过金门大桥并交过路费。如果你要求它写一个爱情故事，它会告诉你一个汽车在雾天迫不及待地穿过心爱的桥梁的故事。如果你问它想象中的它是什么样子，它可能会告诉你它想象中的它看起来像金门大桥。

在短时间内，我们将让每个人都可以与这个模型进行交互。您可以与“Golden Gate Claude”交谈 [Claude.ai](https://claude.ai) （只需单击右侧的金门标志即可）。请记住，这只是一个研究演示，这个特定模型可能会以一些意想不到的、甚至是不和谐的方式表现。

我们的目标是让人们看到我们的可解释性工作可以产生的影响。事实上，我们可以在 Claude 中找到并改变这些功能，这让我们更有信心开始了解大型语言模型的真正工作原理。这不是口头要求模型做一些表演，也不是添加一个新的“系统提示”，为每个输入附加额外的文本，告诉Claude假装这是一座桥梁。它也不是传统的“微调”，即我们使用额外的训练数据来创建一个新的黑匣子来调整旧黑匣子的行为。这是对模型内部激活的一些最基本方面的精确的外科手术改变。

[正如我们在论文中所描述的](https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html)，我们可以使用这些相同的技术来改变强度 *安全相关* 功能——例如与危险计算机代码、犯罪活动或欺骗相关的功能。通过进一步的研究，我们相信这项工作可以帮助提高人工智能模型的安全性。
