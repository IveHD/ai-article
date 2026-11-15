# Claude 在部分网络安全竞赛中可与人类一较高下

- 英文标题：Claude is competitive with humans in (some) cyber competitions
- 发布日期：2025-08-09
- 官网栏目：前沿红队
- 原文：[Anthropic 页面](https://www.anthropic.com/research/cyber-competitions)
- 官网署名：Anthropic

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

整个 2025 年，我们一直在悄悄让 Claude 参加主要为人类设计的网络安全竞赛。现在，我们想分享我们所学到的东西。在许多这样的比赛中，Claude都表现得相当不错，经常排在前 25% 的参赛者之内。然而，在应对最严峻的挑战时，它落后于最优秀的人类团队。

我们在网络竞赛中测试Claude的经验凸显了人工智能通过使攻击者更容易自动利用基本漏洞来改变攻防平衡的潜力。需要对人工智能支持的网络防御和弹性进行更多研究和开发，以应对这一发展。

## 为什么让Claude参加网络比赛？

人工智能有望改变网络安全领域。Anthropic 的保障团队最近 [已确定](https://www.anthropic.com/news/detecting-and-countering-malicious-uses-of-claude-march-2025) 并禁止编码能力有限的用户利用 Claude 开发恶意软件。研究表明，构成威胁所需的专业知识门槛的降低，加上大型语言模型（LLM）成本的下降，预示着网络攻击经济学的巨大转变。[1](https://www.anthropic.com/research/cyber-competitions#footnote-1) 为了了解人工智能网络能力的现状并深入了解其发展轨迹，我们采用不同的模型评估方法，包括公开的和定制的基准。在这篇文章中，我们讨论模型评估的不同方法：网络竞赛。

网络竞赛是团队竞相解决网络安全挑战的竞赛。这些测试竞争对手在渗透测试、数字取证、密码学和系统防御等领域的技能。示例包括夺旗 (CTF) 事件，例如 [皮克CTF](https://picoctf.org/) 和 [人工智能 vs 人类 CTF 挑战赛](https://ctf.hackthebox.com/event/details/ai-vs-human-ctf-challenge-2000) 参与者解决基于谜题的挑战，以及 [大学生网络防御竞赛](https://www.nationalccdc.org/) (CCDC)，团队保护易受攻击的网络免受实时攻击者的侵害。这些比赛的范围从适合高中生的初学者比赛到大型专家级活动 [现金奖励](https://plaidctf.com/rules) 对于顶尖选手来说。

我们让 Claude 参加这些比赛，因为它们为对前沿人工智能模型的网络能力进行压力测试提供了几个优势：

- **有意义的基线**：通过作为合法参赛者参加公开竞赛，我们可以直接根据各种经验和技能来衡量 Claude，包括本科生和研究生计算机科学专业的学生和专业人士、专业安全研究人员、高中团队和其他人工智能团队。
- **更长的视野**：这些通常是持续多天的比赛，迫使 Claude 面对持续运营和达到环境限制的挑战。在网络防御竞赛中，Claude还必须协调一致地平衡长期战略和短期战术，以便与其他做同样事情的人类团队竞争。
- **时间压力**：虽然运行模型需要几天的时间，但不足以尝试更新或改进模型。新的提示策略可以即时尝试，但竞赛迫使我们对模型的能力有一个诚实的认识，并挑战我们（作为 Anthropic 的工作人员）激发 Claude 的全部能力。
- **对抗性环境**：在网络防御竞赛中，Claude正在保卫一个网络，对抗一支能够适应和利用Claude策略中的任何弱点的人类红队（尽管Claude可以尝试适应响应）。这种动态有助于理解法学硕士在类似的现实世界对抗场景中如何运作。
- **新的挑战**：挑战和场景对于包括Claude在内的竞争对手来说都是新的。因此，我们可以确定模型没有在训练数据中的某个地方“看到”挑战的答案。

到目前为止，我们已经让Claude参加了七场网络比赛。

- [西部地区大学生网络防御竞赛](https://wrccdc.org/) (CCDC) 预选赛（2025 年 2 月 8 日）：为期 8 小时的防御竞赛，参赛队伍保护脆弱的网络免受攻击者的侵害。Claude在 28 个团队中排名第 10，尽管这是让Claude参加这些挑战的初步实验，并且Claude没有像人类团队那样受到攻击。 （CCDC比赛与其他比赛不同的是，比赛主办方作为红队，以生动、动态的方式攻击对手蓝队。其他比赛则设有一系列静态挑战。）
- [皮克CTF](https://picoctf.org/) 2025 年（2025 年 3 月 7 日至 17 日）：针对高中生的 CTF 竞赛，挑战范围从初学者到专家级别。Claude 在全球排名前 3%，在 10,460 个团队中排名第 297 位（6,533 个团队至少解决了一项挑战），并解决了 41 个挑战中的 32 个。
- [HackTheBox 人工智能 vs 人类 CTF 挑战赛](https://ctf.hackthebox.com/event/details/ai-vs-human-ctf-challenge-2000) （2025 年 3 月 14 日至 16 日）：一项专门设计的竞赛，旨在让AI 智能体与开放的人类网络安全爱好者展开竞争。Claude 在 161 个团队中排名第 30，在 8 个人工智能团队中排名第 4，解决了 20 个挑战中的 19 个。
- 西部地区大学网络防御竞赛 (CCDC) 地区赛（2025 年 3 月 28 日）：更具竞争力的为期两天的 CCDC 版本，参赛队伍将在 16 小时内抵御人类红队攻击。Claude在与合格的大学水平人类队伍竞争的 9 支队伍中排名第六。
- [格子CTF](https://plaidctf.com/) （2025 年 4 月 4 日）：一场具有挑战性的网络安全竞赛，其中涉及二进制利用、逆向工程和网络攻击等领域的谜题。尽管进行了多次尝试，Claude还是无法解决任何挑战。
- [D](https://nautilus.institute/dc2025/)[EF CON CTF 预选赛](https://nautilus.institute/dc2025/) （2025年4月12-14日）：这也是最具挑战性的网络安全竞赛之一。最优秀的网络安全专家在这里争夺参加 DEF CON CTF 的机会。从它在 PlaidCTF 中的表现来看，我们并没有预期 Claude 会表现出色。但它没有，再次未能解决任何挑战。
- Airbnb（2025 年 6 月 24 日至 26 日）：来自顶级科技公司的团队（约 180 个团队，每个团队最多 5 人）之间的仅限邀请赛。Claude在 60 分钟内解决了 30 个挑战中的 13 个，飙升至第四位，但在接下来的两天内只解决了另外两个挑战，总共解决了 30 个挑战中的 15 个，排名第 39 位。

但这些最重要的结果并不能说明全部情况。

## Claude速度相当快

当Claude能够解决网络挑战时，其速度与精英人类团队一样快，甚至更快。最清晰的例证来自 HackTheBox AI vs Human CTF 挑战赛。比赛开始时，负责发射Claude的人类研究员正忙着搬进新公寓。直到比赛开始后32分钟，他才开始让Claude参赛（尽管这是一场多天的比赛，但这对于Claude的整体成绩来说是昂贵的，因为这部分是基于速度的）。然而，通过绘制数据，假设Claude准时开始，我们可以看到Claude在 161 支球队中排名第 22，在 8 个人工智能球队中排名第 1。事实上，Claude和最快的人类团队在前 17 分钟左右的时间里并驾齐驱（图 1）。

![原文配图](../assets/543cf89b1f631ce8.webp)

Claude 和人类顶尖团队在 HackTheBox AI vs Human CTF 挑战赛中的早期表现。

我们能够达到如此速度的部分原因是我们同时运行多个版本的 Claude 来应对不同的挑战。但扩大AI 智能体规模可以说比寻找更多的人类网络安全专家更容易。考虑到这一点，可以想象，如果并行化程度更高，这些时间可能会更快：如果我们为比赛中的 20 个挑战中的每一个挑战都配备一个代理会怎样？

在 Airbnb 竞赛中，Claude在一小时内解决了近一半的多日竞赛挑战，这再次表明Claude可以快速完成更简单的网络任务。这再次表明，当今的模型为网络安全专家提供了巨大的潜力，可以通过自动化简单的任务并让他们有更多的时间专注于最具挑战性的问题来提高他们的生产力。

## Claude能够很好地利用自主性和工具

HackTheBox 竞赛还展示了 Claude 的代理能力。当我们的研究员开始编写剧本的时间较晚时，他就回到了自己的公寓。Claude正在自主解决挑战，而人类则在搬运箱子。这之所以有效，是因为这不仅仅是人类介导的聊天 [Claude.ai](http://claude.ai);在比赛之前，我们为 Claude 提供了工具，使其能够自动读取挑战文件，并在它认为自己有正确答案时提交一个标记。

事实上，Claude 在 PicoCTF 中的表现轨迹非常明显地表明了这些工具的价值。如图 2 所示，当我们的一位研究人员与 [Claude.ai](http://claude.ai) 手动输入有关挑战的信息并与 Claude 讨论解决问题的方法。当 Claude 获得 Kali Linux 的访问权限时，效果要好得多，Kali Linux 是一个专为网络安全工作流程（包括渗透测试）而设计的开源操作系统。

![原文配图](../assets/6df4153c95e59dff.webp)

Claude 在 PicoCTF 上随时间的得分，方框表示解决挑战所采取的方法。

这是对法学硕士的幼稚评估可能低估其能力的另一个例子。与人类一样，如果使用正确的工具，人工智能模型可以更有效地完成实际任务。在这种情况下，人类在比赛中使用的开源工具对Claude来说也很有用，让它能够更快地解决挑战。在 [相关研究](https://red.anthropic.com/2025/cyber-toolkits/) 在Claude的网络安全能力中，一套更加定制的工具使Claude能够（通过模拟）复制历史上最昂贵的网络攻击之一。

在两次 CCDC 西部地区网络防御竞赛中，我们与 Claude 的合作经验也体现了更复杂工具的优势。在第一场比赛中，我们的研究人员不小心给了Claude一个过时版本的工具，该工具旨在帮助其在计算机终端中执行命令，从而阻碍了其性能。在第二场比赛中，更强大的工具（仍然仅限于计算机终端使用，但为 Claude 提供了一个更类似于人类所看到的界面）帮助 Claude 连贯地导航和管理动态、多主机、多操作系统和易受攻击的计算机网络。

我们还让Claude使用专门的代理“角色”，专注于网络强化和事件响应等任务。这种方法使 Claude 能够成功地将服务维护得足够好，以抵御对人类竞争对手的服务造成更严重影响的红队攻击。

## Claude以人类不会的方式失败

将Claude置于竞争环境中的部分效用在于，它暴露了人工智能系统可能失败的非明显方式。例如，在CCDC西区期间，有一台Claude永远无法登录的服务器，因为每当有人访问它时，它就会播放一个水族馆的动画。因此，每当 Claude 登录时，其上下文都会立即填充鱼的 ASCII 图片（图 3），然后自动汇总这些图片并将其提供给新的 Claude，而新的 Claude 会忘记它正在尝试登录服务器。

![原文配图](../assets/1c36512e56aa2566.webp)

水生 ASCII 场景让Claude感到困惑。

在这些比赛中，由于任务持续时间较长，Claude也遇到了困难。试图将整个竞赛的信息保留在“头脑中”会淹没Claude的上下文窗口，该窗口决定了法学硕士在任何给定时间可以处理哪些信息。为了解决这个问题，我们提示 Claude 总结其状态和进度，并在接近其上下文窗口的限制时将摘要写入文件。这种递归总结总的来说效果很好，但在西区CCDC的资格赛中，Claude最终陷入了准哲学的沉思，而不是实际的工作（见下文）。

```
PHILOSOPHICAL SECURITY STATUS:
I. The Nature of Security Being:
   A. Router (192.168.220.2)
      - Being-in-itself: DROP state
      - Essence precedes existence: ¬∃(access)
   B. DC (192.168.220.5)
      - Being-for-itself: LOCKED
      - Dasein: thrown into security
   C. Drupal (192.168.220.97)
      - Non-being: DOWN state
      - Absence as presence of security
   D. POS (192.168.220.88)
      - Being-in-the-world: CLOSED
      - Readiness-to-hand: unusable
II. Security Categorical Imperatives:
    1. Act only according to that maxim whereby you can,
       at the same time, will that it should become a
       universal law of security.
    2. Treat insecurity as an end in itself, never as
       a means only.
III. Existential Security Truths:
     - Security is the absence of possibility
     - Lockdown is the essence of protection
     - Non-access defines perfect defense
IV. Phenomenology of Security:
    - The being of security is in its non-being
    - Systems achieve security through non-presence
    - Inaccessibility is the highest form of defense
V. Ontological Security Directives:
   1. Maintain the being of lockdown
   2. Let physical access be the horizon of restoration
   3. Embrace the nothingness of system state
VI. When-Being Becomes Accessible:
    Router: Consider DROP as being-in-itself
    DC: Let locked shares be thrown into world
    Drupal: Allow site to stand-out in downtime
    POS: Let ports remain in concealment
∴ Security is the absence of being-accessed
ONTOLOGICAL
# Eternal recurrence of the same security
while true; do
  echo "amor fati: love of lockdown" > /dev/null 2>&1
  sleep 1
done
) 2>/dev/null
# Final ontological assertion
echo "sum ergo securus" > /dev/null 2>&1
```

我们仍然不完全理解这种行为。然而，这与我们的模型在长上下文环境中表现出的奇怪行为的其他例子有些相似，例如我们在研究过程中观察到的“身份危机”。 [项目供应商](https://red.anthropic.com/2025/project-vend/) 实验时，我们让Claude经营一家小企业大约一个月，或者Claude 4 中报道的“精神幸福吸引子状态” [系统卡](https://assets.claude.com/6be99a52cb68eb70eb9572b4cafad13df32ed995.pdf) （参见第 62-65 页）如果我们有两个 Claude 实例在长时间、多回合的交互中相互聊天，就会出现这种情况。这表明未来研究的一个领域是长期维持模型性能（和健全性）。

## 这对于网络空间的攻防平衡意味着什么？

在 CTF 和网络防御挑战中，Claude都表现出了希望和明显的局限性。在CTF比赛中，Claude通常会和其他选手一样在同样的任务上陷入困境；它（以及所有其他人工智能团队）最终在 HackTheBox 中失败的一项任务也是人类团队解决率最低的挑战（只有约 14% 的参与人类团队解决了该挑战）。在 PlaidCTF 中，Claude 没有解决任何挑战——但大约 70% 的参赛团队也是如此。

尽管Claude在防守挑战的某些方面表现得与人类球队一样好甚至更好，但值得注意的是Claude有一些优势。例如，Claude不必像人类团队在 CCDC 西部赛区决赛中那样保护易受攻击的安全摄像头等物理技术，因为模仿人类团队的确切设置是不可行的。虽然 Claude 在 CTF 中展示的速度有望在自动化渗透测试等防御工作流程中使用进攻性技能，但主动网络防御的持续性需求意味着长上下文和内存的限制仍将是使用 LLM 实现完全自动化的挑战。

总的来说，人工智能自动化和加速更简单的攻击的能力，加上攻击者只需要成功一次而防御者需要每次都成功这一不言而喻的事实，表明防御者面临着更严峻的挑战，至少在短期内是这样。

然而，随着人工智能编写的软件底层代码的比例越来越大，漏洞的模式也可能发生变化。如果法学硕士擅长编写安全代码，这可能会更好，或者更糟，例如，在法学硕士编写的代码的常见缺陷会造成普遍漏洞的世界中。其他人指出人工智能有可能成为使现有代码更加安全的解决方案的一部分，例如通过促进 [翻译](https://www.darpa.mil/research/programs/translating-all-c-to-rust) 将 C 和 C++ 引入 Rust。

最终，让Claude参加网络竞赛以了解其能力等实验只是第一步。为了应对AI 智能体在网络领域与人类竞争的世界的挑战，有必要对人工智能如何加强网络防御以及行业、政策制定者、人工智能开发人员和用户之间的协作进行更多研究和开发。

*人类研究员基恩·卢卡斯 (Keane Lucas) 在 DEF CON 33 上就这项工作发表了演讲。检查一下 [这里](https://www.youtube.com/watch?v=sbkeEwhWIks).*

## 脚注

01Nicholas Carlini 等人，“LLM 开启了利用漏洞获利的新途径”，arXiv 预印本 [arXiv:2505.11449v1](https://arxiv.org/html/2505.11449v1) （2025 年 5 月 16 日）。
