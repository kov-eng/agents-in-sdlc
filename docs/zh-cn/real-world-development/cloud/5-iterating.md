---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在整个实验中，你与 GitHub Copilot 协作完成了多项任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换的无障碍功能。现在一起查看代码变更，必要时向 Copilot 提供反馈以改进成果。

### 场景

正如前面多次强调的，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。现在先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的变更，然后再为团队其他成员启用审查。

## 安全和 GitHub Copilot 云端代理

由于 Copilot 云端代理会在没有监督的情况下异步执行任务，因此必须设置一些安全限制：

- Copilot 对存储库只有读取权限，对代码分支只有写入权限。
- 云端代理在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云端代理创建的第一个拉取请求（PR），其中包含代码文档。你将在 GitHub.com 上使用标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。你也可以[自定义或禁用 Copilot 云端代理的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似于 **Add missing documentation** 或更完整描述的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会有 **[WIP]** 标记。此时请等待 Copilot 完成工作。可能需要几分钟，可以稍作休息，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查变更。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。确切的变更会有所不同。

    查看变更时，留意 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应该会在 PR 的检查部分排队。顺利的话，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像 @ 团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论并启动新会话来处理反馈。由于结果具有非确定性，我们无法提供固定的提示文本。可以要求 Copilot 更新的内容包括：

- 在每个代码文件顶部添加简要说明其用途的注释头。
- 在 TypeScript 和 Astro 文件中添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他希望在代码库中看到的文档修改。
2. 选择 **View Session**，观察 Copilot 执行修改。注意 Copilot 会启动新会话来更新内容。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成变更后，PR 中应该会出现新的提交。
5. 选择 **Files changed** 选项卡，审查变更。

可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿转换为可审查状态，然后合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为“在游戏详情页显示相关游戏”问题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或更完整描述的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应该会在 PR 的检查部分排队。顺利的话，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：** 你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，你可以通过迭代与 Copilot 云端代理协作，请它更新功能。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或更完整描述的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会有 **[WIP]** 标记。此时请等待 Copilot 完成工作，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现时，重点关注：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式变更。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应该会在 PR 的检查部分排队。顺利的话，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：** 你可以切换到 Codespace 中的此分支，手动测试无障碍功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能符合预期，并且刷新页面后设置仍然保留。

请注意，无障碍自定义代理如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍方面的问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云端代理适合处理待办事项，让你能把精力放到其他工作上。可以在自己的存储库中再创建几个问题并分配给 Copilot，以培养这种工作习惯。以下是一些想法：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云端代理练习系列。在这些练习中，你：

- **检查了存储库附带的自定义说明文件**，以便之后观察它们对云端代理输出的影响。
- **将问题分配给 Copilot 云端代理**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义代理**处理无障碍问题，添加高对比度和浅色模式切换。
- **使用 Copilot 代理页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代优化云端代理的拉取请求**，标记 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成云端代理练习系列。若想继续学习，其他练习模块可以补充本次内容：

- 🖥️ **[VS Code 练习模块](../../vscode/)** — 在 IDE 中体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 练习模块](../../cli/)** — 从终端交付两个经过审查的变更，体验 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端代理桥接。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率问题分配给云端代理。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云端代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云端代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
