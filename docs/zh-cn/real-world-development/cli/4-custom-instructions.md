---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "查看存储库说明、添加文档规范，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相关指导。[说明文件][instruction-files]不仅描述你希望编写什么代码，也说明代码应该如何组织。现在筛选功能已经完成，你将查看 Copilot 使用的说明、添加文档规范，并将其应用于代码。

本课将学习如何：

- 了解存储库说明和按路径限定的说明文件如何提供给代理。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

使用说明文件，可以确保 Copilot 了解这些实践要求并据此完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它们能引导 Copilot 提供更相关的建议和代码片段。你可以指定编码规范、库，以及偏好的注释形式。说明可应用于整个存储库，也可以针对特定文件类型，为具体任务提供上下文。

有两种说明文件：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明文件

入门项目已包含一组说明文件。先查看现有内容，再进行修改，以了解修改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件，了解项目简介和编码指导。这些说明适用于此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。请注意，其中有针对 Astro 文件、Drizzle 数据层、测试等的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了决定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆成较小的可复用文件，并为 Copilot 提供可参考的示例。

## 更新说明文件以符合团队指南

现有文件是不错的起点，但仍有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 将以下内容添加为该部分的最后一条：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能对话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查已更改的 TypeScript 文件。查看新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何读取存储库说明，并将新的规范应用到了功能中。具体来说，你：

- 了解了存储库说明和按路径限定的说明文件如何提供给代理。
- 更新了说明文件，确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
