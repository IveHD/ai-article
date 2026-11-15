# 用 Claude 3.5 Sonnet 刷新 SWE-bench Verified 的成绩

- 英文标题：Raising the bar on SWE-bench Verified with Claude 3.5 Sonnet
- 发布日期：2025-01-06
- 官网栏目：工程
- 原文：[Anthropic 页面](https://www.anthropic.com/research/swe-bench-sonnet)
- 官网署名：Anthropic

> 中文机器初译，未逐篇人工校对。引用研究结论时请核对英文原文。

*我们的最新型号，升级版 [Claude 3.5 十四行诗](https://www.anthropic.com/news/3-5-models-and-computer-use)，在软件工程评估 SWE-bench Verified 上取得了 49% 的成绩，超过了之前最先进模型的 45%。这篇文章解释了我们围绕该模型构建的“代理”，旨在帮助开发人员从 Claude 3.5 Sonnet 中获得最佳性能。*

[SWE-长凳](https://www.swebench.com/) 是一个人工智能评估基准，用于评估模型完成现实世界软件工程任务的能力。具体来说，它测试模型如何解决流行开源 Python 存储库中的 GitHub 问题。对于基准测试中的每项任务，都会为 AI 模型提供一个已设置的 Python 环境以及问题解决之前的存储库的签出（本地工作副本）。然后，模型需要在提交其建议的解决方案之前理解、修改和测试代码。

每个解决方案都根据来自关闭原始 GitHub 问题的拉取请求的真实单元测试进行评分。这测试了人工智能模型是否能够实现与 PR 的原始人类作者相同的功能。

SWE-bench 不仅仅评估孤立的人工智能模型，而是评估整个“代理”系统。在这种情况下，“代理”是指人工智能模型及其周围软件支架的组合。该脚手架负责生成进入模型的提示，解析模型的输出以采取操作，并管理交互循环，将模型先前操作的结果合并到下一个提示中。即使使用相同的底层 AI 模型，SWE-bench 上代理的性能也会根据此脚手架而显着不同。

对于大型语言模型的编码能力还有许多其他基准测试，但 SWE-bench 因其以下几个原因而受到欢迎：

1. 它使用来自实际项目的真实工程任务，而不是竞赛或面试式的问题；
2. 它尚未饱和——还有很大的改进空间。在 SWE-bench Verified 上尚未有模型完成度超过 50%（尽管更新后的 Claude 3.5 Sonnet 在撰写本文时已完成 49%）；
3. 它测量整个“代理”，而不是孤立的模型。开源开发人员和初创公司在优化脚手架方面取得了巨大成功，大大提高了同一模型的性能。

请注意，原始 SWE-bench 数据集包含一些任务，如果没有 GitHub 问题之外的其他上下文（例如，关于要返回的特定错误消息），则无法解决这些任务。 [SWE-工作台-验证](https://openai.com/index/introducing-swe-bench-verified/) 是 SWE-bench 的 500 个问题子集，经过人类审查以确保它们是可解决的，从而提供了编码代理性能的最清晰衡量标准。这是我们将在本文中提到的基准。

## 达到最先进的水平

### 工具使用代理

在创建针对更新的 Claude 3.5 Sonnet 进行优化的代理支架时，我们的设计理念是为语言模型本身提供尽可能多的控制，并保持支架最小化。该代理有一个提示符、一个用于执行 bash 命令的 Bash 工具以及一个用于查看和编辑文件和目录的编辑工具。我们继续采样，直到模型确定它已完成，或超过其 200k 上下文长度。该支架允许模型使用自己的判断来解决问题，而不是硬编码到特定的模式或工作流程中。

提示概述了模型的建议方法，但对于此任务来说并不过长或太详细。该模型可以自由选择如何从一步移动到另一步，而不是进行严格和离散的转换。如果您对令牌不敏感，那么明确鼓励模型产生长响应会有所帮助。

以下代码显示了我们的代理脚手架的提示：

```
<uploaded_files>
{location}
</uploaded_files>
I've uploaded a python code repository in the directory {location} (not in /tmp/inputs). Consider the following PR description:

<pr_description>
{pr_description}
</pr_description>

Can you help me implement the necessary changes to the repository so that the requirements specified in the <pr_description> are met?
I've already taken care of all changes to any of the test files described in the <pr_description>. This means you DON'T have to modify the testing logic or any of the tests in any way!

Your task is to make the minimal changes to non-tests files in the {location} directory to ensure the <pr_description> is satisfied.

Follow these steps to resolve the issue:
1. As a first step, it might be a good idea to explore the repo to familiarize yourself with its structure.
2. Create a script to reproduce the error and execute it with `python <filename.py>` using the BashTool, to confirm the error
3. Edit the sourcecode of the repo to resolve the issue
4. Rerun your reproduce script and confirm that the error is fixed!
5. Think about edgecases and make sure your fix handles them as well

Your thinking should be thorough and so it's fine if it's very long.
```

该模型的第一个工具执行 Bash 命令。该架构很简单，仅需要在环境中运行的命令。然而，该工具的描述更有分量。它包含更详细的模型说明，包括转义输入、缺乏互联网访问以及如何在后台运行命令。

接下来，我们展示 Bash 工具的规范：

```
{
   "name": "bash",
   "description": "Run commands in a bash shell\n
* When invoking this tool, the contents of the \"command\" parameter does NOT need to be XML-escaped.\n
* You don't have access to the internet via this tool.\n
* You do have access to a mirror of common linux and python packages via apt and pip.\n
* State is persistent across command calls and discussions with the user.\n
* To inspect a particular line range of a file, e.g. lines 10-25, try 'sed -n 10,25p /path/to/the/file'.\n
* Please avoid commands that may produce a very large amount of output.\n
* Please run long lived commands in the background, e.g. 'sleep 10 &' or start a server in the background.",
   "input_schema": {
       "type": "object",
       "properties": {
           "command": {
               "type": "string",
               "description": "The bash command to run."
           }
       },
       "required": ["command"]
   }
}
```

模型的第二个工具（编辑工具）要复杂得多，包含模型查看、创建和编辑文件所需的一切。同样，我们的工具描述包含有关如何使用该工具的模型的详细信息。

我们在各种代理任务中的这些工具的描述和规格上投入了大量精力。我们对它们进行了测试，以发现模型可能误解规范的任何方式，或者使用这些工具可能存在的陷阱，然后编辑描述以预防这些问题。我们认为，应该更多地关注为模型设计工具界面，就像大量关注为人类设计工具界面一样。

以下代码显示了我们的编辑工具的描述：

```
{
   "name": "str_replace_editor",
   "description": "Custom editing tool for viewing, creating and editing files\n
* State is persistent across command calls and discussions with the user\n
* If `path` is a file, `view` displays the result of applying `cat -n`. If `path` is a directory, `view` lists non-hidden files and directories up to 2 levels deep\n
* The `create` command cannot be used if the specified `path` already exists as a file\n
* If a `command` generates a long output, it will be truncated and marked with `<response clipped>` \n
* The `undo_edit` command will revert the last edit made to the file at `path`\n
\n
Notes for using the `str_replace` command:\n
* The `old_str` parameter should match EXACTLY one or more consecutive lines from the original file. Be mindful of whitespaces!\n
* If the `old_str` parameter is not unique in the file, the replacement will not be performed. Make sure to include enough context in `old_str` to make it unique\n
* The `new_str` parameter should contain the edited lines that should replace the `old_str`",
...
```

我们提高性能的一种方法是“防错”我们的工具。例如，有时，在代理移出根目录后，模型可能会弄乱相对文件路径。为了防止这种情况，我们只是让该工具始终需要绝对路径。

我们尝试了几种不同的策略来指定对现有文件的编辑，并且在字符串替换方面具有最高的可靠性，其中模型指定“old_str”在给定文件中替换为“new_str”。仅当“old_str”恰好有一个匹配时才会发生替换。如果匹配项较多或较少，模型会显示一条适当的错误消息以供重试。

我们的编辑工具的规格如下所示：

```
...
   "input_schema": {
       "type": "object",
       "properties": {
           "command": {
               "type": "string",
               "enum": ["view", "create", "str_replace", "insert", "undo_edit"],
               "description": "The commands to run. Allowed options are: `view`, `create`, `str_replace`, `insert`, `undo_edit`."
           },
           "file_text": {
               "description": "Required parameter of `create` command, with the content of the file to be created.",
               "type": "string"
           },
           "insert_line": {
               "description": "Required parameter of `insert` command. The `new_str` will be inserted AFTER the line `insert_line` of `path`.",
               "type": "integer"
           },
           "new_str": {
               "description": "Required parameter of `str_replace` command containing the new string. Required parameter of `insert` command containing the string to insert.",
               "type": "string"
           },
           "old_str": {
               "description": "Required parameter of `str_replace` command containing the string in `path` to replace.",
               "type": "string"
           },
           "path": {
               "description": "Absolute path to file or directory, e.g. `/repo/file.py` or `/repo`.",
               "type": "string"
           },
           "view_range": {
               "description": "Optional parameter of `view` command when `path` points to a file. If none is given, the full file is shown. If provided, the file will be shown in the indicated line number range, e.g. [11, 12] will show lines 11 and 12. Indexing at 1 to start. Setting `[start_line, -1]` shows all lines from `start_line` to the end of the file.",
               "items": {
                   "type": "integer"
               },
               "type": "array"
           }
       },
       "required": ["command", "path"]
   }
}
```

## 结果

总的来说，升级后的 Claude 3.5 Sonnet 表现出比我们之前的模型更高的推理、编码和数学能力，并且 [以前最先进的](https://solverai.com/) 模型。它还展示了改进的代理能力：工具和脚手架有助于充分利用这些改进的能力。

型号

**Claude 3.5 十四行诗（新）**

以前的SOTA

Claude 3.5 十四行诗（旧）

Claude 3 作品

SWE-bench 验证分数

49%

45%

33%

22%

SWE-bench 上的分数已验证我们的一些模型，所有模型都使用此代理支架。

## 代理行为示例

为了运行基准测试，我们使用了 [SWE-代理](https://swe-agent.com/) 框架作为我们代理代码的基础。在下面的日志中，我们将代理的文本输出、工具调用和工具响应呈现为思想、行动和观察，即使我们不将模型限制为固定顺序。

下面的代码块将介绍 Sonnet 3.5 解决 SWE 基准问题的典型案例。

在第一个块中，您可以看到给予模型的初始提示的一部分，其中“{pr_description}”填充了 SWE 基准任务的实际值。重要的是，此任务包含重现问题的步骤，这将为模型提供一个有价值的调查起点。

```
<uploaded_files>
/repo
</uploaded_files>
I've uploaded a python code repository in the directory /repo (not in /tmp/inputs). Consider the following PR description:

<pr_description>
linear_model.RidgeClassifierCV's Parameter store_cv_values issue
#### Description
Parameter store_cv_values error on sklearn.linear_model.RidgeClassifierCV

#### Steps/Code to Reproduce
import numpy as np
from sklearn import linear_model as lm

#test database
n = 100
x = np.random.randn(n, 30)
y = np.random.normal(size = n)

rr = lm.RidgeClassifierCV(alphas = np.arange(0.1, 1000, 0.1), normalize = True, 
                                         store_cv_values = True).fit(x, y)

#### Expected Results
Expected to get the usual ridge regression model output, keeping the cross validation predictions as attribute.

#### Actual Results
TypeError: __init__() got an unexpected keyword argument 'store_cv_values'

lm.RidgeClassifierCV actually has no parameter store_cv_values, even though some attributes depends on it.
```

模型响应并首先使用编辑工具查看存储库结构。您可以在 THOUGHT 和 ACTION 下查看模型的文本输出和工具调用参数，以及在 OBSERVATION 下查看工具的部分输出：

```
THOUGHT:
I'll help you implement the changes to satisfy the PR description. Let's follow the steps you outlined.

1. First, let's explore the repo structure:
ACTION (str_replace_editor):
command:
view

path:
/repo

OBSERVATION:
Here's the files and directories up to 2 levels deep in /repo, excluding hidden items:
/repo
/repo/PULL_REQUEST_TEMPLATE.md
/repo/ISSUE_TEMPLATE.md
/repo/sklearn
/repo/sklearn/decomposition
/repo/sklearn/kernel_ridge.py
/repo/sklearn/model_selection
/repo/sklearn/covariance
/repo/sklearn/cluster
/repo/sklearn/base.py
/repo/sklearn/dummy.py
/repo/sklearn/naive_bayes.py
...
```

现在模型对存储库结构有了更好的了解，它使用编辑工具创建一个新脚本，用于重现问题并测试其修复：

```
THOUGHT:
Based on the repository structure, we'll need to modify the Ridge Classifier in the `sklearn/linear_model` directory.

Let's create a script to reproduce the error:
ACTION (str_replace_editor):
command:
create

path:
/repo/reproduce_error.py

file_text:
import numpy as np
from sklearn import linear_model as lm

#test database
n = 100
x = np.random.randn(n, 30)
y = np.random.normal(size = n)

rr = lm.RidgeClassifierCV(alphas=np.arange(0.1, 1000, 0.1), normalize=True, 
                         store_cv_values=True).fit(x, y)

OBSERVATION:
File created successfully at: /repo/reproduce_error.py
```

然后，模型使用 Bash 工具执行它编写的脚本，并成功重现任务中的问题：

```
THOUGHT:
Let's verify that we can reproduce the error:
ACTION (bash):
command:
python3 /repo/reproduce_error.py

OBSERVATION:
Traceback (most recent call last):
  File "/repo/reproduce_error.py", line 10, in <module>
    store_cv_values=True).fit(x, y)
TypeError: __init__() got an unexpected keyword argument 'store_cv_values'
```

从现在开始，模型使用编辑工具更改存储库中的源代码，并重新运行其脚本以验证更改是否解决了问题：

```
THOUGHT:
The error is reproducible. Looking at the code, I can see that the issue is that while the `RidgeClassifierCV` class inherits from `_BaseRidgeCV`, it fails to pass the `store_cv_values` parameter from its constructor through to the base class constructor.

Let's fix this:
ACTION (str_replace_editor):
command:
str_replace

path:
/repo/sklearn/linear_model/ridge.py

old_str:
    def __init__(self, alphas=(0.1, 1.0, 10.0), fit_intercept=True,
                 normalize=False, scoring=None, cv=None, class_weight=None):
        super(RidgeClassifierCV, self).__init__(
            alphas=alphas, fit_intercept=fit_intercept, normalize=normalize,
            scoring=scoring, cv=cv)
        self.class_weight = class_weight

new_str:
    def __init__(self, alphas=(0.1, 1.0, 10.0), fit_intercept=True,
                 normalize=False, scoring=None, cv=None, class_weight=None,
                 store_cv_values=False):
        super(RidgeClassifierCV, self).__init__(
            alphas=alphas, fit_intercept=fit_intercept, normalize=normalize,
            scoring=scoring, cv=cv, store_cv_values=store_cv_values)
        self.class_weight = class_weight
```

在这个特定示例中，模型运行了 12 个步骤，然后才决定准备好提交。然后，该任务的测试成功运行，验证模型的解决方案是否解决了问题。有些任务需要经过 100 轮以上才模型提交解决方案；在其他情况下，模型会不断尝试，直到脱离上下文。

通过回顾更新后的 Claude 3.5 Sonnet 与旧型号相比的尝试，更新后的 3.5 Sonnet 自我更正的频率更高。它还展示了尝试几种不同解决方案的能力，而不是一遍又一遍地犯同样的错误。

## 挑战

SWE-bench Verified 是一项功能强大的评估，但它的运行也比简单的单轮评估更复杂。这些是我们在使用它时面临的一些挑战——其他人工智能开发人员也可能遇到的挑战。

1. **持续时间和高代币成本。** 上面的例子来自一个通过12步成功完成的案例。然而，许多成功的运行模型需要数百轮才能解析，并且需要超过 100k 个令牌。更新后的 Claude 3.5 Sonnet 非常顽强：只要有足够的时间，它通常可以找到解决问题的方法，但这可能会很昂贵；
2. **等级。** 在检查失败的任务时，我们发现模型表现正常，但存在环境设置问题，或者应用两次安装补丁的问题。解决这些系统问题对于准确了解AI 智能体的性能至关重要。
3. **隐藏的测试。** 由于模型无法看到其评分所依据的测试，因此它经常“认为”自己已经成功，而实际上任务却失败了。其中一些失败是因为模型在错误的抽象级别解决了问题（应用创可贴而不是更深层次的重构）。其他失败感觉有点不公平：它们解决了问题，但与原始任务的单元测试不匹配。
4. **多式联运。** 尽管更新后的 Claude 3.5 Sonnet 具有出色的视觉和多模式功能，但我们没有实现让它查看保存到文件系统或作为 URL 引用的文件的方法。这使得调试某些任务（尤其是来自 Matplotlib 的任务）特别困难，并且还容易出现模型幻觉。这里肯定有一些容易实现的成果可供开发人员改进——SWE-bench 推出了一个新的 [评估侧重于多模式任务](https://www.swebench.com/multimodal.html)。我们期待在不久的将来看到开发人员在 Claude 的评估中取得更高的分数。

升级后的 Claude 3.5 Sonnet 在 SWE-bench Verified 上取得了 49% 的成绩，击败了之前的最先进版本 (45%)，具有简单的提示和两个通用工具。我们相信，使用新的 Claude 3.5 Sonnet 构建的开发人员将很快找到新的、更好的方法来提高 SWE 基准分数，超过我们最初在这里演示的分数。
