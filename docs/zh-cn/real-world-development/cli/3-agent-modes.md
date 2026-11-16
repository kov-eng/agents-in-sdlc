---
title: "课程 3 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，通过 Autopilot 根据问题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。但较大的变更需要更完善的流程。幸运的是，GitHub Copilot CLI 能融入组织现有的工作流，帮助我们以正确的方式构建正确的功能。接下来的几课将演示典型的代理驱动开发流程：从问题开始创建功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 在继续开发功能的过程中，你会使用同一个对话和分支。通常，不同文件类型的工作会放在不同分支或 PR 中，但为了专注于核心概念，本练习会采用简化流程。

本课将学习如何：

- 从 GitHub 问题启动新的 Copilot 对话。
- 在 Plan 模式下定义要求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理，并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商筛选游戏。待办问题描述了这项功能，但在编码之前，还需要确定类别组合等细节。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在明确范围内实现功能。

## 背景

将 AI 编码代理引入开发流程，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发者的流程大致如下：

1. 从描述需求的问题开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述方式类似。

遵循这种标准方法，可以确保 AI 生成的代码满足既定要求，并经过与手写代码相同的审核流程。

## 对话模式

**对话模式**决定代理拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在各模式间切换：

- **Interactive**：你与代理协作。代理提出修改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并不断迭代，直到任务完成。

先从 Plan 模式开始，审查计划，然后使用 Autopilot 实现计划。

## 从问题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有问题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的问题。
9. 按 <kbd>C</kbd> 将问题添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 问题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划可以先确定实现功能或执行任务的方法，再交给 Copilot。对于复杂任务，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的问题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳方案。

3. 过程中，Copilot 可能会询问筛选功能应该如何工作。根据你的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已编写并审查，现在开始实现。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已解决。

1. 选择 **接受计划并使用 Autopilot 构建（推荐）**，或当前版本中类似的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

代码生成后，和其他代码一样，必须在合并前审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 根据问题和规划时的决定审查结果。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败项。

## 试用新功能

代码看起来不错，但它能运行吗？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **在浏览器中打开**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的组合方式。
4. 确认重置和无结果时的行为符合问题描述及你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并审查功能。本课中，你：

- 从 GitHub 问题启动了新的 Copilot 对话。
- 在 Plan 模式下定义了要求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，[使用自定义说明][next-lesson]深入了解如何让代码遵循书面规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
