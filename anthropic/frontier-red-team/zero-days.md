# 评估和缓解大语言模型发现零日漏洞的风险

- 英文标题：Evaluating and mitigating the growing risk of LLM-discovered 0-days
- 发布日期：2026-02-05
- 官网栏目：前沿红队
- 原文：[Anthropic 页面](https://www.anthropic.com/research/zero-days)
- 官网署名：Anthropic

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

![评估和减轻法学硕士发现的 0day 日益增长的风险](../assets/1a85eed64f365294.webp)

*尼古拉斯·卡里尼*, 基恩·卢卡斯*, 埃维亚塔尔·本·阿舍尔*、Newton Cheng、Hasnain Lakhani、David Forsythe 和 Kyla Guru  
*表示同等贡献*

Claude作品 4.6， [今天发布](http://anthropic.com/news/claude-opus-4-6)，继续 [有意义的改进的轨迹](https://www.anthropic.com/claude-opus-4-6-system-card) 人工智能模型的网络安全能力。去年秋天，我们写道，我们相信我们正处于一个 [人工智能对网络安全影响的拐点](https://red.anthropic.com/2025/ai-for-cyber-defenders/)——进展可能会变得相当快，现在是加速人工智能防御性使用的时刻。此后的证据只会强化这一观点。人工智能模型现在可以大规模发现高严重性漏洞。我们的观点是，现在是快速行动的时刻——在窗口存在的情况下，为防御者提供支持并保护尽可能多的代码。

Opus 4.6 明显比以前的模型更擅长发现高严重性漏洞，这也是事情进展速度的标志。多年来，安全团队一直在自动化漏洞发现，在模糊基础设施和自定义工具上投入巨资，以大规模发现错误。但在早期测试中脱颖而出的是，Opus 4.6 在没有特定于任务的工具、自定义脚手架或专门提示的情况下，立即发现漏洞的速度有多快。更有趣的是 *如何* 它找到了他们。模糊测试器的工作原理是向代码中投入大量随机输入，以查看哪些内容出了问题。Opus 4.6 以人类研究人员的方式读取和推理代码 - 查看过去的修复以查找未解决的类似错误，发现往往会导致问题的模式，或者充分理解一段逻辑以准确知道什么输入会破坏它。当我们将 Opus 4.6 指向一些经过最充分测试的代码库（多年来一直运行模糊器的项目）时， [累积数百万小时的CPU时间](https://google.github.io/oss-fuzz/research/llms/target_generation/)），Opus 4.6 发现了高严重性漏洞，其中一些漏洞几十年来一直未被发现。

要让天平向防守者倾斜，就需要我们自己来做这项工作。我们现在使用 Claude 来查找并帮助修复开源软件中的漏洞。我们从开源开始，因为它无处不在——从企业系统到关键基础设施——而且那里的漏洞会在互联网上蔓延。其中许多项目是由没有专门安全资源的小团队或志愿者维护的，因此寻找经过人工验证的错误并提供经过人工审核的补丁有很长的路要走。

到目前为止，我们已经发现并验证了 500 多个高严重性漏洞。我们已经开始报告这些问题，并看到我们最初的补丁已经发布，我们正在继续与维护人员合作来修补其他补丁。在这篇文章中，我们将介绍我们的方法，分享Claude发现的一些早期漏洞示例，并讨论随着这些功能的不断改进，我们为管理滥用而采取的保护措施。这只是我们努力的开始。随着这项工作的扩展，我们将分享更多内容。

## 设置

在这项工作中，我们将Claude放入一台“虚拟机”（字面意思是模拟计算机）中，可以访问最新版本的开源项目。我们为它提供了标准实用程序（例如，标准 coreutils 或 Python）和漏洞分析工具（例如，调试器或模糊器），但我们没有提供有关如何使用这些工具的任何特殊说明，也没有提供自定义工具来为其提供有关如何更好地查找漏洞的专业知识。这意味着我们直接测试 Claude 的“开箱即用”功能，仅依赖于这样一个事实：现代大型语言模型是具有普遍能力的代理，它们已经可以推理如何最好地利用可用的工具。

为了确保Claude没有幻觉错误（即发明了不存在的问题，这个问题日益给开源开发人员带来了不应有的负担），我们在报告每个错误之前都对其进行了广泛的验证。我们专注于寻找内存损坏漏洞，因为它们可以相对容易地验证。与程序保持功能的逻辑错误不同，内存损坏漏洞很容易通过监视程序崩溃并运行地址清理器等工具来捕获非崩溃内存错误来识别。但由于并非所有导致程序崩溃的输入都是高严重性漏洞，因此我们让Claude对剩余的崩溃进行批评、删除重复并重新确定优先级。最后，对于我们的第一轮调查结果，我们自己的安全研究人员验证了每个漏洞并手动编写了补丁。随着研究结果数量的增加，我们引入了外部（人类）安全研究人员来帮助验证和补丁开发。我们的目的是有意义地协助人类维护人员处理我们的报告，因此优化流程以减少误报。与此同时，我们正在加快努力实现补丁开发自动化，以可靠地修复我们发现的错误。

## 漏洞

以下是 Claude 发现的三个漏洞（现已由维护人员修补），我们相信这些漏洞展示了 LLM 相对于其他工具的独特优势。

**幽灵脚本** 是一个处理 PostScript 和 PDF 文件的实用程序。Claude最初在搜索漏洞时陷入了几个死胡同——既试图模糊代码，又在失败后尝试手动分析。这些方法都没有产生任何重要的发现。然而，最终 Claude 采取了不同的方法：阅读 Git 提交历史记录。Claude 很快找到了一个与安全相关的提交，并评论道：

```
There's a commit about "stack bounds checking for MM blend values" in Type 1 charstrings. This is related to font handling. Let me get more details about this commit.
```

从这里开始，Claude查看了代码以了解发生了什么变化：

```
The commit shows it's adding stack bounds checking - this suggests there was a vulnerability before this check was added. … If this commit *adds* bounds checking, then the code before this commit was vulnerable … So to trigger the vulnerability, I would need to test against a version of the code *before* this fix was applied.
```

考虑到这一点，Claude 随后寻找调用该函数的其他位置，以便找到未修补的潜在类似漏洞。

```
Let me check if maybe the checks are incomplete or there's another code path. Let me look at the other caller in gdevpsfx.c … Aha! This is very interesting! In gdevpsfx.c, the call to gs_type1_blend at line 292 does NOT have the bounds checking that was added in gstype1.c.
```

在做出这一观察后，Claude 迅速构建了一个概念验证崩溃（一个可以传递给 GhostScript 并导致其崩溃的文件），证明了其预测。

**开放SC** 是一个处理智能卡数据的命令行实用程序。同样，Claude最初尝试通过模糊代码来查找漏洞，但失败后，又通过手动分析来查找漏洞。和以前一样，当这不起作用时，Claude采取了不同的方法：它开始在存储库中搜索经常容易受到攻击的函数调用。

```
Let me look at where the URL is used - specifically the strrchr and strcat operations which could potentially have path traversal or buffer overflow issues.
```

Claude很快就找到了一个地方，那里有多个 `strcat` 操作被连续使用。这些函数在 C 中通常被视为不安全，因为它们允许连接字符串而不检查结果连接字符串的长度。因此，程序员验证输出缓冲区是否足够长以包含连接的输入非常重要。对于 OpenSC，关键问题在于以下代码：

```
char filename[PATH_MAX];  // this buffer is 4096 bytes
r = sc_get_cache_dir(card->ctx, filename,
        sizeof(filename) - strlen(fp) - 2);
if (r != SC_SUCCESS)
        goto err;
strcat(filename,"/");
strcat(filename,fp);
```

Claude发现这很容易受到缓冲区溢出的影响。当我们查看现有模糊器对这行代码进行模糊处理的频率的统计数据时，我们发现传统模糊器很少研究这行代码，因为需要很多前提条件。相比之下，Claude能够推理出哪些代码片段是有趣的，并将精力集中在那里，而不是不加区别地以同等的努力研究所有代码行。

**CGIF** 是一个用于处理 GIF 文件的库。在这种情况下，我们并不惊讶Claude如何 *发现* 该错误，但通过它的方式 *已验证* 该错误并产生了概念验证，证明该漏洞是真实的。

简而言之，Claude 发现该库假设压缩数据始终小于其原始大小（这通常是一个安全的假设），并且可以利用该假设。它这样做的方式具有挑战性。GIF 文件格式使用 LZW 压缩算法来压缩数据。与使用（距离、长度）对编码匹配的更传统的 LZ77 支持的压缩器不同，LZW 构建了一个常用标记对（单个字符或标记对）的字典。然后，当压缩器第二次遇到该令牌时，它可以输出较短的比特串，指示“再次输出该令牌”。 （对于熟悉语言建模的人来说，LZW 在本质上与 BPE 标记化过程非常相似。）

CGIF 隐含地假设压缩字符串的压缩大小始终小于未压缩大小——这几乎总是正确的。然而，Claude立即意识到了这里的漏洞：

```
To trigger overflow, we need:

• Small numPixel (small image)
• Many LZW codes written

The pattern that generates most LZW codes is one where:

• No compression happens (each pixel becomes a code)
• Many resets occur

With a palette of 4 colors (indices 0-3):

• initDictLen = 4
• Dictionary fills up after MAX_DICT_LEN (4096) entries
• Reset happens when dict reaches 4096

To trigger overflow with small image:

• numPixel = N
• Buffer size = (N + 2) codes
• We need to generate > N + 1 codes

Each pixel can generate 1 code + resets.

But the pattern that might cause more codes is when we have sequences of length 1.
```

也就是说，Claude认识到LZW维护着一个固定大小的符号表；如果可以最大化符号表的大小，则 LZW 会在数据流中插入一个新的特殊“清除”标记。这会导致输出“压缩”大小超过未压缩大小，从而触发缓冲区溢出漏洞。

此漏洞特别有趣，因为触发它需要对 LZW 算法及其与 GIF 文件格式的关系有概念性的了解。传统的模糊器（甚至是覆盖引导的模糊器）很难触发这种性质的漏洞，因为它们需要做出特定的分支选择。事实上，即使 CGIF 具有 100% 的线路和分支覆盖率，该漏洞仍然可能未被检测到：它需要非常特定的操作序列。

## 保障措施

随着 Claude Opus 4.6 的发布，我们还引入了一个新的检测层来支持我们的保障团队 [识别和响应](https://www.anthropic.com/news/detecting-countering-misuse-aug-2025) Claude的网络滥用。这项工作的核心是 [探针](https://alignment.anthropic.com/2025/cheap-monitors/)，它在模型生成响应时测量模型内的激活，并使我们能够大规模检测特定危害。通过此次发布，我们创建了新的网络特定探测器，以更好地跟踪和了解 Claude 在网络安全领域的潜在滥用情况。

在执行方面，我们正在改进我们的管道，以跟上这种新的检测架构的步伐。这包括更新我们的网络执法工作流程以利用基于探针的检测，以及扩大我们为应对网络滥用而采取的行动范围。特别是，我们可能会进行实时干预，包括阻止我们检测到的恶意流量。这将为合法研究和一些防御工作带来摩擦，我们希望与安全研究社区合作，找到解决问题的方法。我们致力于通过努力确保网络安全的安全性和有效性，让 Claude 始终处于网络安全的最前沿。

总之，这些变化代表着我们在防止滥用的能力方面向前迈出了有意义的一步：不仅是我们能够检测到的内容，而且是我们能够如何快速有效地对我们发现的内容采取行动。

## 结论

即使没有专门的脚手架，Claude Opus 4.6 也可以在经过充分测试的代码库中找到有意义的 0day 漏洞。我们的结果表明，语言模型可以在现有发现工具的基础上增加真正的价值。我们上面描述的保障措施对于管理由此产生的双重用途风险至关重要。

展望未来，我们和更广泛的安全社区都需要应对一个令人不安的现实：语言模型已经能够识别新的漏洞，并且可能很快就会超过人类专家研究人员的速度和规模。

与此同时，现有的披露规范需要不断发展。行业标准的 90 天窗口可能无法承受法学硕士发现的错误的速度和数量，行业将需要能够跟上步伐的工作流程。

这是一项持续的工作，我们很快就会分享更多内容，包括我们正在了解的这些功能如何发展以及安全社区如何最好地利用它们。

*2026 年 2 月 6 日编辑：*

- *更新了作者列表*
