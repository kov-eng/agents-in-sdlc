---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。向 Copilot 提供特定上下文有多种方式，其中一种重要方式是[说明文件][instruction-files]，你可以通过它们为代码生成提供指导。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目专属上下文、编码指南和文档标准；
- 在当前说明文件生效时，生成用于筛选功能的第一部分数据层代码（publisher 辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准；
- 再次运行相同提示，观察生成的代码如何采用新标准；
- 将说明更新和筛选功能的数据层改动提交到 `main`，供 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合你设定的某些标准。Copilot 的输出具有非确定性。本练习旨在观察更新说明后行为的*整体变化趋势*，而不是要求输出逐字一致。

## 说明文件

### 场景

作为一家优秀的软件开发公司，Tailspin Toys 为开发实践制定了一系列指导方针和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。借助此功能，你可以引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望添加的注释类型。你可以为整个存储库创建说明，也可以为特定文件类型创建说明，为具体任务提供上下文。

说明文件有两种类型：

- `.github/copilot-instructions.md` 是一个说明文件，Copilot 每次处理该存储库的请求时都会收到它。此文件应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型创建。你可以为 TypeScript 或 Astro 等语言添加指导，也可以为创建 UI 组件或一组单元测试等任务添加指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时都会使用存储库级说明文件，以及具有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持使用其他标准编写的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理说明文件的最佳实践

全面介绍如何创建说明文件超出了本练习的范围。不过，示例项目展示了一种有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 中的说明限定为项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对文件类型（如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明正确和错误的写法。

创建说明文件没有唯一的正确方式，使用 AI 也是如此。通过实践，你会逐渐找到适合自己项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。浏览此项目中的说明文件时，你可能会发现有很多不同任务的文件，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同界面提供的操作方式有所不同（例如，在 VS Code 中选择 **Configure Chat → Generate Agent Instructions**，或在 Copilot CLI 中运行 `/init`）。相关练习会在适用时介绍具体方法。
>
> 想查找模板或入门示例？可以浏览 [awesome-copilot][awesome-copilot]，其中收录了大量说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义说明文件

花点时间阅读此存储库中的说明文件：其中有一个核心文件 `copilot-instructions.md`，还有一组用于不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网站界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 进行的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 阅读此项目中有关创建单元测试的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。此处的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前尚未要求为代码添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件头注释的规则。

## 为更改创建分支

现在遵循最佳实践，为更改创建一个分支。

1. 从上一练习返回 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 以开始新的对话。不同练习所用的模式和模型可能不同；每个步骤会在相关处说明。

要观察自定义说明的影响，先在当前说明文件生效时发送提示。稍后更新说明后，再次发送相同提示，比较两次结果。

1. 关闭之前练习中打开的编辑器选项卡，确保 Copilot 只获取你需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，建议的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的 publisher 数组。这一约定来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 注意，建议的代码**缺少** TSDoc 文档注释和文件级头注释。

> [!CAUTION]
> 由于 Copilot 的输出具有概率性，即使没有相关要求，它也可能添加文档注释。如果发生这种情况也没关系；本练习的重点仍然是观察更新说明后，输出一致性的改善。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一条存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意其中介绍了项目编码标准，但没有关于代码文档的规则，这正是生成的辅助函数缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建说明文件以提供全局指导，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚刚添加的文档注释规则。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，然后选择 **New Chat** 清除对话内容。
2. 选择 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 再次发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，现在建议的文件开头会包含类似下面的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的 publisher 数据访问辅助函数。
    * 提供从数据库检索 publisher 信息的函数。
    */
   ```

5. 注意，现在建议的函数包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有 publisher 的 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为 publisher 对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循一项新的项目标准，并将它应用于实际代码。下一练习将在此基础上继续。

## 提交说明文件并推送分支

说明文件与存储库中的其他资源一样，也需要使用相同的源代码管理方式进行维护。现在将更改提交并推送到存储库中的分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

推送分支后，接下来创建拉取请求，并将其关联到待办事项中的文档标准 issue。可以手动完成，也可以让 Copilot 代劳：提示它查找 issue，创建用于关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已经将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录即可。后续练习会介绍这种连接所用的 Model Context Protocol。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选中 **Agent**。
3. 让 Copilot 查找与更新说明文件相关的 issue，并从当前分支创建 PR，说明其中既更新了说明文件，也添加了新的 publisher 辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 将开始查找 issue 并创建 PR。
5. 如果 Copilot 提示你**允许**它代表你执行 GitHub 操作，请检查命令，并酌情选择 **Allow**。
6. 创建 PR 后，提示 Copilot 合并 PR 并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示你允许它代表你执行 GitHub 操作和运行 shell 命令，请检查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件中获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下操作：

- 发送代码生成提示，观察 Copilot 在现有说明下生成的内容；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 再次运行相同提示，观察建议的代码如何遵循新标准；
- 将说明文件更新和筛选功能的数据层改动提交到 `main`，供下一练习继续使用。

接下来，在[代理模式][next-lesson]中运用这些说明，让 Copilot 跨存储库添加新功能。

## 资源

- [自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更出色的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
