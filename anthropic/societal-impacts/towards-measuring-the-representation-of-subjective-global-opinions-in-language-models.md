# 衡量语言模型如何表达全球不同人群的主观看法

- 英文标题：Towards measuring the representation of subjective global opinions in language models
- 发布日期：2023-06-29
- 官网栏目：社会影响
- 原文：[Anthropic 页面](https://www.anthropic.com/research/towards-measuring-the-representation-of-subjective-global-opinions-in-language-models)
- 官网署名：Anthropic
- 官网推荐阅读：[阅读论文](https://arxiv.org/abs/2306.16388)

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

![衡量语言模型中主观全球观点的表示](../assets/3d120b0feb0ff85d.webp)

### 摘要

大型语言模型（LLM）可能无法公平地代表关于社会问题的不同全球观点。在本文中，我们开发了一个定量框架来评估模型生成的响应与谁的意见更相似。我们首先构建了一个数据集 GlobalOpinionQA，其中包含来自跨国调查的问题和答案，旨在捕获不同国家对全球问题的不同意见。接下来，我们定义一个指标，用于量化法学硕士生成的调查响应与人类响应之间的相似性（以国家为条件）。通过我们的框架，我们对一位法学硕士进行了三个实验，该硕士经过训练，能够使用宪法人工智能提供帮助、诚实且无害。默认情况下，法学硕士的回答往往更类似于某些人群的观点，例如来自美国、一些欧洲和南美国家的观点，这凸显了存在偏见的可能性。当我们提示模型考虑特定国家的观点时，反应会变得更类似于提示人群的观点，但可能反映出有害的文化刻板印象。当我们将 GlobalOpinionQA 问题翻译成目标语言时，模型的回答不一定与这些语言的使用者的意见最相似。我们发布我们的数据集供其他人使用和构建。我们的数据位于 [这个网址](https://huggingface.co/datasets/Anthropic/llm_global_opinions)。我们还提供交互式可视化 [这个网址](https://llmglobalvalues.anthropic.com/).
