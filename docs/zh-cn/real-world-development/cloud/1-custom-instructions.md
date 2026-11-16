---
title: "练习 1 - 自定义说明（云端代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云端代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]可提供指导，让 Copilot 不仅了解你希望它做什么，也了解如何完成任务。

本练习将学习如何：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义说明和按路径限定的说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习模块不同，这里不会运行“前后对比”提示。Copilot 云端代理会异步处理 GitHub 问题，因此难以实时并排展示效果。稍后审查云端代理生成的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 和优秀的开发团队一样，制定了开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应以 TSDoc 文档注释的形式在代码中添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

通过使用说明文件，可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它们能引导 Copilot 提供更相关的建议和代码片段。你可以指定编码规范、库，以及偏好的注释形式。说明可应用于整个存储库，也可以针对特定文件类型，为具体任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建。例如，针对 TypeScript 或 Astro，或者创建 UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云端代理在生成代码时都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 最后，Copilot 也支持使用其他标准的说明文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本练习的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码示例说明推荐或不推荐的写法。

说明文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实践，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的说明文件。查看本项目中的文件时，你可能会发现有适用于多种任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同入口的操作方式有所不同，例如 VS Code 中的 **配置 Chat → 生成代理说明**，或 Copilot CLI 中的 `/init`。相关练习会在适用时介绍具体操作。
>
> 想找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：其中包含一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中的文件。请注意，其中有针对 Astro 文件、Drizzle 数据层、测试等的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了决定说明适用文件的 glob。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目中创建单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆成较小的可复用文件，并为 Copilot 提供可参考的示例。（其中的路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求添加代码内文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要进行本练习唯一的编辑：添加一条项目级规则，要求以 TSDoc 文档注释和文件级注释头的形式在代码中记录文档。之后，云端代理会在本练习系列的后续任务中读取此规则。

编辑前，先创建一个分支（云端代理会读取问题目标分支中的说明，但你仍会将自己的编辑整洁地提交）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码规范，要求生成 TSDoc 文档注释和文件注释头。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分描述了项目编码标准，但尚未要求添加代码内文档，因此生成的辅助函数没有文档注释。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 正如上一课所学，说明文件可以放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供说明。刚才添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并说明更新

云端代理会读取问题目标分支中的说明。下一课将问题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将说明更新合并到 `main`，云端代理才能使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中分配问题后，云端代理就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支，直接提交到 `main`。此处设置分支是为了让工作坊更贴近真实项目的处理方式。

## 总结和后续步骤

你探索了 Copilot 如何读取本项目的说明文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。接下来的练习中，你会看到云端代理生成的拉取请求如何遵循此标准。

接下来，[将第一个问题分配给 Copilot 云端代理][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
